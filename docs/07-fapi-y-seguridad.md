# 07 · FAPI y seguridad de APIs financieras

**Fecha de corte: 10 de septiembre de 2026**

> **FAPI** = *Financial-grade API*, familia de perfiles de seguridad de la
> **OpenID Foundation (OIDF)**. No es una API: es un **perfil restrictivo de OAuth 2.0 y
> OpenID Connect** que elimina las opciones inseguras y fija las obligatorias.
>
> Es el estándar de seguridad de facto de open banking en el mundo: Reino Unido, Brasil,
> Australia (CDR), FDX (EE.UU./Canadá), Berlin Group (UE) — **y ahora Colombia**, que lo
> exige explícitamente en el proyecto de Capítulo IX de la Circular Básica Jurídica.

---

## 1. Estado de las especificaciones (OIDF FAPI Working Group)

### Finales (*Final Specifications*)
| Especificación | Notas |
|---|---|
| **FAPI 2.0 Security Profile** | Aprobada como Final en **febrero de 2025**. La recomendación oficial para ecosistemas nuevos |
| **FAPI 2.0 Attacker Model** | Aprobada como Final junto con el Security Profile. Define el modelo formal de amenazas contra el que se verifica el perfil |
| **FAPI 2.0 Message Signing** | Final (revisión pública y votación completadas en 2025). Firma y verificación de solicitudes/respuestas — para no repudio |
| **FAPI 1.0 Part 1: Baseline** | Perfil base, menor exigencia |
| **FAPI 1.0 Part 2: Advanced** | El perfil que usan UK, Brasil y Australia hoy |
| **JARM** | *JWT Secured Authorization Response Mode* |

### Implementer's Drafts
| Especificación | Uso |
|---|---|
| **FAPI-CIBA Profile** | Flujos desacoplados (autenticación en otro dispositivo). Base conceptual de la *Jornada Sem Redirecionamento* brasileña |
| **Grant Management** | Gestión de consentimientos basada en estándar. Nació de los requisitos de PSD2 y de Australia |

> **Posición oficial de la OIDF:** los ecosistemas **nuevos deben adoptar FAPI 2.0**; los
> que están en FAPI 1.0 deben **planificar la transición**.
> Colombia, al ser nuevo, hace lo correcto exigiendo 2.0 directamente.

## 2. FAPI 2.0 Security Profile — requisitos normativos

### Tokens *sender-constrained* (obligatorio)
El servidor de autorización **solo debe emitir tokens vinculados al remitente**, mediante:
- **mTLS** — *Mutual-TLS Client Certificate-Bound Access Tokens* (RFC 8705), o
- **DPoP** — *Demonstrating Proof-of-Possession* (RFC 9449)

Elimina la clase de ataque de **token robado y reutilizado**: el token solo sirve a quien
posee la clave privada asociada.

### PAR obligatorio
El AS **debe soportar** *Pushed Authorization Requests* con cliente autenticado y
**debe rechazar** solicitudes de autorización enviadas sin PAR. Los parámetros sensibles
nunca viajan por el canal frontal (navegador).

### PKCE con S256
Obligatorio **incluso para clientes confidenciales**. Sustituye a `state` + `s_hash` como
mecanismo anti-CSRF.

### Autenticación de cliente — solo dos métodos
- **mTLS** (RFC 8705), o
- **`private_key_jwt`** (OpenID Connect Core)

El AS **debe rechazar** un JWT cuyo `aud` no sea exactamente su identificador de emisor
como *string*.

### Requisitos del Authorization Server
- Publicar metadatos vía **OpenID Discovery**.
- **Rechazar** el grant *Resource Owner Password Credentials*.
- Soportar **únicamente clientes confidenciales**.
- **Códigos de autorización con vida máxima de 60 segundos**.
- Incluir el parámetro **`iss`** en las respuestas de autorización (**RFC 9207**) —
  mitiga *mix-up attacks*.
- Aceptar JWT con marcas de tiempo de hasta **10 segundos en el futuro** (tolerancia de reloj).

### Requisitos del Resource Server
- Aceptar el token en el **encabezado HTTP** (RFC 6750 Bearer o RFC 9449 DPoP).
- **Nunca** aceptarlo en parámetros de *query*.
- Verificar validez, integridad, expiración y **estado de revocación**.

### Qué cambió respecto de FAPI 1.0 Advanced

| Aspecto | FAPI 1.0 | FAPI 2.0 | Razón |
|---|---|---|---|
| Integridad de la solicitud | **JAR** (request object firmado) | **PAR** | Mejor interoperabilidad, menos complejidad |
| Respuesta de autorización | **JARM** o `code` + `id_token` | Solo **`code`** | Se elimina el ID token del canal frontal |
| Protección CSRF | `state` + `s_hash` | **PKCE** | Mecanismo más robusto y ya universal |
| Validación de ID Token | Requerida | Innecesaria | El token solo viaja por canal posterior |
| Redirect URI | Preregistrados | Enviados en **PAR** | Flexibilidad, protegida por autenticación |

> **Resumen:** FAPI 2.0 es **más simple y más seguro** que 1.0. Menos piezas
> criptográficas en el canal frontal, más restricción en el posterior. Migrar de 1.0 a 2.0
> no es solo cambiar una versión: cambia el flujo.

## 3. El stack completo de una implementación FAPI 2.0

```
┌─────────────────────────────────────────────────────────────┐
│  Tercero Receptor (cliente OAuth confidencial)              │
│  · Certificado mTLS o clave privada para private_key_jwt    │
│  · PKCE S256 · valida iss (RFC 9207)                        │
└───────────────┬─────────────────────────────────────────────┘
                │ (1) PAR autenticado  → request_uri
                │ (2) /authorize?client_id&request_uri
                ▼
┌─────────────────────────────────────────────────────────────┐
│  Authorization Server del Proveedor de Datos                │
│  · Autenticación fuerte del Titular (SCA equivalente)       │
│  · Pantalla de consentimiento / CONFIRMACIÓN (Colombia)     │
│  · code con TTL ≤ 60 s                                      │
│  · Token sender-constrained (mTLS o DPoP)                   │
└───────────────┬─────────────────────────────────────────────┘
                │ (3) code → /token (mTLS | private_key_jwt)
                │ (4) access_token vinculado
                ▼
┌─────────────────────────────────────────────────────────────┐
│  Resource Server (APIs de datos)                            │
│  · Token en header · verifica binding, expiración, revocación│
│  · OpenAPI 3.1 · JSON · REST · ISO 20022                     │
│  · Logs por solicitud (Colombia: 5 años)                    │
└─────────────────────────────────────────────────────────────┘
```

RFC y specs a tener a mano:

| Pieza | Referencia |
|---|---|
| OAuth 2.0 | RFC 6749 |
| PKCE | RFC 7636 |
| Bearer tokens | RFC 6750 |
| mTLS client auth + certificate-bound tokens | RFC 8705 |
| PAR | RFC 9126 |
| DPoP | RFC 9449 |
| `iss` en respuesta de autorización | RFC 9207 |
| JAR | RFC 9101 |
| Token introspection / revocation | RFC 7662 / RFC 7009 |
| JWT / JWS / JWA | RFC 7519 / 7515 / 7518 |

## 4. Lo que exige Colombia

> ✅ **FAPI 2.0 ya es obligatorio en Colombia desde febrero de 2024.** No es una novedad del
> proyecto de 2026. La **Circular Externa 004 de 2024**, numeral **3.2.3 literal a)** del
> Capítulo IX del Título I de la Parte I de la Circular Básica Jurídica, dice literalmente:
> *"Cumplir con el marco FAPI 2.0 desarrollado por The OpenID Foundation (OIDF) para los
> perfiles de seguridad."*
>
> El plazo para adoptarlo venció el **7 de agosto de 2026** (18 meses originales + 6 de la
> CE 009 de 2025 + 6 de la CE 001 de 2026).

### 4.1 Lo vigente — Circular Externa 004 de 2024, numeral 3.2

**Arquitectura (3.2.1):** intercambio en formato **JSON**; cumplir el marco de referencia
**REST** y que la implementación sea **RESTful**.

**Administración de datos (3.2.2):** cumplir el estándar **ISO 20022** en lo relacionado con
el diccionario de datos y **utilizar el diccionario de campos** que establece ese estándar,
en aquellos campos financieros que corresponda.

**Seguridad (3.2.3):**
- a) **Cumplir con el marco FAPI 2.0** de la OpenID Foundation para los perfiles de seguridad.
- b) Autorización sobre **OAuth 2.0**, con mecanismos seguros de emisión del Access Token
  (Client Credentials RFC 6749, Authorization Code RFC 6749, Authorization Code con PKCE
  RFC 7636 o Refresh Token RFC 6749). El token debe generarse como **JWT**, firmarse con
  **PS256 o superior**, y usar **`private_key_jwt`** como método de autenticación.
- c) Intercambio bajo **TLS con autenticación mutua**, con certificados digitales vigentes
  conforme a la **Ley 527 de 1999**, usando `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` o
  `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`.
- **Cláusula de obsolescencia:** si el organismo que soporta un formato, marco o protocolo
  lo declara obsoleto, la entidad debe adoptar el que lo sustituya.

**Principios generales (3.1):** red interna **lógicamente separada**; monitoreo; no exponer
públicamente los repositorios; **logs de auditoría por 5 años** por cada solicitud, con
origen, momento del consumo, usuario, información circulada y estado del proceso,
enmascarados o cifrados según criticidad; redundancia, balanceo de carga y tolerancia a fallos.

### 4.2 Lo que añadiría el proyecto de 2026

> ⚠️ Texto de un **proyecto de circular externa** publicado para comentarios del
> 24-jul-2026 al 11-ago-2026. **Aún no expedido** a la fecha de corte.
>
> Mantiene FAPI 2.0, OAuth 2.0, PS256+, `private_key_jwt`, mTLS y las mismas cipher suites.
> **Añade**: OpenAPI 3.1, ACORD/ISIN/CFI/OpenFunds/FIX para seguros e inversión, pruebas de
> calidad de datos con criterios **DAMA**, gestión de vulnerabilidades con **OWASP API
> Security Top 10 / NIST SP 800-204 / CSA**, y validación periódica del intercambio por API.

### Arquitectura (5.2.1)
- Documentación y diseño de APIs con **OpenAPI versión 3.1**.
- Intercambio en formato **JSON**.
- Cumplir el marco **REST** y que la implementación sea **RESTful**.

### Administración de datos (5.2.2)
- Respuestas basadas en elementos y componentes de mensaje de **ISO 20022**.
- Para **seguros** y **administradores de activos de terceros**, se admiten:
  **ACORD**, **ISIN**, **CFI**, **OpenFunds** o **FIX**, según aplique.

### Seguridad (5.2.3)
- **Cumplir el marco FAPI 2.0** de la OpenID Foundation para los perfiles de seguridad.
- Autorización sobre **OAuth 2.0** (IETF OAuth WG), con mecanismos seguros de emisión de
  *Access Token*: **Client Credentials** (RFC 6749), **Authorization Code** (RFC 6749),
  **Authorization Code con PKCE** (RFC 7636) o **Refresh Token** (RFC 6749).
- Access Token generado como **JWT**, **firmado con PS256 o superior**, y con
  **`private_key_jwt`** como método de autenticación.
- **TLS con autenticación mutua** usando certificados digitales vigentes conforme a la
  **Ley 527 de 1999**, restringido a dos suites de cifrado:
  - `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
  - `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- **Cláusula de obsolescencia**: si el organismo que soporta un estándar lo declara
  obsoleto, la entidad debe adoptar el que lo sustituya. (Esto ata a Colombia
  automáticamente a la evolución del FAPI WG — muy bien diseñado.)

### Requisitos generales de infraestructura (5.1)
- Segmentación, aislamiento y protección de componentes del sistema.
- **No exponer públicamente** los repositorios de información usados para el desarrollo.
- **Logs por cada solicitud, conservados 5 años**, que permitan determinar como mínimo:
  origen de la solicitud, momento del consumo, usuario que la ejecutó, información
  objeto de circulación y estado del proceso. Enmascarados o cifrados según criticidad.
- Disponibilidad continua: **redundancia, balanceo de carga y tolerancia a fallos**.
- **Pruebas de calidad de datos** con los atributos de *precisión, completitud,
  actualización y pertinencia*, siguiendo criterios del estándar **DAMA**, ejecutadas por
  un área designada dentro del Sistema de Control Interno, documentadas y reportadas.
- Procedimientos y herramientas de **validación periódica** del intercambio por API.
- Gestión de vulnerabilidades de API con marcos como **OWASP API Security Top 10**,
  **NIST SP 800-204** y **Cloud Security Alliance (API Security)**.

### Requisitos exigibles a Terceros Receptores No Vigilados (numeral 6.2)
Los Proveedores de Datos deben verificar que el no vigilado:
- Esté inscrito en el **Registro Nacional de Bases de Datos** (Decreto 886 de 2014) cuando
  aplique; si no está obligado, que tenga políticas de tratamiento de datos personales.
- Tenga procedimientos de atención de consultas y reclamos, y de actualización, revocatoria
  y supresión de la autorización.
- Cumpla los **estándares comunes de obligatoria adopción**, incluidos los de infraestructura.
- Gestione riesgos operacionales y de continuidad.
- Tenga **capacidad humana, técnica y financiera** para responder por sanciones de
  protección de datos o responsabilidad civil frente a consumidores.
- Gestione riesgos de seguridad de la información y ciberseguridad usando marcos como
  **ISO 27001**, **NIST CSF** u **OWASP ASVS**.
- Cuente con **certificación PCI-DSS** emitida por un **QSA** y soportada por el **AoC**,
  si va a almacenar, procesar o transmitir datos de tarjetas débito o crédito.
- Mantenga los datos **cifrados en almacenamiento y circulación**, con estándares que
  ofrezcan al menos la seguridad de **AES** o **RSA**.
- Notifique en el menor tiempo posible cualquier evento que comprometa la seguridad.

Además: las políticas de vinculación deben ser **aprobadas por la junta directiva** y
estar **publicadas en la web** del Proveedor de Datos, y se aplican las medidas de
**conocimiento y debida diligencia SARLAFT** previstas para clientes (arts. 102 y ss. EOSF,
Cap. IV, Tít. IV, Parte I de la CBJ).

## 5. Observación crítica: la brecha FAPI 2.0 vs. lo que se especifica

> Este defecto **está en la norma vigente** (CE 004 de 2024) y **se arrastra idéntico** al
> proyecto de 2026. No es un problema del borrador: es un problema que lleva dos años en firme.

La norma dice "cumplir el marco FAPI 2.0" **y luego** enumera detalles que son propios de
FAPI 1.0 o directamente incompatibles con FAPI 2.0:

| Punto | Lo que dice la norma (CE 004/2024 y proyecto 2026) | Lo que exige FAPI 2.0 | Comentario |
|---|---|---|---|
| Grants permitidos | Menciona **Client Credentials** entre los "mecanismos seguros" | Client Credentials no es un flujo de acceso a datos del titular | Confusión de capas: sirve para APIs sin titular (p. ej. catálogo de productos), no para datos personales |
| Autenticación de cliente | **`private_key_jwt`** | `private_key_jwt` **o mTLS** | El proyecto parece imponer solo uno de los dos |
| Sender-constrained tokens | **No lo menciona** | **Obligatorio** (mTLS o DPoP) | Es el requisito central de FAPI 2.0 y no aparece |
| **PAR** | No lo menciona | **Obligatorio** | Idem |
| Firma del token | PS256 o superior | FAPI 2.0 no exige que el access token sea JWT firmado | Requisito adicional colombiano, compatible |
| Cipher suites | Solo dos suites RSA-ECDHE | FAPI 2.0 no restringe a esas dos | Las suites listadas **no incluyen TLS 1.3** ni ECDSA — puede volverse un problema operativo |

**Recomendación concreta:** pedir que el texto se remita a la especificación FAPI 2.0
**por referencia**, sin reenumerar requisitos parciales. Reenumerar crea contradicción entre
el marco citado y el detalle. En la práctica, una entidad que implemente **FAPI 2.0 puro**
(con PAR y tokens sender-constrained) cumple la norma con holgura; una que implemente solo
la lista literal del numeral 3.2.3 **no cumple FAPI 2.0**, pese a que la norma lo exige.

> Nota sobre las **cipher suites**: son suites de **TLS 1.2**. FAPI 2.0 y las buenas
> prácticas actuales apuntan a **TLS 1.3**. Si la circular se expide literalmente así, una
> entidad que solo soporte TLS 1.3 quedaría técnicamente fuera de norma. Vale la pena
> señalarlo.

## 6. Certificación de conformidad

La OpenID Foundation opera **suites de pruebas de conformidad** gratuitas y
autoadministradas para FAPI 1.0 y **FAPI 2.0 Security Profile y Message Signing**.

- Brasil y Australia hacen la certificación **obligatoria antes de producción**.
- Colombia previó un **"espacio de pruebas"** (art. 2.35.8.4.2) que la SFC **podrá**
  habilitar — es facultativo, no obligatorio.

> 🔴 **Riesgo:** sin *conformance testing* obligatorio como requisito de inscripción en el
> directorio, cada entidad interpretará el estándar a su manera y la interoperabilidad
> real no ocurrirá. Es la lección más consistente de UK, Brasil y Australia.

## 7. Checklist de implementación

- [ ] AS con **PAR** obligatorio y rechazo de `/authorize` sin `request_uri`
- [ ] **PKCE S256** exigido a todos los clientes
- [ ] Tokens **sender-constrained**: mTLS (RFC 8705) o DPoP (RFC 9449)
- [ ] Autenticación de cliente: **mTLS** y/o **`private_key_jwt`**; nunca `client_secret_*`
- [ ] `iss` en la respuesta de autorización (**RFC 9207**)
- [ ] `code` con TTL **≤ 60 s**, un solo uso
- [ ] Rechazo explícito de ROPC y de clientes públicos
- [ ] Metadatos publicados vía **OpenID Discovery** (`/.well-known/openid-configuration`)
- [ ] Token **nunca** en query string
- [ ] Introspección/revocación con verificación de estado en cada llamada crítica
- [ ] **mTLS** contra certificados de la PKI reconocida (Ley 527 de 1999)
- [ ] **OpenAPI 3.1** publicado y versionado
- [ ] Modelo de datos alineado a **ISO 20022**
- [ ] API de **consentimiento** con ciclo de vida explícito y dashboard para el titular
- [ ] Soporte del **doble consentimiento** colombiano (autorización + confirmación)
- [ ] **Logs de 5 años** con los 5 campos exigidos, cifrados o enmascarados
- [ ] Pruebas de calidad de datos con criterios **DAMA**, documentadas
- [ ] Gestión de vulnerabilidades con **OWASP API Top 10**, **NIST SP 800-204**, **CSA**
- [ ] Certificación de conformidad **FAPI 2.0** de la OIDF (aunque aún no sea obligatoria)
