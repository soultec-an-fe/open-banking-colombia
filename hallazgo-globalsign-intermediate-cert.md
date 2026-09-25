# Hallazgo: falta la CA intermedia en el handshake TLS del sandbox de Open Finance de Bancolombia

> **Fecha del hallazgo: 25 de septiembre de 2026.**
> **Alcance:** transporte TLS/mTLS hacia `rs1-api-open-finance-sandbox.ambientesbc.com`. No cubre autenticación de negocio (OAuth2/consentimientos).

## Resumen

El servidor del sandbox `rs1-api-open-finance-sandbox.ambientesbc.com` no envía su
certificado de CA intermedia durante el handshake TLS. Un cliente que solo confía en el
truststore por defecto de la JVM (que trae la raíz `GlobalSign Root CA - R3`, pero no la
intermedia `GlobalSign RSA OV SSL CA 2018`) no puede construir la cadena de confianza y
falla con `SunCertPathBuilderException`, aunque el certificado del servidor sea
perfectamente válido.

La solución es descargar esa CA intermedia por fuera (desde su propia extensión AIA) y
registrarla explícitamente como trust anchor adicional en el cliente.

## Diagnóstico paso a paso

**1. Síntoma inicial.** Con la identidad de cliente mTLS ya configurada (certificado +
llave propios), la conexión falla con:

```
SunCertPathBuilderException: unable to find valid certification path to requested target
```

Este error es de **validación de la cadena del servidor**, no de credenciales — apunta a
un problema del lado de "en quién confío", no de "quién soy".

**2. Inspeccionar qué cadena manda realmente el servidor:**

```bash
openssl s_client -connect rs1-api-open-finance-sandbox.ambientesbc.com:443 -showcerts
```

Resultado real (25-sep-2026):

```
Certificate chain
 0 s:C=CO, ST=Antioquia, L=Medellín, O=BANCOLOMBIA S.A., CN=rs1-api-open-finance-sandbox.ambientesbc.com
   i:C=BE, O=GlobalSign nv-sa, CN=GlobalSign RSA OV SSL CA 2018
```

Solo hay **una** entrada (`0`, el certificado hoja). Un handshake bien formado normalmente
incluye también la intermedia como entrada `1`; el servidor simplemente no la envía. Es
una configuración incompleta del propio sandbox, no algo corregible desde el cliente.

**3. Confirmar que la intermedia falta en el truststore de la JVM y localizarla por la
extensión AIA (Authority Information Access) del certificado hoja:**

```bash
openssl x509 -noout -text -in leaf.pem | grep -A3 "Authority Information Access"
```

Resultado real:

```
Authority Information Access:
    CA Issuers - URI:http://secure.globalsign.com/cacert/gsrsaovsslca2018.crt
    OCSP - URI:http://ocsp.globalsign.com/gsrsaovsslca2018
```

**4. Descargar la intermedia (viene en DER binario) y convertirla a PEM:**

```bash
curl -o gsrsaovsslca2018.crt http://secure.globalsign.com/cacert/gsrsaovsslca2018.crt
openssl x509 -inform DER -in gsrsaovsslca2018.crt -out globalsign-rsa-ov-ssl-ca-2018.pem
```

Ese es el archivo `globalsign-rsa-ov-ssl-ca-2018.pem` incluido en esta rama.

**5. Verificar identidad del certificado descargado** (para confirmar que es la CA
correcta y no un archivo corrupto o sustituido):

```bash
openssl x509 -in globalsign-rsa-ov-ssl-ca-2018.pem -noout -subject -issuer -fingerprint -serial
```

```
subject=/C=BE/O=GlobalSign nv-sa/CN=GlobalSign RSA OV SSL CA 2018
issuer=/OU=GlobalSign Root CA - R3/O=GlobalSign/CN=GlobalSign
SHA256 Fingerprint=B6:76:FF:A3:17:9E:88:12:09:3A:1B:5E:AF:EE:87:6A:E7:A6:AA:F2:31:07:8D:AD:1B:FB:21:CD:28:93:76:4A
serial=01EE5F221DFC623BD4333A8557
```

## Cómo se integra (ejemplo con Quarkus TLS registry)

La intermedia se registra como trust anchor adicional, **separado** de la identidad de
cliente mTLS (esa es otra mitad de la configuración, no relacionada):

```properties
# Identidad de cliente (quién soy yo) — no tiene relación con este hallazgo
quarkus.tls.bancolombia-mtls.key-store.pem.0.cert=.certs/certificado.pem
quarkus.tls.bancolombia-mtls.key-store.pem.0.key=.certs/Llave.key

# Confianza en el servidor (en quién confío yo) — esto es lo que resuelve este hallazgo
quarkus.tls.bancolombia-mtls.trust-store.pem.certs=.certs/globalsign-rsa-ov-ssl-ca-2018.pem
```

Con la intermedia registrada, la cadena cierra: hoja → intermedia (agregada manualmente) →
raíz `GlobalSign Root CA - R3` (ya presente en el truststore por defecto de la JVM). El
handshake TLS se completa y el cliente recibe una respuesta HTTP real del recurso en vez
de una excepción de validación de certificados.

## Por qué importa

Cualquier cliente (no solo Java/Quarkus) que valide estrictamente la cadena de
certificados fallará contra este sandbox si no agrega manualmente esta intermedia — el
problema no es del código cliente, es que el servidor no completa el handshake con la
cadena completa. Vale la pena verificar periódicamente con `openssl s_client -showcerts`
si Bancolombia corrige esto en el sandbox (en cuyo caso este workaround dejaría de ser
necesario) o si rota de CA (en cuyo caso este `.pem` quedaría obsoleto y habría que repetir
el proceso de la extensión AIA con el certificado nuevo).

## Certificado incluido en esta rama

| Archivo | Qué es | Origen |
|---|---|---|
| `globalsign-rsa-ov-ssl-ca-2018.pem` | CA intermedia pública de GlobalSign | Descargada de `http://secure.globalsign.com/cacert/gsrsaovsslca2018.crt` (URL tomada de la extensión AIA del certificado hoja del sandbox). No es secreta — es un certificado público de una CA comercial. |
