# 02 · Reino Unido

**Fecha de corte: 10 de septiembre de 2026**

El Reino Unido sigue siendo la referencia mundial: fue el primero en obligar (2017) y hoy
es el primero en **transitar de open banking a open finance con hoja de ruta formal**.

---

## 1. Línea de tiempo

| Año | Hito |
|---|---|
| 2016 | La CMA concluye su investigación de banca minorista |
| **2017** | **CMA Retail Banking Market Investigation Order** — obliga a los 9 bancos más grandes (**CMA9**) a exponer APIs. Se crea **OBIE** (Open Banking Implementation Entity) |
| 2018 | Entra en vigor **PSD2** (aún miembro UE) — AIS/PIS y SCA |
| 2021 | Se completa el *CMA Order Roadmap*. OBIE → **Open Banking Limited (OBL)** |
| 2022 | Se crea el **JROC** (Joint Regulatory Oversight Committee): FCA, PSR, HM Treasury, CMA |
| 2023 | JROC publica recomendaciones: modelo comercial sostenible + *Future Entity* |
| **jun-2025** | **Data (Use and Access) Act 2025 (DUAA)** — *Royal Assent* el 19 de junio. Marco habilitante para *smart data schemes* en cualquier sector |
| sep-2025 | FCA lanza el **Smart Data Accelerator** |
| dic-2025 | Carta de la FCA a asociaciones gremiales sobre el proceso para constituir la **Future Entity** |
| **Q1-2026** | Primeras transacciones **cVRP** en vivo |
| **jun-2026** | Lanzamiento del esquema **UK Payments Initiative (UKPI)** — 31 firmas |
| **abr-2026** | FCA publica *"Open finance: our vision for a smart data future"* — **roadmap a 2030** |

## 2. Cifras del ecosistema (junio 2026)

- **2,81 mil millones** de llamadas API en el mes (récord histórico, +4,4% MoM).
- Superados **100 mil millones** de llamadas API acumuladas y **1.000 millones** de pagos.
- **18,81 millones** de conexiones de usuario.
- **40,16 millones** de pagos en el mes; VRP creciendo **+6,7% MoM**.
- Tiempo de respuesta promedio **349 ms**; disponibilidad ponderada **99,80%**.
- ~17 millones de usuarios activos ≈ **1 de cada 3 adultos** del Reino Unido.

> **Lección para Colombia:** el volumen no llegó con el mandato, llegó ~5 años después,
> cuando maduraron el estándar, el directorio y los casos de uso de pago. Los SLA
> (349 ms / 99,8%) son el tipo de métrica que la SFC tendrá que definir en sus indicadores
> trimestrales del art. 2.35.8.7.2.

## 3. Estándar técnico

**Open Banking Standard (OBL)** — *Read/Write Data API Specification*, versión **4.0**:
APIs REST que permiten a los TPP leer información e iniciar pagos, cubriendo
simultáneamente las obligaciones del CMA Order y de PSD2/RTS.

Componentes:
- **Account & Transaction API** (AIS)
- **Payment Initiation API** (PIS), incluye **VRP**
- **Confirmation of Funds API** (CoF)
- **Event Notification / Webhooks**
- **Dynamic Client Registration** + **OB Directory**

Perfil de seguridad: **FAPI 1.0 Advanced** (con migración a FAPI 2.0 en discusión),
certificados **OBWAC/OBSeal**, y `tls_client_auth` / `private_key_jwt`.

## 4. Variable Recurring Payments (VRP) y el UKPI

**VRP** = mandato del cliente que autoriza pagos recurrentes de monto variable dentro de
límites definidos (importe máximo por pago, por periodo, fecha de vencimiento). Es la
alternativa británica al débito directo y a la tarjeta en archivo.

- **Sweeping VRP** (me-to-me, entre cuentas propias): obligatorio para los CMA9 desde 2022,
  sin costo.
- **Commercial VRP (cVRP)**: pagos a terceros. **No** se impuso por mandato — se resolvió
  vía esquema **voluntario liderado por la industria**.

### UK Payments Initiative (UKPI)
- Anunciado por la FCA el **2 de junio de 2026**.
- **31 firmas** (bancos + fintechs) constituyen el operador del esquema.
- Despliegue por fases durante el año: **servicios públicos, gobierno central y local,
  organizaciones benéficas y servicios financieros** (recargas de cuentas).
- La FCA apoya además la creación de un **organismo independiente de estándares** y
  consultará sobre el marco regulatorio de largo plazo antes de finales de 2026.

> **Lección para Colombia:** el Reino Unido resolvió el problema del **modelo comercial**
> —quién paga por la infraestructura de pagos— con un esquema privado con reglas
> multilaterales, en vez de un mandato gratuito. Colombia tomó el camino opuesto: datos
> gratis + recuperación de costos de infraestructura. El punto de tensión será el mismo.

## 5. Data (Use and Access) Act 2025 — *smart data schemes*

El DUAA da al Gobierno el **poder de obligar** a empresas de cualquier sector a compartir
datos del cliente, a petición de este, con terceros autorizados.

- Sectores señalados: banca y finanzas, energía, propiedad, retail, transporte, telecom,
  mercados digitales, agroalimentario.
- Cada esquema se crea por **regulación secundaria** (statutory instrument), lo que
  permite calibrar alcance, participantes, tarifas y gobernanza sector por sector.
- Se espera que HM Treasury legisle bajo el DUAA para dar a la **FCA poderes de
  *rulemaking* sobre open banking** — hoy el mandato sigue apoyado en el CMA Order de 2017.

## 6. La *Future Entity*

Cuerpo permanente que sustituirá a OBL como fijador de estándares:

- Diseño consultado en **FS25/4** ("Design of the Future Entity for UK open banking").
- Estructura esperada: **sin ánimo de lucro**, *company limited by guarantee*, con
  ingresos recaudados de forma equitativa entre usuarios y beneficiarios, y nombramientos
  del consejo hechos por un **comité independiente**.
- Dos candidatos declararon interés: **Open Banking Limited** y **Smart Data Group**.

## 7. Roadmap de open finance de la FCA (abril 2026)

Casos de uso prioritarios: **crédito a pymes** e **hipotecas**.

### 2026 — Colaborar para priorizar
| Trimestre | Actividad |
|---|---|
| Q1 | TechSprints (SME Lending y Mortgages, con datos sintéticos; corrieron nov-2025 a feb-2026) |
| Q2 | **PolicySprint** + engagement dirigido (workshops, roundtables) |
| Q3 | Informe del **taskforce PRISM** (*Prioritisation and Real-world Insights Selection Matrix*) |
| Q4 | **TechSprint** + **Discussion Paper sobre el primer esquema de open finance** |

### 2027 — Diseñar y coordinar
- Engagement para delimitar el marco del primer esquema.
- **Discussion paper** sobre el marco regulatorio de largo plazo.
- Trabajo con **HM Treasury** sobre opciones de marco regulatorio.
- Investigación y pruebas de **modelos de arquitectura e infraestructura tecnológica**.

### 2028–2030 — Escalar
Ciclo repetible por caso de uso: *identificar y acordar caso de uso → desarrollar marco
del esquema → probar marco y políticas → consultar y refinar → lanzar esquema*.

### Cifras de impacto que cita la FCA
- McKinsey: open finance podría generar **1–1,5% del PIB** británico a 2030.
- OBL + EY: impacto económico combinado de open banking + open finance de hasta
  **£7.400 millones al año** en 5 años.

## 8. Cooperación internacional

El Reino Unido participa en **Project Aperta** (BIS Innovation Hub Hong Kong), que busca
reducir fricciones en finanzas globales. Casos de uso iniciales: **apertura de cuentas
bancarias para pymes** y **financiación de comercio transfronterizo**.

---

## Qué copiar y qué no, desde Colombia

| ✅ Copiar | ⚠️ No copiar tal cual |
|---|---|
| Publicación de **métricas mensuales** de disponibilidad, latencia y volumen | El mandato solo sobre 9 bancos (Colombia ya obliga a todo el universo vigilado) |
| Separar **operador del estándar** del supervisor | Dependencia de un *CMA Order* de derecho de competencia como base legal |
| **Sandbox / TechSprints con datos sintéticos** antes de fijar el estándar | Dejar el modelo comercial de pagos sin resolver por 8 años |
| Esquema voluntario multilateral para pagos comerciales (UKPI) | |
