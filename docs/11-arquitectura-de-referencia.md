# 11 · Arquitectura de referencia para Colombia

**Fecha de corte: 10 de septiembre de 2026**

> Blueprint técnico derivado de: el Decreto 0368 de 2026, el Decreto 0977 de 2026, el
> proyecto de Capítulo IX de la CBJ, FAPI 2.0, y las implementaciones de Brasil y Reino Unido.
> **Los estándares funcionales colombianos aún no existen** — esto es lo que se puede
> construir hoy sin riesgo de reproceso mayor.

---

## 1. Vista de componentes

```
┌────────────────────────── ECOSISTEMA ───────────────────────────────┐
│                                                                     │
│   ┌──────────────────┐         ┌──────────────────────────────┐     │
│   │  SFC             │         │  PKI (Ley 527 de 1999)       │     │
│   │  · Directorio    │◄───────►│  · Certificados de           │     │
│   │    (3 módulos)   │         │    transporte (mTLS)         │     │
│   │  · Estándares    │         │  · Certificados de firma     │     │
│   │  · Espacio de    │         └──────────────────────────────┘     │
│   │    pruebas       │                                              │
│   │  · Indicadores   │                                              │
│   └────────┬─────────┘                                              │
│            │ metadata, roles, tarifas, claves públicas              │
│   ┌────────┴──────────────────────┬──────────────────────────┐      │
│   ▼                               ▼                          ▼      │
│ PROVEEDOR DE DATOS       TERCERO RECEPTOR          TERCERO RECEPTOR │
│ (entidad vigilada)        VIGILADO                 NO VIGILADO      │
│                            (derecho de acceso)     (acuerdo bilat.) │
└─────────────────────────────────────────────────────────────────────┘
```

## 2. Arquitectura interna del Proveedor de Datos

```
                    Internet (mTLS obligatorio)
                             │
              ┌──────────────▼──────────────┐
              │  API Gateway / Edge         │
              │  · Terminación mTLS         │
              │  · Validación de cert. vs   │
              │    directorio SFC           │
              │  · Rate limiting por TRD    │
              │  · WAF (OWASP API Top 10)   │
              │  · Medición para tarifa     │
              └──────────────┬──────────────┘
                             │
      ┌──────────────────────┼──────────────────────┐
      ▼                      ▼                      ▼
┌───────────┐      ┌──────────────────┐    ┌─────────────────┐
│ AS FAPI   │      │ Consent Service  │    │ Resource APIs   │
│ · PAR     │◄────►│ · Autorización   │    │ · /accounts     │
│ · PKCE    │      │ · CONFIRMACIÓN   │    │ · /transactions │
│ · mTLS/   │      │ · Revocación     │    │ · /credit       │
│   DPoP    │      │ · Dashboard      │    │ · /kyc          │
│ · code    │      │ · Trazabilidad   │    │ · /products     │
│   ≤60 s   │      └────────┬─────────┘    │ · /portability  │
└─────┬─────┘               │              └────────┬────────┘
      │                     │                       │
      └─────────────────────┼───────────────────────┘
                            ▼
              ┌──────────────────────────────┐
              │  Capa de datos + Audit Log   │
              │  · Mapeo a ISO 20022         │
              │  · Pruebas de calidad DAMA   │
              │  · Logs 5 años (5 campos)    │
              │  · Cifrado AES/RSA           │
              └──────────────────────────────┘
                            │
              ┌─────────────▼────────────────┐
              │  Core bancario / orígenes    │
              └──────────────────────────────┘
```

## 3. El flujo de doble consentimiento (lo distintivo de Colombia)

Colombia exige **dos actos separados** (arts. 2.35.8.3.2 y 2.35.8.3.3). Este es el punto
de diseño más delicado del modelo: mal implementado, duplica la fricción y mata la conversión.

```
Titular          Tercero Receptor        Proveedor de Datos
   │                    │                        │
   │ 1. Solicita servicio                        │
   ├───────────────────►│                        │
   │                    │                        │
   │ 2. AUTORIZACIÓN (art. 2.35.8.3.2)           │
   │    · id del tercero (razón social+domicilio)│
   │    · datos autorizados                      │
   │    · tratamiento                            │
   │    · finalidad específica                   │
   │    · tiempo de la finalidad                 │
   │◄──────────────────►│                        │
   │  [se genera ARTEFACTO DE CONSENTIMIENTO]    │
   │                    │                        │
   │                    │ 3. PAR autenticado     │
   │                    │    (+ artefacto)       │
   │                    ├───────────────────────►│
   │                    │                        │
   │ 4. AUTENTICACIÓN FUERTE (art. 2.35.8.3.4)   │
   │◄────────────────────────────────────────────┤
   │                    │                        │
   │ 5. CONFIRMACIÓN (art. 2.35.8.3.3)           │
   │    Muestra el contenido mínimo de la        │
   │    autorización y permite AUTORIZAR o NEGAR │
   │◄────────────────────────────────────────────┤
   │ ──────────── autoriza ─────────────────────►│
   │                    │                        │
   │                    │ 6. code (≤60 s)        │
   │                    │◄───────────────────────┤
   │                    │ 7. token sender-constr.│
   │                    │◄──────────────────────►│
   │                    │ 8. GET /transactions   │
   │                    ├───────────────────────►│
```

### Claves de diseño
- **El artefacto de consentimiento debe ser un objeto estructurado y firmado**, no texto
  libre. Propuesta: JSON con los 5 campos del art. 2.35.8.3.2, firmado como **JWS con PS256**
  por el Tercero Receptor, y transportado como *claim* dentro del **PAR**. Esto permite que
  el Proveedor renderice la pantalla de confirmación con exactamente el contenido que el
  Titular ya vio, sin reinterpretación. Modelo de referencia: **DEPA/India**.
- **La confirmación (paso 5) debe fusionarse con la autenticación (paso 4)** en una sola
  pantalla del Proveedor. Si se hacen en dos pantallas, se pierde al usuario.
- El proyecto de Capítulo IX (numeral 7) **prohíbe explícitamente** interferir, obstaculizar,
  añadir validaciones "innecesarias, redundantes o duplicadas", o usar advertencias y
  diseños que generen incertidumbre sobre la legitimidad del sistema. Es una regla
  anti-*dark-patterns*: el Proveedor no puede usar la confirmación para desincentivar.

## 4. Modelo de datos y APIs

### Dominios y endpoints propuestos

| Dominio | Endpoints base | Categoría del art. 2.35.8.2.1 | Autorización |
|---|---|---|---|
| **Productos (catálogo)** | `/products/credit`, `/products/deposits`, `/products/insurance`, `/products/investments` | 3 | ❌ No requiere |
| **Consentimientos** | `/consents`, `/consents/{id}` (POST/GET/DELETE) | transversal | — |
| **Cuentas** | `/accounts`, `/accounts/{id}/balances`, `/accounts/{id}/transactions` | 1 | ✅ Sí |
| **Crédito** | `/credit-operations`, `/credit-operations/{id}/payments` | 1 | ✅ Sí |
| **Depósito a término** | `/term-deposits` | 1 | ✅ Sí |
| **Vinculación (KYC)** | `/customer`, `/customer/identification`, `/customer/financial-relations` | 2 | ✅ Sí |
| **Portabilidad** | `/portability/certificates` (POST solicitud, GET certificado) | 4 (D-0977) | ✅ Sí |
| **Eventos** | `/webhooks`, notificaciones de revocación e incidentes | transversal | — |

### Reglas de modelado
- **OpenAPI 3.1** como fuente de verdad; generación de SDK y de *mocks* desde ahí.
- Payloads mapeados a **componentes de mensaje ISO 20022**
  (p. ej. `CashAccount40`, `ActiveOrHistoricCurrencyAndAmount`, `PartyIdentification`).
- Para seguros: **ACORD**. Para inversión: **ISIN**, **CFI**, **OpenFunds**, **FIX**.
- **Versionado por dominio** (`/open-finance/accounts/v1/...`), no global — permite
  evolucionar cada categoría al ritmo del cronograma de la SFC.
- **Mínimo 12 meses** de historial transaccional en depósitos a la vista, o la totalidad
  si la vinculación es menor.
- **No exponer datos derivados** (parágrafo 5 del art. 2.35.8.2.1): scores, segmentos,
  modelos. Sí exponer el dato base.

## 5. Requisitos no funcionales

| Requisito | Fuente | Meta sugerida |
|---|---|---|
| **Disponibilidad** | Proyecto Cap. IX 5.1 (redundancia, balanceo, tolerancia a fallos) | ≥ 99,8% mensual (benchmark UK: 99,80%) |
| **Latencia** | — | p95 ≤ 500 ms; p99 ≤ 1.500 ms (benchmark UK: 349 ms promedio) |
| **Logs** | Proyecto Cap. IX 5.1 | **5 años**, con: origen, momento del consumo, usuario, información circulada, estado del proceso. Enmascarados/cifrados según criticidad |
| **Calidad de datos** | Proyecto Cap. IX 5.1 | Precisión, completitud, actualización, pertinencia — criterios **DAMA**. Pruebas periódicas, documentadas, ejecutadas por área del SCI |
| **Cifrado** | Proyecto Cap. IX 6.2 | AES/RSA o superior, en reposo y en tránsito |
| **Gestión de vulnerabilidades** | Proyecto Cap. IX 5.1 | **OWASP API Security Top 10**, **NIST SP 800-204**, **CSA API Security** |
| **Aislamiento** | Proyecto Cap. IX 5.1 | Segmentación de componentes; **no exponer repositorios públicamente** |
| **Medición para tarifa** | Art. 2.35.8.3.9 | Contador por Tercero Receptor, por volumen de consultas, auditable |

## 6. Roadmap de construcción (sin esperar a los estándares)

### Fase 0 — Ahora (Q3–Q4 2026)
Trabajo que **no depende** de los estándares de la SFC:

- [ ] **Gateway con mTLS** y validación de certificados Ley 527 de 1999
- [ ] **Authorization Server FAPI 2.0**: PAR + PKCE S256 + tokens *sender-constrained*
      (mTLS o DPoP) + `private_key_jwt` + `iss` (RFC 9207) + `code` TTL 60 s
- [ ] **Consent Service** con ciclo de vida completo y artefacto firmado
- [ ] **Audit log** de 5 años con los 5 campos exigidos
- [ ] Certificación de conformidad **FAPI 2.0** en la suite de la OpenID Foundation
- [ ] **API de catálogo de productos** (categoría 3, no requiere autorización) — el
      *quick win* de menor riesgo, útil desde el día uno

### Fase 1 — Cuando salga el estándar de portabilidad (~2027)
- [ ] Endpoint de **certificado de portabilidad** con los 6 campos del art. 2.35.8.8.9
- [ ] Motor de **estudio de portabilidad** y generación de **oferta irrevocable**
- [ ] Orquestación de garantías: cesión/subrogación, registro de garantías mobiliarias
      (Ley 1676 de 2013), libranzas, cambio de beneficiario de seguros
- [ ] Gestión de plazos: 3 días hábiles (certificado), 5/30 días calendario (decisión),
      5 días hábiles (aceptación)

### Fase 2 — Datos del titular (según cronograma)
- [ ] `/accounts` + `/transactions` con 12 meses de historial
- [ ] `/credit-operations`
- [ ] `/customer` (KYC reutilizable)

### Fase 3 — Iniciación de pagos
- [ ] Seguir la vía del **art. 2.17.4.1.3** y del **Banco de la República**
      (art. 104 Ley 2294 de 2023), que puede ir por delante del cronograma de la SFC

## 6-bis. Certificados y algoritmos para mTLS

> Base: CE 004 de 2024, numeral 3.2.3 lit. c) (`fuentes-primarias/circular-externa-004-2024-anexo.pdf`,
> página 3 del Capítulo IX). El contenido mínimo del certificado **no ha sido definido por la SFC**;
> quedará en los lineamientos del directorio (plazo 7/10-oct-2027, ampliado por el D-1489). Lo demás es práctica de UK/Brasil.

### Dos pares de llaves, no uno

| Llave | Función | Emisor | Algoritmo |
|---|---|---|---|
| Certificado **mTLS** | Autenticar el canal y vincular tokens al cliente (`cnf.x5t#S256`, RFC 8705) | **ECD acreditada por ONAC** (Ley 527/1999; Decreto-ley 19/2012 art. 160; Decreto 333/2014) | RSA |
| Llave de **firma** (JWK en `jwks_uri`) | `private_key_jwt` al solicitar tokens | Propia | PS256 |

No reutilizar el mismo par para ambas: la rotación de una rompería la otra.

### Contenido recomendado del certificado mTLS (X.509 v3)

| Campo | Contenido |
|---|---|
| `Subject` | `CN` = FQDN · `O` = razón social · `serialNumber`/`organizationIdentifier` = **NIT** · `C` = CO |
| `SAN` | Todos los FQDN |
| `Key Usage` | `digitalSignature`, `keyEncipherment` |
| `EKU` | `clientAuth`; `serverAuth` si además se exponen APIs |
| `Basic Constraints` | `CA:FALSE` |
| `AIA` / `CDP` | OCSP y CRL de la ECD |
| Llave | **RSA ≥ 2048 (3072 recomendado)** |
| Firma | `sha256WithRSAEncryption` |
| Vigencia | ≤ 1 año, rotación automatizada |

El **NIT en el subject** es el ancla natural para cruzar contra el módulo de receptores del directorio
(art. 2.35.8.5.3 num. 2). Brasil usa el CNPJ con el mismo fin.

mTLS es bidireccional: el cliente valida también el certificado del banco contra la cadena de la ECD,
verifica revocación por OCSP y hace *pinning* de la CA, nunca del certificado hoja.

### Algoritmos por capa

| Capa | Exigido / recomendado |
|---|---|
| Suites TLS (norma) | `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` · `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384` (preferir 256) |
| Llave del certificado | **RSA obligatoriamente** — `ECDHE_RSA` no negocia con certificados ECDSA |
| Versión TLS | Soportar **1.2** con esas suites (cumplimiento literal) **y 1.3** (FAPI 2.0 / industria) |
| Tokens JWT | **PS256** (HS256 rechazado por la SFC) |
| Datos en reposo | AES-256 |

> ⚠️ **Gotcha:** solicitar a la ECD un certificado **ECDSA** —la recomendación moderna por
> rendimiento— deja a la entidad **fuera de norma**, porque ninguna de las dos suites permitidas
> lo acepta. Especificar RSA al pedir el certificado.

## 7. Decisiones técnicas que conviene tomar ya

| Decisión | Opciones | Recomendación |
|---|---|---|
| **Sender-constrained tokens** | mTLS (RFC 8705) vs DPoP (RFC 9449) | **mTLS**: ya es obligatorio en Colombia para transporte, y es lo que usan Brasil, UK y Australia. DPoP como opción secundaria |
| **Base del estándar funcional** | Diseñar desde cero vs adaptar Open Finance Brasil vs adaptar OBL v4 | **Open Finance Brasil** — mismo contexto regulatorio, mismo perfil FAPI, mismo modelo de directorio, y ya resolvió el ciclo de vida del consentimiento |
| **Producto de AS** | Construir vs Keycloak vs comercial certificado FAPI | Comercial o Keycloak **con certificación FAPI 2.0 vigente**. No construir un AS FAPI propio |
| **Transporte del artefacto de consentimiento** | Claim en PAR vs API separada | **Claim firmado (JWS) en el PAR** — evita una llamada extra y garantiza integridad |
| **TLS** | 1.2 (suites de la circular) vs 1.3 | Soportar **ambas**: 1.2 con las dos suites listadas (cumplimiento literal) y 1.3 (buena práctica). Comentar el punto a la SFC |
| **Versionado** | Global vs por dominio | **Por dominio** — el cronograma de la SFC libera categorías a distinto ritmo |

## 8. El Directorio de participantes: cómo se consume (Colombia vs. Brasil, UK, UE, Chile)

> **Fecha de corte de esta sección: 9 de noviembre de 2026.** Colombia: el Directorio es
> **norma vigente en su diseño general pero sin lineamientos**; todo lo que sigue sobre su
> *consumo técnico* en Colombia es **análisis, no norma**. Los datos de Brasil se observaron
> el 9-nov-2026 en el endpoint público; los de UK, UE y Chile vienen de documentación
> pública y de resultados de búsqueda (marcados como tales).

### 8.1. Qué es (y qué no es) el Directorio colombiano

- **Qué:** "herramienta de consulta" que identifica a los participantes en cada rol,
  administrada por la SFC (D-0368, art. 2.35.8.5.1). **No** lista a todas las vigiladas: solo
  a quienes participan en finanzas abiertas.
- **Tres módulos** (art. 2.35.8.5.3): (1) Proveedores — identificación, costo de uso de la
  infraestructura, contacto y *"datos necesarios para el acceso"*; (2) Receptores —
  identificación, contacto y *"datos necesarios para su autenticación"* por el Proveedor;
  (3) Vinculaciones voluntarias — Proveedor, Tercero No Vigilado, fecha y estado.
- **Quién llena qué:** la SFC verifica estándares *antes* de inscribir a vigilados (art.
  2.35.8.5.4) y tramita solicitudes en máximo 1 mes (art. 2.35.8.5.5). Un **No Vigilado no se
  inscribe por sí mismo**: aparece cuando un Proveedor registra la vinculación (art.
  2.35.8.5.7).
- **Qué falta:** el proyecto de Capítulo IX remite los lineamientos a una circular posterior
  (`fuentes-primarias/proyecto-capitulo-IX-CBJ-2026.txt`, Sección III). El Documento Técnico de
  la URF de 2026 (`fuentes-primarias/urf-documento-tecnico-ajuste-plazos-2026.txt`, §4.3) dice
  que está **por definir** si será solo registral o si incorporará capacidades de
  *identificación, autenticación y conexión* para reducir los acuerdos bilaterales, y que
  la SFC buscará cooperación con la CMF (Chile), el Banco Central de Brasil y la FCA.
- **Plazo:** **7/10-oct-2027** (18 meses; antes 12), por el Decreto 1489 de 2026.
- **Visión de 2024 (descartada en parte):** el documento técnico de la URF de 2024 (§6) lo
  describía como plataforma pública que expone certificados digitales, *endpoints* de API y
  contactos, con un módulo de **Terceros de Confianza** que **no quedó** en el decreto final.
- No confundir con el **DICE** del Banco de la República (DSP-470), que es el directorio
  centralizado de **llaves de Bre-B**, otro sistema.

### 8.2. Cómo lo hacen otros

| Jurisdicción | Rol del directorio | Consumo técnico |
|---|---|---|
| **Brasil** — Diretório de Participantes (Open Finance Brasil) | Ancla de confianza del ecosistema: fuente única de participantes y software acreditados | **API pública sin autenticación**: `https://data.directory.openbankingbrasil.org.br/participants` devolvió JSON (≈4,4 MB; 105 organizaciones activas y 219 servidores de autorización en el archivo del 9-nov-2026; no se verificó que sea el listado completo del ecosistema). Por organización: razón social, CNPJ, roles (`CONTA`, `DADOS`, `PAGTO`, `CCORR`); por servidor de autorización: `Issuer`, `OpenIDDiscoveryDocument`, URI del JWKS, `SupportsDCR`, `SupportsCiba`, APIs con familia y versión, certificaciones con vigencia y portal de desarrolladores. Además un *keystore* público con JWKS (`keystore.directory.openbankingbrasil.org.br/openbanking.jwks`) |
| **Reino Unido** — Open Banking Directory (OBL) | Emite credenciales de software y firma el SSA | La fintech sube certificados eIDAS-like (OBWAC/OBSeal), crea un *software statement* y obtiene un **SSA** (JWT firmado por el directorio) que presenta a cada banco por **DCR**. Hay *directory sandbox* (guía de uso v2.5, feb-2025) |
| **UE** — registro EBA (PSD2) | **Solo informativo**: agrega los registros de las autoridades nacionales; no incluye bancos. Lo mantiene la EBA desde 18-mar-2019 | No es el canal de autenticación. La identidad técnica viaja en el **certificado cualificado** (QWAC/QSealC, ETSI TS 119 495) con autoridad nacional, número de autorización y roles (`PSP_AI`, `PSP_PI`, `PSP_AS`, `PSP_IC`). *No se verificó cómo contrasta cada banco el certificado con el registro* |
| **Chile** — Directorio de Participantes (CMF) | La CMF lo administra (NCG 514 de 2024, ajustada por la NCG 569) | Según presentaciones de la CMF (búsqueda, sin lectura del texto): pruebas a inicios de 2026 y, en agosto de 2026, aún en construcción |

**Flujo típico en Brasil y UK (resumen):** (1) el Receptor registra su software en el
directorio; (2) el directorio emite un SSA firmado (PS256); (3) el Receptor obtiene su
certificado cliente, cuyo identificador referencia el software registrado; (4) presenta el SSA al
Transmisor por DCR; (5) el Transmisor valida firma y certificado contra el directorio. Fuente:
guías del Open Finance Brasil y de OBL, ver `docs/99`.

### 8.3. Lectura para Colombia (análisis, no norma)

1. **Tres modelos posibles:** *registral* (como la UE: lista pública, la confianza viaja en el
   certificado), *ancla de confianza* (Brasil/UK: el directorio firma y alimenta el registro
   de clientes) o un híbrido. Los campos "datos necesarios para el acceso/autenticación" del
   art. 2.35.8.5.3 apuntan al segundo, pero la SFC no ha decidido.
2. **Diseñar con el directorio como dependencia sustituible:** el AS debe poder alimentar su
   *trust store* y su registro de clientes tanto desde un directorio con API como desde una
   lista cargada a mano. Un *adaptador* aísla ese cambio.
3. **Casos que el diseño debe cubrir** aunque aún no existan los endpoints: **suspensión o
   retiro** de un participante (art. 2.35.8.5.5 num. 5 y 6) → revocar acceso rápido y cachear
   con caducidad corta; **cambio de datos** → refresco periódico; y el **NIT** como llave de
   cruce con el certificado mTLS (ver §6-bis).
4. **Oportunidad regulatoria:** los lineamientos saldrán por circular (tope 7/10-oct-2027). Es el
   momento de pedir, vía los canales de la SFC, un directorio **legible por máquina** y
   público estilo Brasil, con SSA o equivalente y estado de certificación.
5. **Para una fintech no vigilada:** sin vinculación de un Proveedor, no figura en el módulo 3;
   el directorio no sustituye el acuerdo bilateral.
