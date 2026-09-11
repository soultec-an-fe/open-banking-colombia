# 00 · Resumen ejecutivo

**Fecha de corte: 10 de septiembre de 2026**

---

## 1. La foto global en una línea

Open Banking dejó de ser un experimento regulatorio: **Reino Unido** está migrando de
open banking a **open finance** con hoja de ruta a 2030; la **UE** cerró PSD3/PSR y tiene
FIDA estancada; **EE.UU.** está en un limbo judicial con la regla §1033 suspendida y en
reescritura; **Brasil** es el ecosistema obligatorio más grande del mundo (100M+ de
clientes); y **Colombia** acaba de saltar del modelo voluntario al obligatorio.

## 2. Colombia — lo que cambió en 2026

| Norma | Fecha | Qué hace |
|---|---|---|
| **Decreto 0368 de 2026** | 7-abr-2026 (rige 10-abr-2026) | Sustituye el Título 8, Libro 35, Parte 2 del Decreto 2555 de 2010. Crea el **Sistema de Finanzas Abiertas obligatorio**. Deroga el esquema voluntario del Decreto 1297 de 2022. |
| **Decreto 0977 de 2026** | 4-ago-2026 | Añade el **Capítulo 8** — *portabilidad financiera* como servicio dentro del sistema de finanzas abiertas (consumo, hipotecario incl. leasing habitacional, comercial). |
| **Circular Externa 004 de 2024** | 7-feb-2024, **vigente** | Crea el Capítulo IX de la CBJ. **Ya exige FAPI 2.0**, OAuth 2.0, ISO 20022, mTLS, `private_key_jwt`, PS256+. Plazo final de adopción: **7-ago-2026** |
| **Proyecto de Circular Externa** (Cap. IX CBJ) | Consulta jul–ago 2026, **no expedida** | Subroga el Capítulo IX. Mantiene FAPI 2.0 y **añade** OpenAPI 3.1, DAMA, OWASP API Top 10 |
| **Proyecto de Carta Circular** (cronograma) | Consulta **hasta 15-sep-2026** | Fija el orden y los plazos de expedición de estándares. |

### Los tres actores del modelo colombiano

1. **Proveedor de Datos** — entidad vigilada por la SFC que expone la información.
   Para la lista del art. 2.35.8.2.3 la participación es **obligatoria**.
2. **Tercero Receptor de Datos Vigilado** — entidad vigilada por la SFC. Acceso
   obligatorio y no discriminatorio si cumple los estándares y está en el directorio.
3. **Tercero Receptor de Datos No Vigilado** — persona jurídica no vigilada (fintech,
   por ejemplo). Solo entra por **acuerdos bilaterales voluntarios** con cada Proveedor.

> ⚠️ **Esto es lo más importante para una fintech no vigilada**: el decreto **no** te da
> derecho de acceso. Te da un marco de no discriminación y transparencia, pero la puerta
> sigue siendo bilateral. El proyecto de decreto original de la URF sí contemplaba un
> "Tercero Confiable" y reciprocidad; **ambos se eliminaron** en el texto final.

### Alcance de datos (art. 2.35.8.2.1)

1. Información de **productos y servicios a nombre del Titular** — incluye saldo, uso,
   condiciones particulares e **historial transaccional de mínimo 12 meses** para
   depósitos a la vista.
2. Información del **proceso de vinculación** (KYC).
3. **Características generales** de productos y servicios ofrecidos (no requiere
   autorización del titular — es información pública comparativa).
4. **Servicio de portabilidad financiera** (añadido por el D-0977/2026).

**Fuera del alcance:** la información *derivada* del análisis, procesamiento o
transformación de esos datos (scores, segmentaciones, modelos). El dato base sí debe
compartirse; el derivado no.

### Doble consentimiento

Colombia adopta un esquema poco común: **autorización** al Tercero Receptor
(art. 2.35.8.3.2) **+ confirmación** ante el Proveedor de Datos (art. 2.35.8.3.3) antes
de que circule la información. La SIC objetó esta doble verificación; el Gobierno la
mantuvo. Diseñar el UX de esto es un reto real (ver
[`11-arquitectura-de-referencia.md`](11-arquitectura-de-referencia.md)).

### Costos

Los datos son **gratuitos**. El Proveedor puede cobrar solo por **uso de infraestructura**,
por volumen de consultas, con factores objetivos, medibles, verificables y en igualdad
de condiciones. La tarifa se publica en el módulo de proveedores del directorio.

---

## 3. Tablero de fechas Colombia

| Fecha | Hito | Base legal |
|---|---|---|
| 7-ago-2026 | 🔴 **Venció** el plazo final de la CE 004/2024 — incluida la adopción de **FAPI 2.0**. Sin tercera prórroga | CE 001 de 2026 |
| **15-sep-2026** | 🔴 **Cierre de comentarios al cronograma** | Proyecto de Carta Circular |
| **10-oct-2026** | 🔴 **Fecha límite para que la SFC publique el cronograma** | D-0368/2026, art. 4.1 |
| ~dic-2026 | La SFC debe incluir portabilidad en el cronograma (2 meses después) | D-0977/2026, art. 5 |
| 7-abr-2027 | Adecuación de modelos existentes al nuevo Capítulo IX (según proyecto) | Proyecto CE, 2.3 |
| **10-abr-2027** | 🔴 Directorio de participantes operativo + indicadores definidos | D-0368/2026, art. 4.2 y 4.3 |
| ~jun-2027 | Estándar de portabilidad de crédito de consumo (mes 8 del cronograma) | Borrador cronograma |
| ~ago-2028 | Tope legal para estándares de portabilidad | D-0977/2026, art. 6 |
| ~2035–2038 | Historial transaccional e iniciación de pagos (meses 84–132 + 12 de implementación) | Borrador cronograma |

## 4. Los tres riesgos que hay que vigilar

1. **El cronograma borrador es extremadamente largo.** La iniciación de pagos queda a
   **132 meses** (11 años) de la publicación del cronograma, y el historial transaccional
   de depósitos a la vista a 96 meses. Si se confirma así, el "open banking" transaccional
   colombiano no llega antes de 2035. → [análisis](09-colombia-cronograma-y-analisis.md)
2. **Posible inconsistencia normativa**: la portabilidad comercial aparece en el mes 24
   del cronograma (~oct-2028), pero el art. 6 del D-0977/2026 impone un tope de 24 meses
   desde su vigencia (~5-ago-2028). El cronograma podría incumplir su propia habilitante.
3. **Asimetría competitiva para no vigilados** *en finanzas abiertas*: sin derecho de acceso
   ni reciprocidad, el poder de negociación queda en los Proveedores de Datos. El único
   contrapeso son los deberes de no discriminación del art. 2.35.8.6.2 y la vigilancia de la
   SFC/SIC. **Nota:** en **iniciación de pagos** la situación es la opuesta — una sociedad
   no vigilada **sí puede** desarrollar la actividad y **sí tiene** derecho de acceso
   (arts. 2.17.4.1.1 y 2.17.4.1.2). Ver [`14`](14-iniciacion-de-pagos-colombia.md).

## 5. Lo que hay que decidir para empezar a trabajar

Ver [`12-plan-de-trabajo.md`](12-plan-de-trabajo.md). Las decisiones abiertas son:

- ¿Nos posicionamos como **Proveedor de Datos**, **Tercero Receptor Vigilado**,
  **Tercero Receptor No Vigilado** o **proveedor de infraestructura** para terceros?
- ¿Construimos sobre el estándar colombiano (por definir) o adoptamos el
  **Open Finance Brasil** / **UK Open Banking Standard v4** como base y lo adaptamos?
- ¿Radicamos comentarios al cronograma antes del **15 de septiembre**?
