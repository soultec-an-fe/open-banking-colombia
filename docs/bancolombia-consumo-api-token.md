# Bancolombia — Consumo del API de token (API Market)

> **Fecha de corte / verificación: 24 de septiembre de 2026.**
> Fuente: portal público `developer-portal-public-sbx.apps.ambientesbc.com/documentacion`
> (sección "Autenticación - Autorización") y Centro de Ayuda para desarrolladores
> (`soportedevs.bancolombia.com`). URLs completas en la sección "Fuentes" al final.

> ⚠️ **Alcance:** este documento cubre el **API Market general** de Bancolombia (BaaS,
> BNPL, pagos, QR, créditos, etc.), no un producto identificado explícitamente como
> "Finanzas Abiertas" / "Open Finance" regulado. No se encontró ese producto ni en el
> catálogo público ("Públicos") ni en el de aliados/socios ("Aliados", acceso
> restringido). Si el objetivo es integrar contra el API regulado por el Decreto
> 0368/2026 y la Circular Externa 004/2024 (que exige `private_key_jwt` como método de
> autenticación FAPI 2.0, no `client_secret`), **confirmar directamente con Bancolombia**
> (mesa de ayuda: `soportedevs.bancolombia.com`) antes de asumir que este flujo aplica.

## 1. Qué se requiere

1. **Una aplicación creada en el portal**, con:
   - `client_id` y `client_secret` generados al crearla.
   - El producto/API deseado suscrito a esa app (requiere aprobación).
   - Certificado público (X.509) adjunto a la app, **solo si el producto puntual lo exige**
     (ver §4).
2. Conocer el **`scope`** exacto de la operación a consumir — se consulta en el apartado
   "Seguridad" de cada API dentro del API Market. No es genérico: varía por producto
   (ej. `Transfer-Intention:write:app` + `Transfer-Intention:read:app` para Payment Button).

## 2. Request al endpoint de token

**Body** (`application/x-www-form-urlencoded`):
```
grant_type=client_credentials
scope=<scope de la operación>
```

**Headers**:
```
Content-Type: application/x-www-form-urlencoded
Accept: application/vnd.bancolombia.v4+json
Authorization: Basic <base64(client_id:client_secret)>
```

**Respuesta** (200 OK): `token_type`, `access_token`, `scope`, `expires_in`
(1200 s / 20 min por defecto), `consented_on`.

**Uso del token**: header `Authorization: Bearer <access_token>` en las llamadas al
producto correspondiente.

## 3. `tls_client_auth` puro — NO está soportado

Se verificó explícitamente si el endpoint admite `tls_client_auth` (RFC 8705 — solo
`client_id` + `grant_type`, confiando en el certificado mTLS ya negociado, sin
`client_secret`). **No está documentado como método soportado.** Tres fuentes oficiales
consistentes entre sí lo confirman:

| Fuente | Cita |
|---|---|
| Portal — "Autenticación - Autorización" | *"Los clientes confidenciales deben autenticarse mediante la Autenticación básica HTTP, en header Authorization: Basic base64 (client_id:client_secret). Alternativamente, se puede enviar el client_id y client_secret como parámetros en formData."* |
| Centro de Ayuda — "Paso a paso para consumir el producto de APIs Authorization" | Especifica el request literal con `Authorization: Basic base64(client-id:client-secret)`, y documenta el error si se omite: *"el API nos indicará que falta el identificador del cliente"* |
| Centro de Ayuda — "¿Qué es OAuth?" | Instruye a registrar siempre **Client ID y Client Secret** |

**Conclusión:** el `client_secret` es obligatorio en todos los ejemplos oficiales del API
Market. No enviarlo (confiando solo en el certificado mTLS) rompe la petición.

## 4. Capas adicionales de seguridad por producto

No todos los productos usan el mismo mecanismo — hay que revisar el apartado
"Seguridad" de cada operación puntual antes de generar el token:

- Algunas operaciones requieren, **además** del token Bearer, un **JWT firmado (RS256)**
  con certificado X.509 propio de la app. Ejemplo citado por Bancolombia:
  `POST /purchase-intention` del producto BNPL usa **token de Authorization + JWT** al
  mismo tiempo.
- Otras usan **API Key** en lugar de, o junto con, el token.
- El certificado X.509/JWT descrito aquí **no es un `private_key_jwt`** (RFC 7523 / OIDC
  Core) que sustituya al `client_secret` en `/token` — es una capa adicional a nivel de
  producto, no del endpoint de token en sí.

**Generación del certificado** (para el mecanismo JWT, no para `/token`):
- Tamaño de clave: 2048 (RSA).
- Algoritmo de firma: SHA256 con RSA.
- Formato: X.509 codificado en base64.
- **Sandbox**: se permite certificado autofirmado (`openssl req -newkey rsa:2048 -nodes
  -keyout key.pem -x509 -days 365 -out certificate.pem`).
- **Producción**: el autofirmado **no está permitido** — debe emitirlo una Autoridad
  Certificadora (CA) reconocida, conforme a la Ley 527 de 1999.
- El certificado público se adjunta a la app desde "Editar aplicación" en el portal.

**JWT — claims usados por Bancolombia** (según su guía): `iss` (nombre de la app), `sub`
(client_id de la app), `aud` (audiencia / API Gateway, ej. `APIGateway_DMZ`), `exp`
(expiración — en producción no puede superar 1 minuto desde `iat`), `iat` (emisión),
`nonce` (aleatorio, >10 hex o >20 dígitos).

## 5. Errores comunes documentados

- Enviar el body como JSON en vez de `application/x-www-form-urlencoded` → error.
- No enviar las credenciales codificadas en base64 en el header `Authorization` (ej.
  enviarlas de otra forma) → error "falta el identificador del cliente" (401).
- Usar el token sin el prefijo `Bearer` → 401 por error de credenciales.

## 6. Fuentes

| Fuente | URL |
|---|---|
| API Market Bancolombia — Documentación, "Autenticación - Autorización" | https://developer-portal-public-sbx.apps.ambientesbc.com/documentacion |
| Centro de Ayuda — "Paso a paso para consumir el producto de APIs Authorization" | https://soportedevs.bancolombia.com/hc/es-419/articles/21843720412180-Paso-a-paso-para-consumir-el-producto-de-APIs-Authorization |
| Centro de Ayuda — "¿Qué es OAuth?" | https://soportedevs.bancolombia.com/hc/es-419/articles/5520302584340--Qu%C3%A9-es-OAuth |
| Centro de Ayuda — "¿Qué es Json Web Token y cómo funciona?" | https://soportedevs.bancolombia.com/hc/es-419/articles/11542467193492-JWT |
| Centro de Ayuda — "¿Qué son los certificados digitales y cómo funcionan?" | https://soportedevs.bancolombia.com/hc/es-419/articles/30664196184980--Qu%C3%A9-son-los-certificados-digitales-y-c%C3%B3mo-funcionan |
