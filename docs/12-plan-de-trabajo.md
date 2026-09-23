# 12 · Plan de trabajo

**Fecha de corte: 23 de septiembre de 2026**

---

## 1. Lo urgente: 4 días

| # | Acción | Vence | Responsable |
|---|---|---|---|
| 1 | Decidir si se radican comentarios al **proyecto de decreto de ampliación de plazos** (art. 4 D-0368 / art. 6 D-0977 — 6 meses más para cronograma, Directorio, indicadores y portabilidad) | **27-sep-2026, 11:59 p.m.** | — |
| 2 | Comentarios al **proyecto de cronograma** de la SFC — ventana **cerrada** (15-sep-2026). Pendiente confirmar si se radicó la matriz [`entregables/matriz-comentarios-cronograma-finanzas-abiertas.docx`](../entregables/matriz-comentarios-cronograma-finanzas-abiertas.docx) | 15-sep-2026 (cerrado) | — |

Los seis comentarios técnicamente fundamentados ya están redactados en
[`09-colombia-cronograma-y-analisis.md`](09-colombia-cronograma-y-analisis.md), sección 5.
Los tres de mayor impacto:

- **Iniciación de pagos a 132 meses** — pedir adelanto o aclaración de la ruta paralela por
  el Banco de la República (art. 104 Ley 2294 de 2023).
- **Portabilidad comercial a 24 meses** — posible desalineación con el tope del art. 6 del
  Decreto 0977 de 2026.
- **Características generales de productos a 36–72 meses** — adelantarlas como *quick win*
  de bajo riesgo, por no requerir autorización del titular.

## 2. Decisiones estratégicas abiertas

### Decisión A — ¿Qué rol jugamos?

| Rol | Requisitos | Ventaja | Riesgo |
|---|---|---|---|
| **Proveedor de Datos** | Ser entidad vigilada del art. 2.35.8.2.3 | Ya obligado: hay que cumplir sí o sí | Costo de infraestructura; solo recuperable parcialmente |
| **Tercero Receptor Vigilado** | Constitución y funcionamiento autorizados por la SFC (art. 53 EOSF) + inscripción en directorio | **Derecho de acceso** (art. 2.35.8.3.10): el Proveedor no puede negarse | Requiere licencia SFC |
| **Tercero Receptor No Vigilado** | Acuerdo bilateral con cada Proveedor + los ~10 requisitos del numeral 6.2 | Sin licencia SFC | ❌ **Sin derecho de acceso**; negociación uno a uno |
| **Iniciador de pagos (no vigilado)** | Cumplir el reglamento de la EASPBV (art. 2.17.4.1.2 num. 4) | ✅ **Derecho de acceso** y habilitación directa **sin licencia SFC** | Sin estándar común de la SFC → integrar con cada EASPBV por separado |
| **Proveedor de infraestructura** | Ninguno regulatorio directo | Vende a los tres anteriores; mercado cautivo por el mandato | Depende del ritmo del cronograma |

> **Observación:** dado el cronograma largo, el rol de **proveedor de infraestructura y
> cumplimiento** (AS FAPI 2.0, consent management, gateway, logging, certificación) tiene
> demanda inmediata y **no depende** de que salgan los estándares funcionales.

### Decisión B — ¿Base del estándar funcional?
Recomendación: **adaptar Open Finance Brasil**. Mismo perfil FAPI, mismo modelo de
directorio, mismo contexto regulatorio latinoamericano, ciclo de vida de consentimiento ya
resuelto. Alternativa: OBL v4 (más maduro en pagos, pero muy atado a PSD2/CMA Order).

### Decisión C — ¿Certificación FAPI 2.0 ahora o después?
Recomendación: **ahora, y con carácter de cumplimiento vencido, no de anticipación.**

**FAPI 2.0 no es opcional ni futuro.** La Circular Externa 004 de 2024, numeral 3.2.3
literal a), lo exige desde febrero de 2024, y el plazo de adopción **venció el 7 de agosto
de 2026** (18 meses + 6 de la CE 009/2025 + 6 de la CE 001/2026). Cualquier entidad vigilada
que participe en finanzas abiertas y no cumpla FAPI 2.0 está **en incumplimiento hoy**.

La certificación de conformidad de la OpenID Foundation es gratuita y autoadministrada, y
además anticipa el requisito que la SFC casi con certeza convertirá en condición de
inscripción en el directorio (art. 2.35.8.5.4).

### Decisión D — ¿Y si la vía es iniciación de pagos y no finanzas abiertas?
Para un actor **no vigilado**, el Título 4 del Libro 17 ofrece algo que el Título 8 no da:
**habilitación directa de la actividad y derecho de acceso** a los sistemas de pago de bajo
valor (arts. 2.17.4.1.1 y 2.17.4.1.2), sin licencia de la SFC. Vale la pena evaluarlo como
puerta de entrada antes que la negociación bilateral de finanzas abiertas.
Ver [`14`](14-iniciacion-de-pagos-colombia.md).

## 3. Backlog priorizado

### 🔴 P0 — Sin dependencias regulatorias (arrancar ya)

| # | Entregable | Base |
|---|---|---|
| P0-1 | Authorization Server **FAPI 2.0** certificado (PAR, PKCE S256, tokens *sender-constrained*, `private_key_jwt`, `iss`, code TTL 60 s) | [`07`](07-fapi-y-seguridad.md) |
| P0-2 | **Gateway mTLS** con validación de certificados Ley 527 de 1999 y *rate limiting* por Tercero Receptor | [`11`](11-arquitectura-de-referencia.md) |
| P0-3 | **Consent Service**: artefacto JSON firmado (JWS/PS256) con los 5 campos del art. 2.35.8.3.2, ciclo de vida, revocación, dashboard | [`08`](08-colombia-marco-normativo.md) |
| P0-4 | **Pantalla de confirmación** fusionada con autenticación fuerte, sin *dark patterns* (numeral 7 del proyecto Cap. IX) | [`11`](11-arquitectura-de-referencia.md) |
| P0-5 | **Audit log de 5 años** con los 5 campos exigidos, cifrado/enmascarado | Proyecto Cap. IX 5.1 |
| P0-6 | **API de catálogo de productos** (categoría 3 — sin autorización del titular) | Art. 2.35.8.2.1 num. 3 |
| P0-7 | **Marco de pruebas de calidad DAMA** + área responsable en el SCI | Proyecto Cap. IX 5.1 |
| P0-8 | 🔴 **Verificar cumplimiento de la CE 004/2024** — el plazo venció el 7-ago-2026. FAPI 2.0, ISO 20022, logs de 5 años, red lógicamente separada, políticas de vinculación aprobadas por junta y publicadas | CE 004/2024, num. 2 a 5 |

### 🟠 P1 — Preparatorio (Q4 2026 – Q1 2027)

| # | Entregable |
|---|---|
| P1-1 | **Gap analysis** contra el nuevo Capítulo IX + plan de adecuación (exigible en 30 días desde la expedición; tope 7-abr-2027) |
| P1-2 | **Modelo de costos de infraestructura** y esquema tarifario por volumen, con factores objetivos, medibles y verificables (arts. 2.35.8.3.8 y 2.35.8.3.9) |
| P1-3 | **Preparación de inscripción en el directorio** (operativo antes del 10-abr-2027): identificación, contacto, tarifas, claves y contacto operativo designado |
| P1-4 | **Políticas de vinculación de Terceros No Vigilados**, aprobadas por junta directiva y publicadas en web (numeral 6.2 del proyecto Cap. IX) |
| P1-5 | **Mapeo ISO 20022** del catálogo de datos propios |
| P1-6 | **Gestión de vulnerabilidades API** alineada a OWASP API Top 10, NIST SP 800-204 y CSA |
| P1-7 | **Runbook de incidentes** con reporte al administrador del directorio (art. 2.35.8.5.6 num. 3) |

### 🟡 P2 — Portabilidad financiera (2027–2028)

| # | Entregable |
|---|---|
| P2-1 | Endpoint de **certificado de portabilidad** (6 campos del art. 2.35.8.8.9), con vigencias de 5 y 30 días calendario |
| P2-2 | **Motor de estudio de portabilidad** y generación de **oferta irrevocable** (art. 846 C.Co.) |
| P2-3 | Orquestación de **garantías**: cesión/subrogación, registro de garantías mobiliarias, libranzas, cambio de beneficiario de seguros individuales |
| P2-4 | **SLA de plazos**: 3 días hábiles (certificado), 5/30 días calendario (decisión), 5 días hábiles (aceptación) |
| P2-5 | Reglas de **gratuidad** y de no cobro de derechos notariales/registrales/timbre en hipotecario |

### 🟢 P3 — Datos del titular y pagos (según cronograma)

| # | Entregable |
|---|---|
| P3-1 | `/accounts`, `/balances`, `/transactions` con 12 meses de historial |
| P3-2 | `/credit-operations` |
| P3-3 | `/customer` — KYC reutilizable |
| P3-4 | `/term-deposits` |
| P3-5 | Iniciación de pagos — seguimiento de la vía Banco de la República / art. 2.17.4.1.3 |

## 4. Entregables de este repositorio

- [x] Investigación de UK, UE, EE.UU., Brasil, Chile, México, Australia, India
- [x] Estado y detalle de FAPI 1.0 / 2.0
- [x] Marco normativo colombiano completo, artículo por artículo
- [x] Cronograma de la SFC con fechas proyectadas y análisis crítico
- [x] Comparativa internacional
- [x] Arquitectura de referencia
- [x] Matriz de comentarios para la SFC — 8 observaciones, en `entregables/` *(falta identificar al remitente y enviar)*
- [ ] Especificación OpenAPI 3.1 de la API de catálogo de productos
- [ ] Esquema JSON del artefacto de consentimiento colombiano
- [ ] Gap analysis contra el proyecto de Capítulo IX

## 5. Rutina de vigilancia regulatoria

| Frecuencia | Qué revisar | Dónde |
|---|---|---|
| Semanal | Proyectos de normatividad y circulares externas | `superfinanciera.gov.co` → Normativa |
| Semanal | Decretos que modifiquen el Decreto 2555 de 2010, Libro 35, Parte 2, Título 8 | Diario Oficial / `urf.gov.co` / `dapre.presidencia.gov.co` |
| Mensual | Cambios en especificaciones FAPI | `openid.net/wg/fapi/specifications/` |
| Mensual | Indicadores trimestrales del sistema (una vez la SFC los publique) | `superfinanciera.gov.co` |
| Trimestral | Evolución de Open Finance Brasil y del roadmap de la FCA | `openfinancebrasil.org.br`, `fca.org.uk` |
| Trimestral | Estado de FIDA (UE) y del NPRM del CFPB (EE.UU.) | Comisión Europea, `consumerfinance.gov` |

## 6. Riesgos del proyecto

| Riesgo | Impacto | Mitigación |
|---|---|---|
| El cronograma final difiere del borrador | Alto | No comprometer roadmap de producto a fechas del borrador; construir P0 que es independiente |
| El Capítulo IX final cambia los estándares técnicos | Medio | Ceñirse a **FAPI 2.0 puro** — es el marco citado; los detalles reenumerados en el proyecto son los que probablemente se ajusten |
| Tarifas de infraestructura prohibitivas | Alto para Terceros Receptores | Documentar y comparar tarifas publicadas en el directorio; el principio de igualdad del art. 2.35.8.3.9 num. 3 es la base de una eventual queja |
| Sin acuerdo bilateral como no vigilado | Crítico | Evaluar el camino de licencia SFC, o alianza con un vigilado, o rol de proveedor de infraestructura |
| Suites TLS 1.2 exigidas literalmente | Bajo | Soportar 1.2 y 1.3 simultáneamente |
| Reproceso por falta de estándar funcional | Medio | Adoptar el modelo de Open Finance Brasil como base; el costo de adaptación es menor que el de diseñar desde cero |
