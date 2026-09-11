# 05 · Brasil

**Fecha de corte: 10 de septiembre de 2026**

> El ecosistema obligatorio más grande del mundo y **la referencia más útil para Colombia**:
> mismo idioma regulatorio latinoamericano, banco central como orquestador, implementación
> por fases, y arquitectura descentralizada sobre FAPI + PKI nacional.

---

## 1. Arquitectura del modelo

- Regulador: **Banco Central do Brasil (BCB)** — Resolução Conjunta nº 1/2020 y sucesivas.
- Gobernanza: **Estrutura Inicial de Governança do Open Finance Brasil**, con convenções,
  grupos técnicos y un **Diretório de Participantes** central.
- **Modelo descentralizado**: no hay un hub que enrute datos. El directorio publica quién
  es quién y las instituciones se conectan **punto a punto**.
- **PKI: ICP-Brasil** — certificados nacionales para autenticación mutua entre instituciones.
- **Perfil de seguridad: FAPI**, sobre OAuth 2.0 + mTLS. Brasil es históricamente el
  ecosistema con el programa de **certificación de conformidad FAPI más exigente** del
  mundo (pruebas obligatorias de la OpenID Foundation antes de entrar en producción).

## 2. Las cuatro fases

| Fase | Contenido |
|---|---|
| **1** | Datos abiertos: canales de atención, características de productos y servicios (sin identificar clientes) |
| **2** | Datos del cliente: registro (cadastro), cuentas, tarjetas, operaciones de crédito — previo consentimiento |
| **3** | **Iniciación de pagos** (integrada con **Pix**) y propuestas de crédito |
| **4** | **Open Finance completo**: inversiones, seguros, previsión, cambio, cuentas de pago prepagas |

## 3. Novedades 2025–2026

| Fecha | Hito |
|---|---|
| abr-2025 | El BCB anuncia prioridades regulatorias 2025–2026, con la evolución de Open Finance |
| jul-2025 | Actualizaciones normativas significativas |
| 2025 | **Resolução BCB nº 541/2025** — base de la Jornada Sem Redirecionamento |
| **feb-2026** | Lanzamiento de la **portabilidad de crédito vía Open Finance** — traslado de deuda entre instituciones totalmente digital y automatizado |
| **6-feb-2026** | **JSR** entra en pruebas de producción para instituciones detentoras de cuenta Pix |
| **22-abr-2026** | **Liberación general de la JSR** |
| **abr-2026** | Por primera vez el promedio del ecosistema alcanza la **puntuación mínima de conformidad** (Open Finance Association) |
| **2026** | **JSR y conformidad FAPI se vuelven obligatorias para iniciadores de pago** |
| **nov-2026** | Prevista la **portabilidad de crédito consignado** |

### Jornada Sem Redirecionamento (JSR)
Permite que el cliente autorice y confirme un pago **sin salir de la app del iniciador**,
eliminando el redireccionamiento a la app del banco. Reduce drásticamente el abandono en
el embudo de pago. Es el equivalente funcional a un *decoupled flow* — técnicamente
emparentado con **CIBA** (Client-Initiated Backchannel Authentication) del perfil FAPI.

### Otras prioridades declaradas para 2026
- Mejora continua del desempeño de las instituciones participantes (SLA y disponibilidad).
- **"Jornada otimizada"** de iniciación de pagos.

## 4. Escala

- **Más de 100 millones de clientes** con consentimientos activos — el mayor ecosistema
  obligatorio del planeta.
- Brasil lidera globalmente en adopción de open finance por número de vínculos.

## 5. Stack técnico (lo que Colombia puede reutilizar)

| Capa | Brasil |
|---|---|
| Especificación de API | OpenAPI, REST/JSON, versionado semántico por dominio |
| Diccionario de datos | Modelo propio alineado a estándares de mercado |
| Seguridad | **FAPI** + OAuth 2.0 + mTLS + `private_key_jwt` |
| Certificados | **ICP-Brasil** (equivalente funcional a Ley 527 de 1999 en Colombia) |
| Directorio | Diretório de Participantes con *software statements* y registro dinámico de clientes |
| Consentimiento | API de *consents* con ciclo de vida explícito (AWAITING_AUTHORISATION → AUTHORISED → REJECTED / CONSUMED / REVOKED) |
| Conformidad | **Suite de pruebas de conformidad obligatoria** de la OpenID Foundation |

---

## Por qué Brasil es el mejor benchmark para Colombia

1. **Mismo diseño institucional**: supervisor financiero como orquestador + directorio
   central + obligación por norma, no por acuerdo de mercado.
2. **PKI nacional**: Brasil usa ICP-Brasil; Colombia tiene certificados bajo Ley 527 de
   1999. El patrón de confianza es transferible casi tal cual.
3. **Fases secuenciales**: Brasil empezó por **datos abiertos** (fase 1, sin clientes),
   que es exactamente lo que el borrador colombiano prioriza como
   *"características generales de productos y servicios"*.
4. **La API de consentimiento como pieza central**: Colombia necesita una equivalente para
   soportar el modelo de doble verificación (autorización + confirmación).
5. **Certificación obligatoria**: Brasil demuestra que sin *conformance testing* forzoso el
   ecosistema no interopera. Colombia previó el **espacio de pruebas** en el art. 2.35.8.4.2
   — hay que convertirlo en requisito de inscripción en el directorio, no en algo opcional.

### La diferencia crítica
Brasil incluyó **iniciación de pagos en la fase 3** y hoy es el motor del ecosistema.
El borrador de cronograma colombiano la sitúa en el **mes 132** — es decir, último.
Ver el análisis en [`09-colombia-cronograma-y-analisis.md`](09-colombia-cronograma-y-analisis.md).
