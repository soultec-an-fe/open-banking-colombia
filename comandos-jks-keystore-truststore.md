# Comandos: empaquetar el mTLS de Bancolombia como keystore/truststore JKS

> **Fecha: 25 de septiembre de 2026.**
> **Contexto:** `bancolombia-accounts-mtls` pasó de configurar el mTLS como
> PEM sueltos en el TLS registry de Quarkus (`quarkus.tls.*.pem.*`) a
> aceptar **solo** un keystore JKS y un truststore JKS, consumidos a mano con
> `io.vertx.ext.web.client.WebClientOptions` +
> `io.vertx.core.net.JksOptions` en `BancolombiaWebClientProducer`. Este
> documento son los comandos exactos usados para generar esos dos `.jks` a
> partir de los mismos insumos que ya existían (`certificado.pem`,
> `Llave.key`, `globalsign-rsa-ov-ssl-ca-2018.pem` — ver
> `hallazgo-globalsign-intermediate-cert.md` en esta misma rama para el
> porqué de ese último archivo).

## Por qué dos pasos para el keystore

`openssl` no puede escribir directamente en formato JKS (es un formato
propietario de Java); solo sabe escribir PKCS12. Por eso el keystore se
arma en dos pasos: primero un `.p12` con `openssl`, y ese `.p12` se
convierte a `.jks` con `keytool` (que sí entiende ambos formatos). El
truststore no necesita ese paso intermedio porque solo importa certificados
públicos, algo que `keytool` hace directamente contra un `.jks` nuevo.

## 1. Keystore — identidad de cliente (`bancolombia-keystore.jks`)

**1a. `certificado.pem` + `Llave.key` → PKCS12** (paso intermedio, se borra
después):

```bash
openssl pkcs12 -export \
  -in certificado.pem -inkey Llave.key \
  -name bancolombia-mtls \
  -out bancolombia-client.p12 \
  -passout pass:changeit
```

**1b. PKCS12 → JKS:**

```bash
keytool -importkeystore \
  -srckeystore bancolombia-client.p12 -srcstoretype PKCS12 -srcstorepass changeit \
  -destkeystore bancolombia-keystore.jks -deststoretype JKS -deststorepass changeit \
  -noprompt
```

Salida real de este paso:

```
Importing keystore bancolombia-client.p12 to bancolombia-keystore.jks...
Entry for alias bancolombia-mtls successfully imported.
Import command completed:  1 entries successfully imported, 0 entries failed or cancelled

Warning:
The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an
industry standard format using "keytool -importkeystore -srckeystore bancolombia-keystore.jks
-destkeystore bancolombia-keystore.jks -deststoretype pkcs12".
```

Ese warning es esperado y se ignora a propósito: el objetivo explícito de
este cambio era terminar en JKS (para usar `io.vertx.core.net.JksOptions`),
no en PKCS12. Si en algún momento se prefiere el formato estándar de la
industria, Vert.x también tiene `PfxOptions` para PKCS12 — sería cambiar
`JksOptions` por `PfxOptions` en `BancolombiaWebClientProducer`, sin tocar
nada más.

**1c. Borrar el `.p12` intermedio** (ya no hace falta, solo el `.jks`):

```bash
rm -f bancolombia-client.p12
```

## 2. Truststore — confianza en el servidor (`bancolombia-truststore.jks`)

```bash
keytool -importcert -noprompt \
  -alias globalsign-rsa-ov-ssl-ca-2018 \
  -file globalsign-rsa-ov-ssl-ca-2018.pem \
  -keystore bancolombia-truststore.jks -storetype JKS -storepass changeit
```

Salida real:

```
Certificate was added to keystore
```

## 3. Verificación (contenido de cada almacén)

```bash
keytool -list -keystore bancolombia-keystore.jks -storepass changeit
keytool -list -keystore bancolombia-truststore.jks -storepass changeit
```

Salida real:

```
Keystore type: JKS
Keystore provider: SUN

Your keystore contains 1 entry

bancolombia-mtls, Sep 25, 2026, PrivateKeyEntry,
Certificate fingerprint (SHA-256): 65:C1:2C:DD:76:69:98:B2:E4:B0:0D:E2:8E:C9:9F:01:7C:70:B2:B0:25:61:62:66:12:3E:C8:31:FF:B6:59:F2
```

```
Keystore type: JKS
Keystore provider: SUN

Your keystore contains 1 entry

globalsign-rsa-ov-ssl-ca-2018, Sep 25, 2026, trustedCertEntry,
Certificate fingerprint (SHA-256): B6:76:FF:A3:17:9E:88:12:09:3A:1B:5E:AF:EE:87:6A:E7:A6:AA:F2:31:07:8D:AD:1B:FB:21:CD:28:93:76:4A
```

El keystore tiene exactamente 1 `PrivateKeyEntry` (la identidad de cliente);
el truststore tiene exactamente 1 `trustedCertEntry` (la CA intermedia de
GlobalSign). El fingerprint del truststore coincide con el de
`globalsign-rsa-ov-ssl-ca-2018.pem` documentado en
`hallazgo-globalsign-intermediate-cert.md`.

## 4. Cómo se consumen desde el código

```properties
bancolombia.mtls.keystore-path=.certs/bancolombia-keystore.jks
bancolombia.mtls.keystore-password=${BANCOLOMBIA_KEYSTORE_PASSWORD:changeit}
bancolombia.mtls.truststore-path=.certs/bancolombia-truststore.jks
bancolombia.mtls.truststore-password=${BANCOLOMBIA_TRUSTSTORE_PASSWORD:changeit}
```

```java
WebClientOptions options = new WebClientOptions()
        .setSsl(true)
        .setKeyStoreOptions(new JksOptions()
                .setPath(mtlsConfig.keystorePath())
                .setPassword(mtlsConfig.keystorePassword()))
        .setTrustStoreOptions(new JksOptions()
                .setPath(mtlsConfig.truststorePath())
                .setPassword(mtlsConfig.truststorePassword()));
```

Verificado end-to-end contra el sandbox real (`rs1-api-open-finance-sandbox.ambientesbc.com`)
con estos dos `.jks`: el handshake mTLS se completa y el recurso responde
`401 no_access_token: invalid access token` — el mismo resultado que con el
PEM anterior, confirmando que el cambio de formato no cambió el
comportamiento TLS.

## Nunca subir los `.jks` generados

`changeit` es solo la contraseña de ejemplo local, sobreescribible con
`BANCOLOMBIA_KEYSTORE_PASSWORD` / `BANCOLOMBIA_TRUSTSTORE_PASSWORD`.
`bancolombia-keystore.jks` contiene la **llave privada** del cliente mTLS:
no se comparte por ningún canal, ni siquiera en una rama de hallazgos como
esta — por eso este documento solo trae los comandos, no los archivos
resultantes. Cada quien los regenera localmente en
`src/main/resources/.certs/` (gitignored) a partir de sus propios
`certificado.pem` / `Llave.key`.
