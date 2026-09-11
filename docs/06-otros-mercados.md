# 06 · Otros mercados relevantes

**Fecha de corte: 10 de septiembre de 2026**

---

## 1. Chile — Ley Fintec y el Sistema de Finanzas Abiertas

El comparable más directo de Colombia en la región: mismo lenguaje ("finanzas abiertas"),
mismo diseño de supervisor único, y **mismo problema de plazos**.

| Hito | Detalle |
|---|---|
| **Ley 21.521 (Ley Fintec)** | Promulgada a inicios de **2023**. Objetivo: competencia, innovación e inclusión. Crea el **Sistema de Finanzas Abiertas (SFA)** en su Título III |
| **NCG N.º 514 de la CMF** | Norma de Carácter General que regula el SFA. Publicada en **julio de 2024** |
| Cronograma original | La implementación comenzaba **24 meses** después de la norma → **4 de julio de 2026** |
| **nov-2025** | La CMF pone en consulta pública una prórroga de **12 meses adicionales** |
| **1-jun-2026** | La CMF **modifica la NCG 514**, incorpora **anexo técnico** y **pospone la entrada en vigor a julio de 2027** |
| +18 meses | Se suma la obligación para **cooperativas de ahorro y crédito** fiscalizadas por la CMF, **compañías de seguros**, **administradoras de fondos** y **cajas de compensación** |

**Roles del SFA chileno:**
- **IPDI** — Instituciones Proveedoras de Información
- **IPCD** — Instituciones Proveedoras de Cuentas de Depósito
- **PSI** — Proveedores de Servicios basados en Información
- **PSIP** — Proveedores de Servicios de Iniciación de Pagos

> 🔑 **Lección directa para Colombia:** Chile fijó plazos y tuvo que prorrogarlos **dos
> veces**, de julio 2026 a julio 2027. El regulador chileno reconoció que la industria no
> llegaba. Colombia está tomando el camino opuesto — plazos muy largos desde el inicio —
> lo que evita prórrogas pero difiere el beneficio. Ambos extremos tienen costo.

**Diferencia estructural con Colombia:** Chile creó las figuras de tercero **por ley**
(PSI y PSIP se registran ante la CMF, aunque no sean bancos). Colombia dejó a los no
vigilados fuera del régimen obligatorio.

## 2. México — Ley Fintech

| Hito | Detalle |
|---|---|
| **Ley para Regular las Instituciones de Tecnología Financiera** (mar-2018) | **Artículo 76**: obliga a compartir vía APIs estandarizadas |
| Tres tipos de datos | **Datos abiertos** (productos, sucursales, cajeros), **datos agregados** (estadísticos, anonimizados), **datos transaccionales** (del cliente, con consentimiento) |
| **CNBV, Disposiciones de carácter general (mar-2020)** | Solo reglamentaron **datos abiertos** de conglomerados financieros |
| Estado | Los **datos transaccionales** — la parte que realmente habilita open banking — **siguen sin regulación secundaria** |

> ⚠️ **La advertencia mexicana:** un mandato legal ambicioso sin reglamentación técnica se
> queda en papel durante años. México lleva **8 años** con el art. 76 vigente y sin APIs
> de datos transaccionales. Es el riesgo que corre Colombia si el cronograma se estira.

## 3. Australia — Consumer Data Right (CDR)

- **Competition and Consumer Act 2010, Part IVD** + **CDR Rules** (ACCC) + estándares
  técnicos del **Data Standards Body (DSB)**.
- **Multisectorial desde el diseño**: banca (2020), energía (2022), telecomunicaciones
  (en curso). Es el modelo *open data* más maduro del mundo.
- **Accreditation tiers**: *unrestricted*, *sponsored*, *affiliate*, **CDR representative**,
  *trusted adviser* — un espectro de niveles de acreditación que reduce la barrera de
  entrada para actores pequeños.
- Perfil de seguridad: **FAPI 1.0 Advanced**, con extensiones propias (*CDR Arrangement ID*,
  sharing duration).
- **Action initiation**: reforma legislativa que extiende el CDR de leer datos a
  **ejecutar acciones** (pagos, cambio de proveedor, apertura de cuentas).

> 🔑 **Lo más aprovechable para Colombia:** los **niveles de acreditación**. Australia
> resolvió el problema que Colombia dejó abierto — cómo dejar entrar a un actor pequeño no
> supervisado sin bajar la barra de seguridad — creando categorías intermedias
> (*CDR representative*, patrocinado por un acreditado). Es una alternativa concreta al
> esquema de acuerdos bilaterales del Capítulo 6.

## 4. India — Account Aggregator (AA)

- Regulado por el **RBI** como categoría de NBFC (*NBFC-AA*), operativo desde 2021.
- **Modelo de intermediario neutral**: el AA es un "conducto ciego" — mueve datos cifrados
  entre **FIP** (*Financial Information Provider*) y **FIU** (*Financial Information User*)
  **sin poder leerlos**.
- **DEPA** (*Data Empowerment and Protection Architecture*) y el **artefacto de
  consentimiento** legible por máquina: estructura estandarizada que define finalidad,
  tipo de dato, frecuencia y vigencia.
- **Sahamati** es la organización sectorial que opera la gobernanza.
- Escala: cientos de millones de cuentas conectadas; el caso de uso dominante es
  **originación de crédito a microempresas**.

> 🔑 **Lo aprovechable:** el **artefacto de consentimiento estandarizado y legible por
> máquina**. Colombia exige un contenido mínimo de autorización (art. 2.35.8.3.2:
> identificación del tercero, datos, tratamiento, finalidad y tiempo) — que es
> precisamente un artefacto de consentimiento. Estandarizarlo como esquema JSON
> firmado resolvería de un golpe la interoperabilidad del **doble consentimiento**.

## 5. BIS — Project Aperta

Proyecto del **BIS Innovation Hub (Hong Kong)** para reducir fricciones y costos en
finanzas globales mediante **portabilidad de datos transfronteriza**.

- Casos de uso iniciales: **apertura de cuenta bancaria para pymes** y
  **financiación de comercio transfronterizo**.
- La **FCA británica participa** y lo señala explícitamente en su roadmap de open finance.

> Relevante para Colombia como vector de exportación: una pyme colombiana que pueda
> presentar su historial financiero verificado en el exterior reduce fricción de comercio.
> Vale la pena seguirlo antes de que el estándar quede fijado sin la región.

---

## Tabla rápida de madurez

| País | Obligatorio | Alcance | Iniciación de pagos | Estado real |
|---|---|---|---|---|
| Reino Unido | Sí (CMA9) | Cuentas de pago → open finance | Sí, con **VRP/cVRP** en despliegue | 🟢 Maduro, escalando |
| Brasil | Sí (universal) | Open finance completo | Sí, integrado con Pix + JSR | 🟢 Maduro, mayor escala |
| Australia | Sí | Multisectorial | *Action initiation* en reforma | 🟢 Maduro |
| UE | Sí (PSD2) | Cuentas de pago | Sí | 🟡 Maduro pero fragmentado; FIDA estancado |
| India | Sí (RBI) | Financiero amplio | No (solo datos) | 🟢 Alta escala en crédito |
| Chile | Sí (Ley 21.521) | Open finance | Sí (PSIP) | 🟡 Prorrogado a **jul-2027** |
| **Colombia** | **Sí (D-0368/2026)** | Open finance + portabilidad | Diferido (estándares pendientes) | 🟠 **Arrancando** |
| EE.UU. | Formalmente sí, **suspendido** | Tarjetas + cuentas Reg E | Indirecta | 🔴 Limbo judicial |
| México | Sí (art. 76 Ley Fintech) | Solo datos abiertos reglamentados | No | 🔴 Estancado desde 2020 |
