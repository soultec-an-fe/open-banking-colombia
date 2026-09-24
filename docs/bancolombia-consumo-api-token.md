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

## 3. Comparación directa

| | §1 API Market general | §2 Sandbox Open Finance |
|---|---|---|
| Dominio | `developer-portal-public-sbx.apps.ambientesbc.com` | `*-api-open-finance-sandbox.ambientesbc.com` |
| Estándar | OAuth 2.0 genérico | FAPI 2.0 (PAR, PKCE S256, tokens mTLS-bound) |
| Métodos de auth en `/token` | Solo `client_secret` (Basic o formData) | `client_secret_basic`, `client_secret_jwt`, `tls_client_auth`, `private_key_jwt` |
| `tls_client_auth` puro | ❌ No soportado | ✅ Soportado |
| Grant típico | `client_credentials` | `authorization_code` (+ `client_credentials`, `refresh_token`, CIBA) |
| Certificación regulatoria | No aplica | Decreto 0368/2026 + CE 004/2024 |

**Conclusión para tu pregunta original:** si estás integrando contra el **API regulado
de Finanzas Abiertas** (§2), `tls_client_auth` puro (`client_id` + `grant_type`,
confiando en el certificado mTLS) **sí es una opción válida** según lo que declara el
Authorization Server. Si estás integrando contra el **API Market general** (§1, BaaS/
BNPL/pagos), no lo es — ahí el `client_secret` es obligatorio.

## 4. Fuentes

| Fuente | URL |
|---|---|
| API Market — "Autenticación - Autorización" (API Market general) | https://developer-portal-public-sbx.apps.ambientesbc.com/documentacion |
| API Market — Casos de uso → Open Finance → Open Banking Authorization | https://developer-portal-public-sbx.apps.ambientesbc.com/documentacion/Open%20Banking%20Authorization |
| Centro de Ayuda — "Paso a paso para consumir el producto de APIs Authorization" | https://soportedevs.bancolombia.com/hc/es-419/articles/21843720412180-Paso-a-paso-para-consumir-el-producto-de-APIs-Authorization |
| Centro de Ayuda — "¿Qué es OAuth?" | https://soportedevs.bancolombia.com/hc/es-419/articles/5520302584340--Qu%C3%A9-es-OAuth |
| Centro de Ayuda — "¿Qué es Json Web Token y cómo funciona?" | https://soportedevs.bancolombia.com/hc/es-419/articles/11542467193492-JWT |
| Centro de Ayuda — "¿Qué son los certificados digitales y cómo funcionan?" | https://soportedevs.bancolombia.com/hc/es-419/articles/30664196184980--Qu%C3%A9-son-los-certificados-digitales-y-c%C3%B3mo-funcionan |
| Centro de Ayuda — categoría "Casos de uso de Open Banking" (¿Qué es PKCE?, Prepare private key JWT, GET par-auth-code-url, Glosario Open Banking) | https://soportedevs.bancolombia.com/hc/es-419/categories/28750379947156 |
| Documento de descubrimiento OpenID (sandbox, en vivo) | https://auth1-api-open-finance-sandbox.ambientesbc.com/.well-known/openid-configuration |
| JWKS del Authorization Server (sandbox, en vivo) | https://s3.us-east-1.amazonaws.com/keystore.sandbox.bancol.col-hub-prod.ozoneapi.co.uk/server |
