# 04 · Estados Unidos

**Fecha de corte: 10 de septiembre de 2026**

> **Titular:** la regla federal existe, está codificada en el CFR, **y no puede ser
> aplicada por el CFPB** porque un tribunal federal se lo prohibió. Mientras tanto, el
> mercado sigue funcionando con acuerdos bilaterales y los estados empiezan a legislar.

---

## 1. Base legal

**Sección 1033 de la Dodd-Frank Act (2010)** — otorga al consumidor el derecho a solicitar
y recibir, en formato electrónico, información sobre sus productos y servicios financieros.

Notas de alcance:
- La ley solo habla de **derechos de acceso a datos del consumidor**, no de pagos. Pero la
  regla final del CFPB **sí** exigió compartir la información suficiente para que un
  tercero autorizado pueda **iniciar pagos**, remitiendo las consecuencias aguas abajo a
  las reglas existentes de iniciación y procesamiento.
- La §1033 cubre *todos* los productos financieros de consumo, pero la regla final
  **restringió** la obligación a **tarjetas de crédito de consumo y cuentas Regulation E**,
  con posible expansión posterior.

## 2. La regla *Personal Financial Data Rights* (octubre 2024)

- Publicada en **octubre de 2024**, tras un proceso iniciado en 2016.
- **Entidades cubiertas**: depositarias con **US$850 millones o más** en activos y ciertos
  no bancos.
- **Datos exigibles**: transacciones de los **últimos 24 meses**, términos y condiciones de
  la cuenta, e información personal de la cuenta.
- **Prohibición total de cobros** a terceros por el acceso.
- Cumplimiento **escalonado de 2026 a 2030** según tamaño de la entidad.
- Reconoce **organismos de estandarización**: el CFPB reconoció a **FDX**
  (*Financial Data Exchange*) como el primero, en **enero de 2025**.

## 3. El litigio y la suspensión

| Momento | Qué pasó |
|---|---|
| oct-2024 | Se publica la regla; se demanda de inmediato |
| — | **Forcht Bank, Bank Policy Institute y Kentucky Bankers Association** demandan en el Distrito Este de Kentucky |
| 2025 | El CFPB cambia de posición: declara ante el tribunal que **considera la regla ilegal** y que debería anularse |
| — | Un tribunal federal de Kentucky **prohíbe al CFPB aplicar la regla**, al hallar que probablemente excedió su autoridad legal y fue arbitraria y caprichosa |
| — | Apelación ante el **Sexto Circuito**, hoy **suspendida** mientras el CFPB reescribe |
| **1-abr-2026** | La primera fecha de cumplimiento **pasa sin efecto vinculante** |

> ⚠️ **Matiz jurídico clave:** la orden judicial impide la aplicación **por el CFPB**.
> La regla sigue vigente en el CFR. Fiscales generales estatales y reguladores financieros
> estatales podrían argumentar que **sí pueden** ejercer sus poderes independientes bajo
> Dodd-Frank para hacerla cumplir.

## 4. La reescritura en curso

| Fecha | Hito |
|---|---|
| **22-ago-2025** | **ANPR** — *Personal Financial Data Rights Reconsideration*, Docket CFPB-2025-0037, 90 FR 40986 |
| **6-ago-2026** | El CFPB envía el **NPRM** a **OIRA** para revisión bajo la Orden Ejecutiva 12866 |
| pendiente | Publicación en el *Federal Register* y apertura de comentarios (OIRA suele tomar hasta 90 días) |

### Las cuatro preguntas del ANPR
1. Definición de "**representante**" del consumidor autorizado a acceder a datos.
2. Si los proveedores de datos deben poder **cobrar tarifas**.
3. Si los riesgos de **seguridad** están adecuadamente tratados (y su costo-beneficio).
4. Si los riesgos de **privacidad** están adecuadamente tratados.

### Lo que se anticipa del NPRM
- **Eliminar la prohibición total de tarifas**, permitiendo cobros **después de cierto
  número de solicitudes atendidas gratuitamente**.
- Posible redefinición de "consumidor" y "representante" para limitar quién accede.
- Posible flexibilización de restricciones de **uso secundario** de datos.
- Extensión de fechas de cumplimiento.

> El sector bancario argumenta que necesita recuperar del orden de **US$1.400 millones**
> en costos de infraestructura de APIs abiertas. Es exactamente la misma discusión que el
> art. 2.35.8.3.8 colombiano resuelve con "recuperación de costos de infraestructura".

## 5. Los estados entran a llenar el vacío

### Nueva York — el "mini-1033"
- **Assembly Bill A10640** (13-mar-2026) y **Senate Bill S9483** (17-mar-2026).
  Ambas **en comisión** a la fecha de corte.
- Consistente con la §1033 en el núcleo: exige **developer interface** legible por máquina,
  **prohíbe cobros** por acceso, limita las denegaciones a riesgos específicos y conocidos,
  exige **consentimiento expreso** al tercero y adherencia a **GLBA** y a los estándares de
  salvaguardas de la **FTC**.
- **Va más lejos que la regla federal en tres puntos:**
  1. **Más productos** — todos los productos y servicios financieros de consumo, sin la
     restricción a tarjetas de crédito y cuentas Regulation E.
  2. **Cuentas de pequeñas empresas** — la regla federal solo cubre consumidores.
  3. **Sanciones** — hasta **US$10.000 por infracción**, aplicadas por el
     *Superintendent of Financial Services* (NYDFS). Con miles de solicitudes por minuto,
     la exposición escala rápido.
- **Debilidad**: es mucho más corta que el expediente del CFPB — no precisa el alcance
  exacto de los datos, las excepciones, la base para denegar ni los requisitos técnicos
  de API. Eso se resolvería vía reglamentación o se volverá fuente de litigio.

### Consideraciones estatales más amplias
- **Preemption**: cualquier ley estatal que limite la capacidad de los bancos de cobrar
  por acceso a datos puede chocar con poderes bancarios federales.
- **UDAP y privacidad**: fiscales generales estatales están evaluando su autoridad de
  prácticas desleales o engañosas. California, por ejemplo, ya ha llevado acciones contra
  empresas que no atendieron solicitudes de acceso bajo la **CCPA**; la teoría se extiende
  naturalmente a solicitudes de acceso a datos financieros.

## 6. GUARD Financial Data Act (en el Congreso)

Modernizaría la **Gramm-Leach-Bliley Act**:
- Principios de **minimización de datos** — recolección y divulgación limitada a lo
  "adecuado, relevante y razonablemente necesario" para cada finalidad declarada.
- ⚠️ **Codificaría el *screen scraping*** (acceso basado en credenciales) como canal
  válido y paralelo a las APIs, en lugar de prohibirlo. Exigiría a los agregadores
  revelar los riesgos, ofrecer *opt-out*, y prohibiría a las instituciones negar acceso
  a quienes no ejerzan ese *opt-out*.
- Nuevos derechos del consumidor y obligaciones de aviso sobre **uso de IA** y retención.
- **Preempt** de leyes estatales de privacidad y seguridad de datos para entidades
  cubiertas por GLBA — un marco nacional único.

## 7. El mercado no espera

Bancos, prestamistas no bancarios, procesadores de pagos, empresas de datos de nómina y
fintechs siguen firmando **acuerdos bilaterales de acceso a datos**, directamente y con
agregadores. Estos contratos resuelven: asignación de responsabilidad, seguros,
indemnidades, derechos de auditoría, experiencia de cliente, requisitos de seguridad,
implementación técnica, SLA, uso de marcas, servicios de valor agregado y **tarifas**.

La industria bancaria está migrando activamente de *screen scraping* a **acceso por API
basado en OAuth**, donde el cliente se autentica directamente con su banco. Pero el
*scraping* con credenciales **sigue siendo prevalente**.

## 8. FDX — el estándar

**Financial Data Exchange**: consorcio de industria (bancos, fintechs, agregadores).
- Publica la **FDX API**, especificación REST/JSON con OAuth 2.0 y modelo de consentimiento.
- **Reconocido por el CFPB como organismo de estandarización** en enero de 2025.
- Adopción **desigual** — no hay obligación de usarlo mientras la regla esté suspendida.

---

## Lecciones para Colombia

| Lección | Aplicación |
|---|---|
| Un mandato sin base legal sólida es frágil | El D-0368/2026 se apoya en el art. 89 de la Ley 2294 de 2023 (PND) — habilitación legal expresa. Esa es su fortaleza frente al caso estadounidense |
| El conflicto de **tarifas** es inevitable | Colombia ya lo anticipó: datos gratis + recuperación de infraestructura por volumen. Falta el mecanismo de auditoría de razonabilidad (la SIC lo pidió y el Gobierno **no** acogió esa recomendación) |
| Sin mandato, gana el *screen scraping* | Colombia tiene la ventaja de partir de API obligatoria. Conviene prohibir o desincentivar explícitamente el acceso por credenciales compartidas |
| Fragmentación subnacional = costo | No aplica: Colombia es de regulación nacional única. Ventaja competitiva real |
