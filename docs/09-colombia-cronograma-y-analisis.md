# 09 · Colombia — cronograma de estandarización y análisis crítico

**Fecha de corte: 23 de septiembre de 2026**

> ⚠️ **Todo este documento analiza un PROYECTO en consulta pública.**
> *Proyecto de Carta Circular: "Cronograma para la expedición de estándares de intercambio
> de información en el sistema de finanzas abiertas"*, publicado el **31 de agosto de 2026**.
> Comentarios cerraron el **15 de septiembre de 2026, 5:00 p.m.** — la SFC aún no expide
> versión definitiva (ver sección 7 sobre por qué se está demorando).

> ⚠️ **NO VIGENTE — nuevo desde el 22-sep-2026.** Un segundo proyecto, esta vez de
> **decreto** (no de circular), propone **extender 6 meses** todos los plazos de este
> cronograma. Ver sección 7. Mientras no se expida, las fechas de las secciones 1-6 de
> este documento (basadas en el D-0368/2026 vigente) siguen siendo las legales.

---

## 1. De dónde sale el cronograma

**Base legal:** art. 3 del Decreto 0368 de 2026 obliga a la SFC a publicar un cronograma que
incluya, **como mínimo**, los estándares para seis tipos de información/servicio:

1. Historial transaccional de los **depósitos a la vista** a nombre del Titular
2. Historial transaccional de los **productos de crédito** a nombre del Titular
3. Información asociada al proceso de **vinculación** del Titular como cliente
4. Información asociada a los productos de **depósito a término**
5. **Características generales** de los productos y servicios ofrecidos por las vigiladas
6. Estándares para los **servicios de iniciación de pagos** (art. 2.17.4.1.3)

A esto se suma, por el **art. 5 del Decreto 0977 de 2026**, la **portabilidad financiera**,
que la SFC debe incluir dentro de los 2 meses siguientes a la publicación del cronograma.

**Insumos usados por la SFC:**
- 7 mesas técnicas (junio–julio 2026) con 157 entidades.
- Diagnóstico de madurez del ecosistema.
- Priorización con metodología **IGO (Importancia y Gobernabilidad)**.
- Análisis interno de la SFC: necesidades de supervisión, capacidades institucionales y
  necesidades del sistema (parágrafo 2 del art. 3 del D-0368/2026).

> El propio proyecto aclara: la publicación del cronograma *"no comporta instrucciones
> aplicables a las entidades vigiladas, sino que obedece a la definición de una hoja de
> ruta para la expedición posterior de estándares vinculantes."*

## 2. El cronograma propuesto

Los plazos son **meses máximos desde la fecha de publicación del cronograma**.

| # | Tipo de información / servicio | Plazo (meses) | Fecha proyectada* |
|---|---|---|---|
| 1 | **Portabilidad financiera — crédito de consumo** | 8 | ~jun-2027 |
| 2 | **Portabilidad financiera — crédito hipotecario** | 16 | ~feb-2028 |
| 3 | **Portabilidad financiera — crédito comercial** | 24 | ~oct-2028 |
| 4 | Características generales de productos y servicios **de crédito** | 36 | ~oct-2029 |
| 5 | Características generales de productos y servicios **de captación** | 48 | ~oct-2030 |
| 6 | Características generales de productos y servicios **de seguros** | 60 | ~oct-2031 |
| 7 | Características generales de productos y servicios **de inversión** | 72 | ~oct-2032 |
| 8 | **Historial transaccional de productos de crédito** | 84 | ~oct-2033 |
| 9 | **Historial transaccional de depósitos a la vista** | 96 | ~oct-2034 |
| 10 | Información asociada al **proceso de vinculación** (KYC) | 108 | ~oct-2035 |
| 11 | Información asociada a **productos de depósito a término** | 120 | ~oct-2036 |
| 12 | **Servicios de iniciación de pagos** | **132** | **~oct-2037** |

\* Fechas calculadas asumiendo publicación del cronograma el **10 de octubre de 2026**
(fecha límite legal del art. 4.1 del D-0368/2026, contando desde la entrada en vigencia
del 10-abr-2026 según su art. 5). Si la SFC publica antes, todo se corre.
> ⚠️ Ver sección 7: el propio documento técnico de la URF (sep-2026) calcula este mismo
> tope como **7 de octubre de 2026**, y el proyecto de decreto en curso propone moverlo
> a **7 de abril de 2027**. Todas las fechas de esta tabla son **anteriores** a ese
> proyecto y deben leerse como el escenario "sin ampliación".

**El cronograma es revisable:** *"podrá ser objeto de revisiones posteriores, de acuerdo con
los desarrollos normativos, las capacidades de adopción del sistema"* y conforme al
parágrafo 2 del art. 3.

## 3. Cuándo estaría realmente disponible cada dato

A cada estándar hay que sumarle el plazo de **implementación** del art. 2.35.8.3.7:
**12 meses**, prorrogables **6** por una única vez, **+6 adicionales** para clientes persona
jurídica grande.

| Hito de negocio | Estándar | + implementación | **Disponible (mejor caso)** | **Peor caso (+6/+6)** |
|---|---|---|---|---|
| Portabilidad de consumo | jun-2027 | +12m | **jun-2028** | dic-2028 |
| Portabilidad hipotecaria | feb-2028 | +12m | **feb-2029** | ago-2029 |
| Comparador de productos de crédito | oct-2029 | +12m | **oct-2030** | abr-2031 |
| **Agregación de cuentas (historial de depósitos)** | oct-2034 | +12m | **oct-2035** | abr-2036 |
| KYC reutilizable / onboarding | oct-2035 | +12m | **oct-2036** | abr-2037 |
| **Iniciación de pagos** | oct-2037 | +12m | **oct-2038** | **abr-2039** |

## 4. Análisis crítico

### 🟠 Hallazgo 1 — Iniciación de pagos al mes 132: fragmentación, no bloqueo

El caso de uso que en **Brasil** (fase 3, integrado con Pix) y en **Reino Unido** (VRP/cVRP)
es **el motor económico del ecosistema**, en el borrador colombiano queda **último**, a
132 meses del cronograma.

> ⚠️ **Matiz verificado — y es determinante.** Esto **no significa** que la iniciación de
> pagos esté bloqueada 11 años. La actividad **ya está habilitada en Colombia desde julio de
> 2022** por el **Título 4 del Libro 17 de la Parte 2 del Decreto 2555 de 2010** (añadido por
> el art. 4 del Decreto 1297 de 2022), y ya opera sobre **Bre-B**. Incluso las **sociedades
> no vigiladas por la SFC pueden desarrollarla**, con **derecho de acceso** a los sistemas de
> pago de bajo valor. Ver [`14-iniciacion-de-pagos-colombia.md`](14-iniciacion-de-pagos-colombia.md).

Lo que el cronograma difiere son los **estándares comunes**. Las consecuencias reales:

- **Fragmentación por EASPBV.** Sin estándar de la SFC, cada entidad administradora define
  sus propias reglas operativas, técnicas y de seguridad en su reglamento
  (art. 2.17.4.1.2 num. 4). Un iniciador que opere en varias integra varias veces. Es el
  mismo problema que PSD2 tuvo en Europa por no imponer un estándar único de API.
- **Sin pagos recurrentes estandarizados.** El art. 2 del D-0368/2026 amplió el mandato a
  pagos *"recurrentes o no recurrentes"* — la puerta a un **VRP colombiano**. Sin estándar,
  no hay caso de negocio equivalente al del Reino Unido.
- **Autenticación sin reglas comunes.** El num. 3 del art. 2.17.4.1.3 remite a "las reglas
  que para el efecto dicte la SFC". Mientras no existan, cada emisora autentica a su manera.

**Reformulación del comentario a la SFC:** en vez de pedir que se adelante la fila completa,
pedir que se adelanten de forma **separada y prioritaria** (i) las reglas de **autenticación
y confirmación** del num. 3 del art. 2.17.4.1.3 y (ii) los estándares de **pagos recurrentes**.

### 🔴 Hallazgo 2 — Posible inconsistencia con el Decreto 0977 de 2026

| Norma | Qué exige | Fecha tope |
|---|---|---|
| D-0977/2026, **art. 6** | La SFC publica los estándares del **certificado de portabilidad** (art. 2.35.8.8.9) en máximo **24 meses** desde la vigencia del decreto (~5-ago-2026) | **~5 de agosto de 2028** |
| Borrador de cronograma, fila 3 | Portabilidad **comercial** a **24 meses** de la publicación del cronograma | **~10 de octubre de 2028** |

Si el cronograma se publica el 10 de octubre de 2026, **la portabilidad comercial vencería
unos dos meses después del tope legal** del decreto habilitante.

**Lecturas posibles:**
- (a) El art. 6 del D-0977 se refiere al estándar del certificado **como pieza única**, y
  bastaría con expedirlo antes de agosto de 2028 aunque los estándares por tipo de cartera
  se completen después.
- (b) Hay una inconsistencia real que debe corregirse adelantando la fila 3 o publicando el
  cronograma con suficiente antelación.

👉 **Este es un comentario concreto y bien fundamentado para radicar antes del 15-sep-2026.**

### 🟠 Hallazgo 3 — El orden invierte la lógica de Brasil y del Reino Unido

Brasil arrancó por **datos abiertos** (características de productos, sin cliente), lo más
fácil y de menor riesgo, y así construyó capacidad técnica antes de tocar datos personales.

El borrador colombiano arranca por **portabilidad de crédito**, que es:
- El caso de uso **políticamente más visible** (competencia, tasas, inclusión) ✅
- Pero también el **operativamente más complejo**: involucra garantías, libranzas, registro
  de garantías mobiliarias, cesión/subrogación, seguros asociados, titularizaciones y
  procesos con **terceros no vigilados** (notarías, registros).

Y deja las **características generales de productos** —la categoría que **no requiere
autorización del titular**, es decir, la más simple de todas— para los meses 36 a 72.

> **Comentario sugerido:** adelantar las características generales de productos (filas 4–7)
> como *quick win* de bajo riesgo, para que el ecosistema, el directorio y el espacio de
> pruebas se ejerciten con datos que no son personales antes de mover datos del titular.

### 🟠 Hallazgo 4 — No hay auditor de tarifas

> **Contexto de la URF:** su documento técnico reconocía la tensión explícitamente —
> *"si las tarifas de acceso son demasiado altas, la participación de nuevos competidores y
> el desarrollo de nuevos casos de uso se pueden ver limitados"*, y señalaba que los
> esquemas de tarifación por volumen o la negociación bilateral **"suponen una barrera o
> una desventaja para los participantes de menor tamaño"** (CGAP, 2024). También registraba
> que **Reino Unido y Estados Unidos prohíben cobrar** por el acceso, y que **Chile y Brasil**
> priorizan la gratuidad, permitiendo reembolso de gastos incrementales **solo al superar un
> umbral de llamadas API mensuales por cliente y por entidad**.
>
> Colombia adoptó la tarifación por volumen **sin umbral gratuito previo** y **sin auditor**.

La **SIC recomendó** (recomendación 1) establecer qué componentes son costos recuperables,
**quién supervisa y audita la razonabilidad de las tarifas**, y cuándo y cómo se reconocen.
El Gobierno **no acogió** la recomendación.

Resultado: cada Proveedor de Datos fija su tarifa de infraestructura, la publica en el
directorio, y el único control es el principio de **igualdad de condiciones** del
art. 2.35.8.3.9 num. 3 y la supervisión general de la SFC. Para un Tercero Receptor pequeño
esto es un riesgo económico real y difícil de impugnar.

### 🟠 Hallazgo 5 — Asimetría estructural para actores no vigilados

- Los **Vigilados** tienen derecho de acceso (art. 2.35.8.3.10: el Proveedor **no puede
  restringir**).
- Los **No Vigilados** **no tienen derecho de acceso**. Dependen de un acuerdo bilateral
  voluntario, con requisitos exigentes (ISO 27001 / NIST CSF / OWASP ASVS, PCI-DSS con QSA
  y AoC, RNBD, capacidad financiera, debida diligencia SARLAFT como si fueran clientes).
- El proyecto de decreto original de la URF contemplaba un **"Tercero Confiable"** y un
  **requisito de reciprocidad**; **ambos se eliminaron** en el texto final.

#### Qué decía la URF sobre lo que se eliminó

El **Documento Técnico "Sistema de finanzas abiertas obligatorio"** (URF, diciembre de 2024)
que sustentó el proyecto de decreto argumentaba:

**Sobre reciprocidad** — *"La ausencia de reciprocidad entre el consumo y la exposición de
información genera una dinámica anti-competitiva que limita el derecho de los consumidores
para suministrar acceso a su información personal."* La URF citaba a **Brasil y Australia**,
que sí incorporaron el principio, y proponía adoptarlo con la salvedad australiana: la
obligación de actuar como proveedor está sujeta a que el tercero **efectivamente tenga**
información dentro del alcance del esquema (un agregador de cuentas o un comparador de
productos no necesariamente la tiene).

**Sobre Terceros de Confianza** — la URF proponía reconocer a proveedores de servicios
conexos: acceso y autenticación de participantes, **API hubs**, diseño y desarrollo de APIs,
soporte técnico, y **tarifación o neteo de tarifas de acceso**. Con tres salvaguardas:
*desagregación de servicios* (cada uno con tarifa individual), *prohibición de
condicionamiento* (no atar un servicio a la contratación de otro) y *garantía de libre
competencia*.

**Nada de esto quedó en el Decreto 0368 de 2026.** El resultado es que el rol de API hub
—que en la práctica es como entran los actores pequeños en Brasil y Reino Unido— no tiene
reconocimiento normativo ni las salvaguardas antimonopolio que la URF había diseñado.

**Consecuencia práctica:** una fintech no vigilada que quiera operar a escala nacional debe
negociar y sostener acuerdos bilaterales con **decenas** de Proveedores de Datos. El
Capítulo 6 le da protección contra discriminación, no una llave de acceso.

**Alternativas de diseño que existen en el mundo:** los **niveles de acreditación** del CDR
australiano (*sponsored*, *affiliate*, *CDR representative*) resuelven exactamente esto
—permitir la entrada de actores pequeños bajo el patrocinio de uno acreditado— sin bajar la
barra de seguridad. Ver [`06-otros-mercados.md`](06-otros-mercados.md).

### 🟡 Hallazgo 6 — El espacio de pruebas es facultativo

El art. 2.35.8.4.2 dice que la SFC **"podrá"** habilitar un espacio de pruebas. En Brasil y
Australia la **certificación de conformidad es requisito para entrar en producción**.
Sin ese candado, el art. 2.35.8.5.4 (verificación previa a la inscripción) queda sin
mecanismo operativo definido.

## 5. Comentarios sugeridos para radicar antes del 15 de septiembre

| # | Instrucción objeto de comentario | Propuesta |
|---|---|---|
| 1 | Tabla de cronograma, fila "Servicios de iniciación de pagos" (132 meses) | Adelantar significativamente, o declarar explícitamente que la iniciación de pagos se desarrollará por la vía del art. 104 de la Ley 2294 de 2023 (Banco de la República) y no por el cronograma de la SFC |
| 2 | Tabla, fila "Portabilidad financiera crédito comercial" (24 meses) | Armonizar con el tope de 24 meses del art. 6 del Decreto 0977 de 2026, contado desde la vigencia de ese decreto |
| 3 | Tabla, filas 4–7 (características generales, 36–72 meses) | Adelantar como *quick win*: no requieren autorización del titular (parágrafo del art. 2.35.8.3.2) y permiten ejercitar directorio, estándares y espacio de pruebas con datos no personales |
| 4 | Tabla, fila "Historial transaccional de depósitos a la vista" (96 meses) | Adelantar: es el dato base de agregación de cuentas y de la evaluación de capacidad de pago para la Economía Popular, que es el objetivo declarado del art. 88 lit. e) de la Ley 2294 de 2023 |
| 5 | Cuerpo de la carta circular | Incluir **hitos intermedios verificables** por estándar (publicación de borrador, consulta, versión candidata, versión final) y no solo la fecha tope |
| 6 | Cuerpo de la carta circular | Comprometer la publicación del **espacio de pruebas** (art. 2.35.8.4.2) y la **certificación de conformidad** como requisito de inscripción en el directorio |

## 6. Tablero maestro de fechas

| Fecha | Hito | Fuente | Estado |
|---|---|---|---|
| 7-feb-2024 | CE 004 de 2024 | SFC | ✅ Vigente |
| 7-abr-2026 | Decreto 0368 de 2026 | MinHacienda | ✅ Expedido |
| 9-abr-2026 | Publicación Diario Oficial 53.453 | — | ✅ |
| **10-abr-2026** | **Entrada en vigencia del D-0368/2026** | Art. 5 | ✅ |
| jun–jul 2026 | 7 mesas técnicas, 157 entidades | SFC | ✅ |
| 24-jul a 11-ago-2026 | Consulta del proyecto de nuevo Capítulo IX (recibió 651 observaciones de 46 actores) | SFC | ✅ Cerrada |
| 4-ago-2026 | Decreto 0977 de 2026 (portabilidad) | MinHacienda | ✅ Expedido |
| 6-ago-2025 | CE 009 de 2025 — 1.ª prórroga (a 8-feb-2026) | SFC | ✅ |
| **10-ago-2026** | Sismo — Emergencia Económica, Social y Ecológica (Decreto 1261, 19-ago-2026), daños > $30 billones | Gobierno nacional | ✅ |
| 3-feb-2026 | CE 001 de 2026 — 2.ª prórroga (a 7-ago-2026) | SFC | ✅ |
| 7-ago-2026 | 🔴 Vence el régimen de transición de la CE 004/2024. **Sin tercera prórroga** | CE 001 de 2026 | ✅ Vencido |
| 31-ago-2026 | Publicación del proyecto de cronograma | SFC | ✅ |
| **15-sep-2026** | 🔴 Cierre de comentarios al cronograma | Proyecto de Carta Circular | ✅ Cerrado |
| **22-sep-2026** | Publicación del **proyecto de decreto de ampliación de plazos** (art. 4 D-0368 y art. 6 D-0977) | URF | ✅ |
| **27-sep-2026, 11:59 p.m.** | 🔴 **Cierre de comentarios al proyecto de decreto de ampliación** | URF | ⏳ **4 días** |
| **7 / 10-oct-2026** | 🔴 **Tope legal vigente para publicar el cronograma** (7-oct según cómputo de la URF desde la fecha del decreto; 10-oct según la vigencia del art. 5) — ver discrepancia en sección 7.1 | D-0368 art. 4.1 | ⏳ |
| ~dic-2026 | Tope para incluir portabilidad en el cronograma | D-0977 art. 5 | ⏳ |
| Pendiente | Expedición de la nueva Circular Externa (Cap. IX, antes Proyecto 010/2026) | SFC | ⏳ |
| +30 días de esa expedición | Entidades con casos de uso vigentes remiten **plan de adecuación** | Proyecto CE, 2.3 | ⏳ |
| 7-abr-2027 | Adecuación de modelos existentes al nuevo Capítulo IX | Proyecto CE, 2.3 | ⏳ |
| **7 / 10-abr-2027** | 🔴 **Directorio operativo + indicadores definidos** (vigente, sin ampliación) | D-0368 art. 4.2 y 4.3 | ⏳ |
| ~jun-2027 | Estándar de portabilidad de consumo | Borrador cronograma | ⏳ |
| ~5-ago-2028 | Tope legal vigente de estándares de portabilidad | D-0977 art. 6 | ⏳ |
| ~2034–2039 | Historial transaccional e iniciación de pagos | Borrador cronograma | ⏳ |

> Las filas en negrita con dos fechas separadas por "/" reflejan la discrepancia documentada
> en la sección 7.1. Si el proyecto de decreto de ampliación se expide, todas estas fechas
> se recalculan según la tabla de la sección 7.2.

## 7. Proyecto de decreto de ampliación de plazos (22-sep-2026) — NO VIGENTE

> **Fuente primaria:** `fuentes-primarias/urf-documento-tecnico-ajuste-plazos-2026.pdf`
> (Documento Técnico "Ajustes en los plazos de implementación del sistema de finanzas
> abiertas obligatorio", URF, septiembre de 2026, 22 páginas). Publicado a comentarios por
> la URF el **22-sep-2026**, cierra el **27-sep-2026, 11:59 p.m.**
> (`urf.gov.co/normatividad/proyectos-de-decreto/2026`).

El proyecto modifica el **art. 4 del Decreto 368 de 2026** y el **art. 6 del Decreto 977 de
2026** — es decir, toca los decretos mismos, no una circular de la SFC. **No modifica la
obligatoriedad del sistema, los roles de los participantes ni las disposiciones
sustantivas** del Decreto 2555 de 2010 incorporadas por esos dos decretos.

### 7.1. Discrepancia detectada: 7-oct-2026 vs. 10-oct-2026

El art. 5 del D-0368/2026 dice que el decreto *"rige a partir del día siguiente a su
publicación en el Diario Oficial"* (publicado 9-abr-2026 → vigencia **10-abr-2026**), lo que
da **10-oct-2026** para el tope de 6 meses del cronograma y **10-abr-2027** para el de
12 meses del Directorio/indicadores — así lo calculamos en las secciones 1-6 de este
documento.

Sin embargo, el propio **documento técnico de la URF** (pág. 5, sección 1) —el organismo que
redactó el decreto y ahora redacta su modificación— computa esos mismos plazos como
**vencidos el 7 de octubre de 2026** y **el 7 de abril de 2027**, contando aparentemente
desde la fecha de expedición/firma del decreto (7-abr-2026) y no desde su entrada en
vigencia (10-abr-2026). Es una discrepancia de 3 días en un documento oficial que sustenta
un cambio normativo — **pendiente de verificación** contra el texto final del nuevo decreto
cuando se expida. No hay manera de saber, sin ese texto, cuál de las dos fechas usará el
Gobierno como línea base para contar los nuevos plazos.

### 7.2. Por qué la URF pide justo 6 meses (justificación resumida)

1. **Interdependencia entre instrumentos.** El cronograma ordena el resto; el Directorio
   depende de que existan estándares; los indicadores dependen de que el sistema ya opere.
   Publicar cualquiera antes de tiempo obliga a rehacerlo.
2. **Evidencia ya recogida por la SFC:** 7 mesas técnicas con 157 entidades (jun-jul 2026);
   651 observaciones al proyecto de Circular Externa 010/2026; solicitudes de más tiempo en
   la consulta del propio cronograma (cerrada 15-sep-2026).
3. **El Directorio de Participantes es el instrumento más complejo** — falta definir si es
   solo registral o si tendrá funciones operativas de autenticación/conexión, y la SFC
   busca antes cooperar con la CMF (Chile), el BCB (Brasil) y la FCA (Reino Unido).
4. **Fortalecimiento institucional de la SFC** — consultorías, capacitación y talento
   regulatorio/tecnológico nuevo para asumir cronograma + Directorio + indicadores +
   coordinación con la SIC en datos personales, simultáneamente.
5. **Circunstancia sobreviniente: el sismo del 10 de agosto de 2026.** Daños estimados en
   **más de $30 billones**; Emergencia Económica, Social y Ecológica declarada por el
   Decreto 1261 del 19-ago-2026 (art. 215 C.P.). El documento técnico dice explícitamente
   que esto desvió capacidad técnica y jurídica de la SFC hacia la respuesta a la
   emergencia, compitiendo con la implementación de finanzas abiertas.

**Benchmark internacional citado por la URF para justificar que 6 meses es poco:** Reino
Unido tardó ~7 años (2016→2023) en cerrar su roadmap; Brasil pospuso su fase 1 y tardó
~5 años en llegar a portabilidad de crédito; **Chile —el caso más comparable— dio 24 meses
de transición y en junio de 2026 los extendió un año adicional.** Frente a eso, la URF
argumenta que pedir 6 meses en Colombia es "acotado y proporcionado".

### 7.3. Tabla de ajuste propuesto

| Instrumento | Plazo vigente | Vence hoy (según URF) | Plazo propuesto | Nuevo vencimiento propuesto |
|---|---|---|---|---|
| Cronograma de expedición de estándares | 6 meses | 7-oct-2026 | 12 meses | 7-abr-2027 |
| Directorio de Participantes | 12 meses | 7-abr-2027 | 18 meses | 7-oct-2027 |
| Indicadores de seguimiento | 12 meses | 7-abr-2027 | 18 meses | 7-oct-2027 |
| Estándares e información de portabilidad financiera | 24 meses | 4-ago-2028 | 30 meses | 4-feb-2029 |

*Fuente: Tabla 1 del documento técnico de la URF, pág. 18.*

### 7.4. Qué significa para el cronograma de este documento (secciones 1-6)

Si el decreto se expide tal como está el proyecto, el punto de partida del cronograma
(fila "fecha proyectada" de la sección 2) se mueve de oct-2026 a **abr-2027**, y **todas**
las fechas proyectadas de la sección 2 y 3 se corren ~6 meses hacia adelante. El **Hallazgo
2** de la sección 4 (posible inconsistencia entre portabilidad comercial a 24 meses del
cronograma vs. tope del art. 6 del D-0977) queda parcialmente atenuado si el tope de
portabilidad también se mueve a feb-2029, pero **no se resuelve del todo** sin ver el texto
final — sigue siendo un punto a vigilar, no a dar por cerrado.

**No se hicieron cambios a las secciones 1-6** de este documento: siguen describiendo el
escenario vigente hoy. Este documento se actualizará con fechas definitivas solo cuando el
decreto de ampliación se expida (o se archive) en el Diario Oficial.
