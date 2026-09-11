# 01 · Conceptos y modelos regulatorios

**Fecha de corte: 10 de septiembre de 2026**

---

## 1. El espectro: Open Banking → Open Finance → Open Data

| Nivel | Qué cubre | Ejemplos vivos |
|---|---|---|
| **Open Banking** | Cuentas de pago: datos de cuenta + iniciación de pagos | UK (CMA Order), UE (PSD2), Colombia fase inicial |
| **Open Finance** | Todo producto financiero: crédito, inversión, pensiones, seguros | Brasil, Chile (Ley Fintec), Colombia (D-0368/2026), UE (FIDA, propuesta) |
| **Open Data / Smart Data** | Cualquier sector: energía, telecom, retail, transporte | UK (Data (Use and Access) Act 2025), Australia (CDR multisectorial) |

Colombia usa deliberadamente el término **"finanzas abiertas"** (open finance) y el propio
decreto se define como *"un primer paso en la construcción de un esquema de datos
abiertos"* — es decir, apunta al tercer nivel.

## 2. Los dos componentes funcionales

Cualquier régimen se descompone en dos cosas distintas, con regulación distinta:

### a) Derechos de acceso a datos (*data access*)
El titular autoriza a un tercero a leer sus datos. Normalmente read-only. En Colombia
esto es el núcleo del Decreto 0368 de 2026.

### b) Iniciación de pagos (*payment initiation*)
El tercero ordena un pago desde la cuenta del titular. Requiere reglas adicionales de
autenticación, responsabilidad, irrevocabilidad y liquidación.

> En Colombia la iniciación de pagos vive en el **art. 2.17.4.1.3 del Decreto 2555 de 2010**
> (sistemas de pago de bajo valor), **no** en el Título 8. El D-0368/2026 solo modificó ese
> artículo para encargar a la SFC los estándares de iniciación de pagos "inmediatos o no
> inmediatos y recurrentes o no recurrentes", sin perjuicio de las facultades de la Junta
> Directiva del Banco de la República (art. 104 de la Ley 2294 de 2023).

## 3. Tres modelos regulatorios

### Modelo prescriptivo / regulatorio (top-down)
El regulador define alcance, estándares técnicos, plazos y obliga.
**Ejemplos:** Reino Unido (CMA Order 2017), Brasil (BCB), Australia (CDR), Colombia (2026).

- ✅ Interoperabilidad garantizada, cobertura completa del mercado, cronograma claro.
- ❌ Costoso, lento, riesgo de estándar mal diseñado, poca flexibilidad comercial.

### Modelo de mercado / bilateral (bottom-up)
Sin mandato: acuerdos bilaterales, agregadores, *screen scraping*.
**Ejemplo:** EE.UU. hasta la §1033 (y de facto hoy, con la regla suspendida).

- ✅ Rápido, se adapta a la demanda real.
- ❌ Fragmentación, asimetría de poder, dependencia de credenciales compartidas.

### Modelo híbrido
Mandato acotado + capa voluntaria/comercial encima.
**Ejemplos:** UE (PSD2 obligatorio + *premium APIs* comerciales), Reino Unido en 2026
(cVRP vía esquema voluntario UKPI), **Colombia** (obligatorio entre vigilados + esquemas
voluntarios con no vigilados).

> **Colombia es explícitamente híbrida.** Art. 2.35.8.2.3: obligación solo hacia Terceros
> Receptores **Vigilados**. Capítulo 6: los no vigilados entran por vinculación voluntaria.

## 4. Los cinco componentes de infraestructura de cualquier esquema

1. **Estándar de API** — contrato funcional (endpoints, payloads, versionado).
   → OpenAPI + REST/JSON es el consenso global. Diccionario de datos: ISO 20022.
2. **Perfil de seguridad** — cómo se autentica y autoriza.
   → **FAPI** (OpenID Foundation) es el estándar de facto mundial. Ver [`07`](07-fapi-y-seguridad.md).
3. **Directorio / registro central** — quién es quién, qué rol tiene, qué certificados usa.
   → UK: OB Directory. Brasil: Diretório de Participantes. Colombia: **Directorio de
     Participantes administrado por la SFC** (art. 2.35.8.5.1), con tres módulos.
4. **Confianza / PKI** — emisión y validación de certificados.
   → UK: eIDAS/OBWAC-OBSeal. Brasil: **ICP-Brasil**. Colombia: certificados digitales
     conforme a la **Ley 527 de 1999**.
5. **Gestión del consentimiento** — otorgar, consultar, actualizar, revocar; con
   trazabilidad y dashboard para el titular.
   → Colombia: art. 2.35.8.3.2 a 2.35.8.3.5, con **doble verificación**.

## 5. Modelo de gobernanza: quién manda

| Función | UK | Brasil | Colombia |
|---|---|---|---|
| Regulador | FCA / PSR / CMA | BCB | **SFC** + URF (proyecta norma) + MinHacienda (expide decreto) |
| Fija estándares | OBL → *Future Entity* | Estrutura Inicial de Governança | **SFC** (art. 2.35.8.4.1) |
| Opera el directorio | OBL | Governança OF Brasil | **SFC** (art. 2.35.8.5.1) |
| Protección de datos | ICO | ANPD | **SIC** (Ley 1581) + SFC (Ley 1266, dato financiero) |
| Esquema de pagos | Pay.UK / UKPI | BCB (Pix) | Banco de la República (art. 104 Ley 2294) + SFC |

> **Diferencia estructural importante:** en Reino Unido y Brasil el operador del estándar
> y del directorio es una **entidad separada del supervisor**. En Colombia **la SFC hace
> ambas cosas**, lo que concentra rol regulador + operador y le impone una carga
> institucional considerable — eso explica en parte lo conservador del cronograma.

## 6. Glosario rápido (equivalencias entre jurisdicciones)

| Concepto | UK / UE | Brasil | Colombia |
|---|---|---|---|
| Quien tiene los datos | ASPSP | Instituição Transmissora | **Proveedor de Datos** |
| Quien los consume | TPP (AISP/PISP) | Instituição Receptora | **Tercero Receptor de Datos** |
| El cliente | PSU | Cliente | **Titular** |
| Lectura de datos | AIS | Compartilhamento de dados | Acceso y suministro de datos personales |
| Iniciación de pago | PIS | Iniciação de Pagamento | Servicios de iniciación de pagos |
| Pago recurrente variable | VRP / cVRP | Pagamento recorrente / automático | (aún sin figura equivalente) |
| Portabilidad | CASS (cuentas) | Portabilidade de crédito | **Portabilidad financiera** (D-0977/2026) |
