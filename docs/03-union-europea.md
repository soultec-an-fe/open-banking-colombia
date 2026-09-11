# 03 · Unión Europea

**Fecha de corte: 10 de septiembre de 2026**

---

## 1. PSD2 — el régimen vigente

**Directiva (UE) 2015/2366**, aplicable desde enero de 2018.

- Crea dos figuras de tercero (**TPP**):
  - **AISP** — *Account Information Service Provider* (lectura de datos de cuenta).
  - **PISP** — *Payment Initiation Service Provider* (iniciación de pagos).
- Los **ASPSP** (bancos con cuentas de pago) deben dar acceso **gratuito** a los TPP
  autorizados, con consentimiento del cliente.
- **RTS on SCA & CSC** (Reglamento Delegado (UE) 2018/389):
  - **SCA** (autenticación reforzada): dos de tres factores — conocimiento, posesión,
    inherencia — con **dynamic linking** para pagos.
  - Exenciones: bajo valor, TRA (análisis de riesgo transaccional), beneficiarios de
    confianza, pagos recurrentes, acceso AIS cada 180 días.
  - Obligación de **interfaz dedicada** (API) + **mecanismo de contingencia**, salvo
    exención.

### Las tres críticas estructurales a PSD2
1. **Calidad e inconsistencia de las APIs** — sin estándar único obligatorio; convivieron
   Berlin Group NextGenPSD2, STET, Polish API, OBL...
2. **Sin modelo comercial** — acceso gratuito, sin incentivo del banco a la calidad.
3. **Alcance limitado a cuentas de pago** — no cubre crédito, inversión, seguros ni pensiones.

## 2. PSD3 + PSR — el paquete de pagos

Propuesto el **28 de junio de 2023**. Estado a septiembre de 2026:

| Fecha | Hito |
|---|---|
| 27-nov-2025 | Acuerdo político provisional Parlamento–Consejo |
| 23-abr-2026 | Textos finales acordados en trílogo (Parlamento, Consejo, Comisión) |
| mediados de 2026 | Revisión jurídico-lingüística y publicación esperada en el DOUE |

**Estructura de dos instrumentos:**
- **PSD3 (Directiva)** — licenciamiento, autorización, supervisión, acceso a sistemas de pago.
  **Fusiona las licencias de entidad de pago (PI) y de dinero electrónico (EMI)**.
- **PSR (Reglamento)** — reglas de conducta directamente aplicables, sin transposición:
  SCA, fraude, transparencia, acceso a datos, responsabilidad.

**Novedades más relevantes:**
- **Fraude APP** (*authorised push payment*): régimen de responsabilidad e IBAN/name check
  obligatorio para transferencias.
- **SCA** modernizada: accesibilidad, delegación de autenticación, menos fricción.
- **Mejora del acceso de TPP**: obligaciones de rendimiento de las interfaces dedicadas y
  eliminación del *fallback* obligatorio a cambio de estándares de servicio más duros.
- Reglas de **permission dashboard**: el usuario debe poder ver y revocar accesos de datos
  desde su banca en línea.

## 3. FIDA — el open finance europeo (⚠️ estancado)

**Framework for Financial Data Access** — propuesta de Reglamento del 28 de junio de 2023.

**Qué haría:** extender el acceso a datos más allá de cuentas de pago hacia
**inversiones, pensiones, seguros, hipotecas y préstamos**, mediante:
- **FISP** (*Financial Information Service Provider*) — nueva figura de tercero regulado.
- **Financial Data Sharing Schemes (FDSS)** — esquemas de gobernanza obligatorios donde
  la industria acuerda estándares, SLA y — clave — **compensación económica** al titular
  de los datos. Es el reconocimiento explícito del modelo comercial que PSD2 no tuvo.
- **Permission dashboards** obligatorios.

### Estado real a septiembre de 2026
- Sigue **en trílogo**. Las negociaciones se **detuvieron a inicios de 2026**.
- El bloqueo político central: si las **grandes plataformas estadounidenses**
  (Amazon, Apple, Google, Meta) deben quedar **excluidas** del ecosistema FIDA.
- Se esperaba movimiento a partir del verano de 2026.
- Proyecciones de la industria: **mejor caso** acuerdo político en 2026 con aplicación
  operativa hacia **2029**; **caso base**, acuerdo en 2027 y aplicabilidad
  **finales de 2029 – 2030**.

> **Lectura para Colombia:** la UE lleva 3 años sin cerrar su open finance, atrapada
> precisamente en las dos preguntas que Colombia resolvió por decreto en un año:
> *(a)* quién puede ser tercero receptor y *(b)* quién paga. Colombia respondió
> "solo vigilados obligatoriamente" y "los datos son gratis, la infraestructura se
> recupera". Es una decisión defendible, pero es **la misma tensión**, no su ausencia.

## 4. eIDAS 2 y la EUDI Wallet

El **Reglamento (UE) 2024/1183** crea la **European Digital Identity Wallet**, exigible a
los Estados miembros. Los bancos deberán aceptarla como medio de identificación. Impacta
open finance en tres puntos: onboarding, SCA y firma de consentimientos.

## 5. Estándares técnicos de facto

| Estándar | Uso |
|---|---|
| **Berlin Group NextGenPSD2 / openFinance API Framework** | El más extendido en Europa continental |
| **STET** | Francia |
| **OBL Standard** | Reino Unido (post-Brexit, pero interoperable) |
| **FAPI 1.0 Advanced** | Perfil de seguridad recomendado por Berlin Group |

Berlin Group evolucionó a **openFinance API Framework**, que ya anticipa FIDA con dominios
más allá de pagos (crédito, seguros, inversión).

---

## Comparación rápida PSD2 vs. Colombia D-0368/2026

| Dimensión | PSD2 (UE) | Colombia |
|---|---|---|
| Alcance de datos | Cuentas de pago | Depósito, crédito, inversión, seguros, KYC, oferta comercial |
| Iniciación de pagos | Sí, desde el día uno | Fuera del Título 8; estándares diferidos (mes 132 en el borrador) |
| Quién puede ser tercero | Cualquiera con licencia AISP/PISP (incluidas fintechs) | Solo **vigilados** de forma obligatoria; no vigilados vía acuerdo bilateral |
| Costo del acceso | Gratuito | Datos gratuitos; infraestructura con recuperación de costos |
| Autenticación | SCA (RTS) | "Mecanismos fuertes de autenticación" según instrucciones SFC |
| Consentimiento | Un solo consentimiento ante el TPP | **Doble**: autorización al tercero + confirmación ante el proveedor |
| Perfil de seguridad | No prescrito por norma (FAPI de facto) | **FAPI 2.0 prescrito** en el proyecto de Capítulo IX |
