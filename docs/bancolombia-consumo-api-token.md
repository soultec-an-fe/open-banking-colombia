# Bancolombia — Consumo del API de token

> **Fecha de corte / verificación: 24 de septiembre de 2026.**
> Fuentes: portal público `developer-portal-public-sbx.apps.ambientesbc.com/documentacion`,
> Centro de Ayuda para desarrolladores (`soportedevs.bancolombia.com`), y el documento
> de descubrimiento OpenID en vivo del sandbox de Open Finance. URLs completas en la
> sección "Fuentes" al final.

> 🔴 **Corrección importante sobre una verificación previa:** una primera revisión de
> este mismo día concluyó que `tls_client_auth` **no** estaba soportado. Esa conclusión
> era correcta **solo para el API Market general** (§1) — la investigación no había
> encontrado todavía el sandbox dedicado de Open Finance (§2), que si lo soporta. Ver
> §3 para la comparación directa.

Bancolombia expone **dos sistemas distintos** con requisitos de autenticación
**diferentes**. No son intercambiables — hay que identificar cuál aplica antes de
implementar.

## 1. API Market general (BaaS, BNPL, pagos, QR, créditos, etc.)

Portal: `developer-portal-public-sbx.apps.ambientesbc.com`. Es el marketplace genérico
de APIs de Bancolombia, no específico de Finanzas Abiertas.

**Qué se requiere:**
1. Una aplicación creada en el portal, con `client_id` y `client_secret`, y el
   producto/API suscrito.
2. Conocer el `scope` exacto de la operación (se consulta en "Seguridad" de cada API).

**Request al endpoint de token:**

Body (`application/x-www-form-urlencoded`): `grant_type=client_credentials&scope=<scope>`

Headers:
```
Content-Type: application/x-www-form-urlencoded
Accept: application/vnd.bancolombia.v4+json
Authorization: Basic <base64(client_id:client_secret)>
```

Respuesta (200 OK): `token_type`, `access_token`, `scope`, `expires_in` (1200 s por
defecto), `consented_on`. Uso: header `Authorization: Bearer <access_token>`.

**`tls_client_auth` puro: NO soportado aquí.** El `client_secret` es obligatorio en
todos los ejemplos oficiales (Basic Auth o formData); omitirlo produce el error "falta
el identificador del cliente". Confirmado en tres fuentes: la página "Autenticación -
Autorización" del portal, el artículo "Paso a paso para consumir el producto de APIs
Authorization", y "¿Qué es OAuth?" (ver §5, fuentes 1-3).

**Capa adicional por producto:** algunas operaciones (ej. `POST /purchase-intention` de
BNPL) exigen además un JWT firmado RS256 con un certificado X.509 propio de la app —
una capa adicional a nivel de producto, no un `private_key_jwt` que sustituya al
`client_secret` en `/token`.

## 2. Sandbox de Open Finance / Open Banking (el API regulado)

Este es el sistema que implementa el régimen del Decreto 0368/2026 y la Circular
Externa 004/2024. Está documentado en el portal bajo **Documentación → Casos de uso →
Open Finance → Open Banking Authorization / Account Information / Domestic Payment
Initiation / Variable Recurring Payment**, y por separado en el Centro de Ayuda bajo
la categoría **"Casos de uso de Open Banking"**. Corre sobre infraestructura de
**Ozone API** (proveedor de plataforma Open Banking usada también en UK y otros
mercados) — el `jwks_uri` resuelve a un bucket bajo `ozoneapi.co.uk`.

El propio portal lo describe así: *"Los Third Party Providers (TPPs) [...] pueden
integrarse [...] gracias a la autenticación y autorización que te brindan las
capacidades de Open Banking Authorization basadas en las directrices de **FAPI 2.0**"*.
Requisitos generales que declara Bancolombia para este sistema:
**mTLS obligatorio en toda comunicación TPP↔Banco**, certificados (mTLS y
`private_key_jwt`) emitidos por CA reconocida, un endpoint de autorización propio por
producto financiero, y vinculación/habilitación previa del TPP.

### 2.1 Documento de descubrimiento OpenID (en vivo, sandbox)

`GET https://auth1-api-open-finance-sandbox.ambientesbc.com/.well-known/openid-configuration`

Campos relevantes (respuesta real, consultada el 24-sep-2026):

```json
{
  "issuer": "https://auth1-api-open-finance-sandbox.ambientesbc.com",
  "authorization_endpoint": "https://auth1-api-open-finance-sandbox.ambientesbc.com/auth",
  "token_endpoint": "https://as1-api-open-finance-sandbox.ambientesbc.com/token",
  "registration_endpoint": "https://rs1-api-open-finance-sandbox.ambientesbc.com/dynamic-client-registration/v3.2/register",
  "jwks_uri": "https://s3.us-east-1.amazonaws.com/keystore.sandbox.bancol.col-hub-prod.ozoneapi.co.uk/server",
  "userinfo_endpoint": "https://as1-api-open-finance-sandbox.ambientesbc.com/userinfo",
  "introspection_endpoint": "https://as1-api-open-finance-sandbox.ambientesbc.com/introspection",
  "revocation_endpoint": "https://as1-api-open-finance-sandbox.ambientesbc.com/token/revoke",
  "pushed_authorization_request_endpoint": "https://as1-api-open-finance-sandbox.ambientesbc.com/par",
  "backchannel_authentication_endpoint": "https://as1-api-open-finance-sandbox.ambientesbc.com/bc-authorize",
  "grant_types_supported": ["authorization_code", "client_credentials", "refresh_token", "urn:ietf:params:oauth:grant-type:jwt-bearer", "urn:openid:params:grant-type:ciba"],
  "response_types_supported": ["code", "code id_token"],
  "scopes_supported": ["openid", "accounts", "payments", "fundsconfirmations", "consents", "resources", "credit-cards-accounts", "customers", "loans", "financings", "invoice-financings", "unarranged-accounts-overdraft", "consumption", "lg", "recurring-payments", "bank-fixed-incomes", "recurring-consent"],
  "token_endpoint_auth_methods_supported": ["client_secret_basic", "client_secret_jwt", "tls_client_auth", "private_key_jwt"],
  "tls_client_certificate_bound_access_tokens": true,
  "code_challenge_methods_supported": ["S256"],
  "request_uri_parameter_supported": true,
  "require_request_uri_registration": true,
  "mtls_endpoint_aliases": {
    "token_endpoint": "https://as1-api-open-finance-sandbox.ambientesbc.com/token",
    "revocation_endpoint": "https://as1-api-open-finance-sandbox.ambientesbc.com/token/revoke",
    "introspection_endpoint": "https://as1-api-open-finance-sandbox.ambientesbc.com/introspection",
    "pushed_authorization_request_endpoint": "https://as1-api-open-finance-sandbox.ambientesbc.com/par",
    "userinfo_endpoint": "https://as1-api-open-finance-sandbox.ambientesbc.com/userinfo"
  }
}
```

### 2.2 JWKS del servidor de autorización (sandbox)

`GET https://s3.us-east-1.amazonaws.com/keystore.sandbox.bancol.col-hub-prod.ozoneapi.co.uk/server`

```json
{
  "keys": [
    {
      "kty": "RSA",
      "kid": "gG1Sj_csLXsUQKQAaJOnGRcFmIDelrSnfriU1XC98xM",
      "use": "sig",
      "e": "AQAB",
      "n": "7-iW13Z9KMZQ0eDqSjlm96mtFePZ2zRu8PiYM9pqI5K1SvTW1cHDgZvDPqsWtoKLyeRM_ufvUG3gY_X7gWsc7eUbEOr6_p3nX3Qyg1L7cXjGRvwUSS77Va8dnRH4eqrGbg4763vMNYm8c5_U1Y8Zp8PHsjr9fY7tDaRXZxkXtctn2fKlTuN7FJB9si4LbxLyufbBxSeEhtdIUtUT0rdtnqMB1aJjm6ai4fHQ2bPuteHTCnGkQ_ciKIEH4i6nTzYiQrghIIthX1_1uxVnHf1usFcLLj0T20PCnGrLH3NG_Po_zt6kH8ovtizPm2BxEGx3LyR3VoI42MciDK2nfqyu9w"
    }
  ]
}
```

Es la(s) llave(s) pública(s) del **Authorization Server de Bancolombia** — sirven para
verificar los JWT/`id_token` que **Bancolombia firma y emite** (ej. validar un
`id_token` o un JARM), no para que el TPP firme sus propias solicitudes.

### 2.3 `tls_client_auth` puro — SÍ está soportado aquí

`token_endpoint_auth_methods_supported` incluye explícitamente **`tls_client_auth`**,
junto a `private_key_jwt`, `client_secret_basic` y `client_secret_jwt`. Combinado con
`"tls_client_certificate_bound_access_tokens": true`, esto es exactamente RFC 8705: el
cliente se autentica en `/token` presentando el certificado ya negociado en el canal
mTLS, sin enviar `client_secret` — solo `client_id` + `grant_type` (+ lo que exija el
grant, ej. `code` y `code_verifier` en Authorization Code).

**No se probó el request real contra `/token`** (requiere un cliente registrado con
certificado vinculado); esto se verificó contra el documento de descubrimiento
público, que es la fuente estándar y autoritativa para qué métodos acepta un
Authorization Server OIDC/FAPI.

> ⚠️ **Matiz tras revisar el flujo documentado paso a paso** (portal → Documentación →
> Casos de uso → Open Finance → Open Banking Authorization → "Documentación", distinto
> de "Introducción" y "OpenID Discovery"): en **cada uno** de los 6 pasos del flujo que
> Bancolombia describe (autorización inicial del TPP, creación de la intención de
> autorización, PAR, redirección/consentimiento del cliente, intercambio de código por
> token, consumo de las APIs de producto) el texto repite explícitamente:
> *"Para la autenticación del consumidor en el servicio se requiere **MTLS y
> private_key_jwt**"*. El discovery document declara `tls_client_auth` como capacidad
> soportada, pero **el flujo real que Bancolombia documenta y espera que implementes usa
> `private_key_jwt`** de forma consistente en todos los pasos — mTLS ahí funciona como
> capa de transporte y vinculación del token (sender-constraining), no como el método de
> autenticación del cliente en `/token`. No hay ningún paso del flujo documentado donde
> describan `tls_client_auth` puro como el mecanismo a usar. **Recomendación:**
> implementar `private_key_jwt` (lo documentado paso a paso) en vez de apoyarse solo en
> que el discovery doc lista `tls_client_auth` como soportado.

### 2.4 Cómo generar el certificado para firmar el JWT (`private_key_jwt`)

Bancolombia no publica una guía de generación de certificados específica para Open
Finance — remite a la misma guía general del Centro de Ayuda
("¿Qué son los certificados digitales y cómo funcionan?"), que aplica igual aquí:

```bash
openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem
```

Datos de ejemplo pedidos por el prompt de `openssl`:
```
Country Name: CO
State or Province: ANTIOQUIA
Locality Name: MEDELLIN
Organization Name: Mi Organization S.A
Organizational Unit Name: BANCOLOMBIA
Common Name: nombreapp.apps.ambientesbc.com
```

**Características exigidas:** tamaño de clave **2048** (RSA), algoritmo de firma
**SHA256 con RSA**, formato **X.509 codificado en base64**.

**Sandbox vs. producción:**
- Sandbox: certificado **autofirmado** permitido.
- Producción: **no se permite autofirmado** — debe emitirlo una Autoridad Certificadora
  (CA) reconocida, conforme a la **Ley 527 de 1999** (CE 004/2024 exige lo mismo tanto
  para mTLS como para `private_key_jwt`, ver `docs/07-fapi-y-seguridad.md` §4.1.b y c
  de este repositorio).

**Pasos:**
1. Generar el par de llaves (`key.pem` privada, `certificate.pem`/clave pública) con el
   comando anterior.
2. Adjuntar el **certificado público** a la app desde "Editar aplicación" en el portal
   — esto es lo que permite a Bancolombia registrar tu `kid` contra tu llave pública
   (el registro formal de cliente OIDC para Open Finance usa además el
   `registration_endpoint` del discovery doc:
   `https://rs1-api-open-finance-sandbox.ambientesbc.com/dynamic-client-registration/v3.2/register`,
   no confirmado en detalle — requiere sesión autenticada en el portal).
3. Con la **llave privada** (`key.pem`), firmar el JWT en RS256 siguiendo la estructura
   de "Utilidad: Prepare private key JWT" (§2 de este documento no cubierto aquí, ver
   fuente en la tabla de fuentes): `header.alg=RS256`, `header.kid=<el kid registrado>`,
   `body.iss=body.sub=<client_id>`, `body.aud=<issuer del discovery doc>`, `body.exp`,
   `body.iat`, `body.jti=<GUID único>`.
4. Enviar ese JWT firmado como `client_assertion` (con
   `client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer`) en la
   solicitud a `/token`, además de establecer la conexión sobre **mTLS** con el mismo
   certificado.

## 3. Comparación directa

| | §1 API Market general | §2 Sandbox Open Finance |
|---|---|---|
| Dominio | `developer-portal-public-sbx.apps.ambientesbc.com` | `*-api-open-finance-sandbox.ambientesbc.com` |
| Estándar | OAuth 2.0 genérico | FAPI 2.0 (PAR, PKCE S256, tokens mTLS-bound) |
| Métodos de auth en `/token` | Solo `client_secret` (Basic o formData) | `client_secret_basic`, `client_secret_jwt`, `tls_client_auth`, `private_key_jwt` |
| `tls_client_auth` puro | ❌ No soportado | ⚠️ Declarado soportado por el discovery doc, pero **no** es el método que Bancolombia documenta en su flujo paso a paso |
| Método documentado en el flujo paso a paso | `client_secret` (Basic o formData) | `private_key_jwt` + mTLS (repetido en los 6 pasos del flujo) |
| Grant típico | `client_credentials` | `authorization_code` (+ `client_credentials`, `refresh_token`, CIBA) |
| Certificación regulatoria | No aplica | Decreto 0368/2026 + CE 004/2024 |

**Conclusión para tu pregunta original:** si estás integrando contra el **API regulado
de Finanzas Abiertas** (§2), el Authorization Server **declara** soportar `tls_client_auth`
puro (`client_id` + `grant_type`, confiando en el certificado mTLS) en su discovery
document — técnicamente es una opción. Pero el **flujo que Bancolombia documenta y
espera que implementes usa `private_key_jwt`** en cada paso, no `tls_client_auth` puro
(ver §2.3 y §2.4 para cómo generar el certificado para ese flujo). Si vas a producción,
la recomendación es implementar lo documentado (`private_key_jwt`) y no depender de una
capacidad que el AS declara pero que Bancolombia no usa como ejemplo en ningún paso —
confírmalo con ellos antes de decidirte por `tls_client_auth` puro. Si estás integrando
contra el **API Market general** (§1, BaaS/BNPL/pagos), ninguna de las dos aplica — ahí
el `client_secret` es obligatorio.

## 4. Fuentes

| Fuente | URL |
|---|---|
| API Market — "Autenticación - Autorización" (API Market general) | https://developer-portal-public-sbx.apps.ambientesbc.com/documentacion |
| API Market — Casos de uso → Open Finance → Open Banking Authorization | https://developer-portal-public-sbx.apps.ambientesbc.com/documentacion/Open%20Banking%20Authorization |
| Centro de Ayuda — "Paso a paso para consumir el producto de APIs Authorization" | https://soportedevs.bancolombia.com/hc/es-419/articles/21843720412180-Paso-a-paso-para-consumir-el-producto-de-APIs-Authorization |
| Centro de Ayuda — "¿Qué es OAuth?" | https://soportedevs.bancolombia.com/hc/es-419/articles/5520302584340--Qu%C3%A9-es-OAuth |
| Centro de Ayuda — "¿Qué es Json Web Token y cómo funciona?" | https://soportedevs.bancolombia.com/hc/es-419/articles/11542467193492-JWT |
| Centro de Ayuda — "¿Qué son los certificados digitales y cómo funcionan?" | https://soportedevs.bancolombia.com/hc/es-419/articles/30664196184980--Qu%C3%A9-son-los-certificados-digitales-y-c%C3%B3mo-funcionan |
| Centro de Ayuda — categoría "Casos de uso de Open Banking" (¿Qué es PKCE?, Prepare private key JWT, GET par-auth-code-url, Glosario Open Banking) | https://soportedevs.bancolombia.com/hc/es-419/categories/28750341626900-Casos-de-uso-de-Open-Banking |
| Documento de descubrimiento OpenID (sandbox, en vivo) | https://auth1-api-open-finance-sandbox.ambientesbc.com/.well-known/openid-configuration |
| JWKS del Authorization Server (sandbox, en vivo) | https://s3.us-east-1.amazonaws.com/keystore.sandbox.bancol.col-hub-prod.ozoneapi.co.uk/server |
| API Market — Open Banking Authorization → "Documentación" (flujo paso a paso, 6 pasos, cada uno exige MTLS + private_key_jwt) | https://developer-portal-public-sbx.apps.ambientesbc.com/documentacion/Open%20Banking%20Authorization |
