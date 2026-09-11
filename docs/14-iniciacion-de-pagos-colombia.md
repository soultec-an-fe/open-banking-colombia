# 14 · Iniciación de pagos en Colombia — el régimen que ya existe

**Fecha de corte: 10 de septiembre de 2026**

> ⚠️ **Corrección importante.** En una primera lectura del cronograma de la SFC concluí que
> "la iniciación de pagos queda diferida 11 años". Eso es **impreciso**. Lo que está
> diferido son los **estándares de intercambio de información** que debe expedir la SFC.
> **La actividad de iniciación de pagos ya está regulada y habilitada en Colombia desde
> julio de 2022**, en un título distinto del Decreto 2555 de 2010.

---

## 1. Dónde vive la iniciación de pagos

**No está en el Título 8 (finanzas abiertas). Está en el Título 4 del Libro 17 de la
Parte 2 del Decreto 2555 de 2010**, añadido por el **artículo 4 del Decreto 1297 de 2022**.

Es decir: el mismo decreto que creó las finanzas abiertas voluntarias creó, **por
separado**, el régimen de iniciación de pagos. El Decreto 0368 de 2026 derogó lo primero
pero **solo modificó un inciso** de lo segundo.

## 2. Qué dice el régimen vigente

### Art. 2.17.4.1.1 — Quién puede iniciar pagos

> La actividad puede ser desarrollada por: **establecimientos de crédito**, **SEDPE**,
> **entidades administradoras de sistemas de pago de bajo valor (EASPBV)** y
> **sociedades NO vigiladas por la Superintendencia Financiera de Colombia**.

🔑 **Esto es decisivo y contradice la lectura simple del Decreto 0368.** En finanzas
abiertas, un actor no vigilado **no tiene derecho de acceso** — depende de acuerdos
bilaterales. Pero en **iniciación de pagos, una sociedad no vigilada puede desarrollar la
actividad directamente**, sin licencia de la SFC.

Condiciones:
- Requiere **autorización previa del ordenante**.
- Debe tramitarse **a través de una EASPBV**.
- El iniciador **no puede administrar ni entrar en tenencia de los fondos** del ordenante.

**Parágrafo del art. 3 del Decreto 1297 de 2022:** el **Banco de la República**, como
administrador de sistemas de pago de bajo valor, también puede desarrollar la actividad de
iniciación de pagos.

### Art. 2.17.4.1.2 — Acceso de los iniciadores (derecho de acceso real)

Para garantizar libre acceso y promoción de la competencia:

1. Las EASPBV **se abstendrán de restringir arbitrariamente** el acceso de iniciadores, y
   aplicarán **las mismas condiciones y trato** a todas las órdenes de pago.
2. El sistema y sus participantes **no podrán bloquear arbitrariamente** las órdenes
   iniciadas, ni **pactar exclusividad**.
3. Las EASPBV deben cumplir frente a los iniciadores los deberes del art. 2.17.2.1.5.
4. Los iniciadores deben cumplir las **reglas y estándares operativos, técnicos y de
   seguridad** que establezca la EASPBV.
5. Las EASPBV deben **informar** las características del sistema y los **requisitos y
   costos de acceso**.

### Art. 2.17.4.1.3 — Reglas de operación

Toda EASPBV debe incluir en su reglamento, como mínimo:

1. Los iniciadores **no pueden iniciar órdenes sin autorización previa** del ordenante.
2. **Cada orden** de pago o transferencia debe ser autorizada por el ordenante.
3. Las **entidades emisoras deben autenticar al ordenante en todos los casos**. La
   autenticación y la confirmación del resultado se hacen **según las reglas que dicte la
   SFC**, y el resultado debe informarse al iniciador **a través de la EASPBV**.
4. Los iniciadores **no pueden pedir más información de la estrictamente necesaria** y
   **en ningún caso pueden acceder a claves, contraseñas o mecanismos de autenticación**
   del ordenante con su entidad emisora. *(Prohibición efectiva del screen scraping.)*

**Inciso final — modificado por el art. 2 del Decreto 0368 de 2026:**

> *"La Superintendencia Financiera de Colombia expedirá los estándares necesarios para que
> los servicios de iniciación de pagos, inmediatos o no inmediatos y recurrentes o no
> recurrentes, se ejecuten en condiciones de seguridad, transparencia y eficiencia. Estas
> instrucciones se impartirán sin perjuicio de las disposiciones que expida la Junta
> Directiva del Banco de la República en ejercicio de la facultad establecida en el
> artículo 104 de la Ley 2294 de 2023."*

Antes de esa reforma el texto era una **facultad** ("podrá impartir instrucciones"). Ahora
es una **obligación** ("expedirá los estándares") y se amplía explícitamente a pagos
**recurrentes** — que es la puerta a un equivalente colombiano del **VRP** británico.

### Arts. 2.17.4.1.4 y 2.17.4.1.5 — Conflictos de interés

Si una EASPBV (o sus filiales, subsidiarias, controlantes o accionistas) desarrolla también
iniciación de pagos:
- Las solicitudes de acceso las decide el **Comité de Acceso** (art. 2.17.2.1.8).
- Debe tener un capítulo específico de políticas de **conflictos de interés**, con
  separación **decisoria, física y operativa** de las áreas susceptibles de conflicto.

## 3. Bre-B — la infraestructura

**Bre-B** es el sistema de pagos inmediatos interoperado de Colombia, operado por el
**Banco de la República**.

| Norma | Contenido |
|---|---|
| **Resolución Externa 6 del 31 de octubre de 2023** | Reglas de interoperabilidad de los Sistemas de Pago de Bajo Valor Inmediatos. Expedida al amparo del **art. 104 de la Ley 2294 de 2023** |
| **Circular Reglamentaria DSP-465** (últ. act. agosto de 2026) | Estándares de interoperabilidad para las entidades administradoras |
| **Circular DSP-470** | **DICE** — Directorio Centralizado |
| **Circular DSP-471** | **MOL** — Mecanismo Operativo de Liquidación |

Cifras: entre el **6 de octubre de 2025** y el **31 de enero de 2026** se liquidaron
**370,4 millones de operaciones** por **$59 billones**, con un valor promedio de
**$159.456** por transacción.

> ⚠️ La regulación de Bre-B publicada por el Banco de la República regula la
> **interoperabilidad entre administradores de sistemas de pago**, no el acceso de terceros
> iniciadores. El derecho de acceso del iniciador vive en el **Decreto 2555, art. 2.17.4.1.2**,
> y se materializa en el **reglamento de cada EASPBV**.

## 4. Qué falta realmente

| Pieza | Estado |
|---|---|
| Figura del iniciador de pagos | ✅ **Existe** (art. 2.17.4.1.1, desde julio de 2022) |
| Apertura a sociedades no vigiladas | ✅ **Existe** |
| Derecho de acceso a sistemas de pago | ✅ **Existe** (art. 2.17.4.1.2) |
| Prohibición de acceso a credenciales | ✅ **Existe** (art. 2.17.4.1.3 num. 4) |
| Infraestructura de pagos inmediatos | ✅ **Existe** (Bre-B, operando desde oct-2025) |
| Reglas de autenticación y confirmación de la SFC | ⏳ Pendientes (art. 2.17.4.1.3 num. 3) |
| **Estándares de la SFC para iniciación de pagos** | ⏳ **Mes 132 en el cronograma borrador (~2037)** |
| Reglas de pagos recurrentes / VRP colombiano | ⏳ Dentro de esos estándares |

## 5. Lectura corregida

**El cronograma de 132 meses no bloquea la iniciación de pagos.** Bloquea la
**estandarización** de esos servicios dentro del sistema de finanzas abiertas.

Lo que sí implica, y sigue siendo un problema real:

1. **Fragmentación.** Sin estándar de la SFC, cada EASPBV define sus propias reglas
   operativas, técnicas y de seguridad en su reglamento (art. 2.17.4.1.2 num. 4). Un
   iniciador que quiera operar en varias EASPBV integra varias veces. Es exactamente el
   problema que PSD2 tuvo en Europa por no imponer un estándar único.
2. **Sin pagos recurrentes estandarizados.** El equivalente al VRP británico —el caso de
   uso que sostiene el modelo de negocio del open banking de pagos— queda sin marco común.
3. **Autenticación sin reglas comunes.** El art. 2.17.4.1.3 num. 3 remite a "las reglas que
   para el efecto dicte la SFC". Mientras no existan, cada emisora autentica a su manera.

> **Conclusión práctica:** si el objetivo es hacer iniciación de pagos en Colombia,
> **no hay que esperar al cronograma de finanzas abiertas**. El camino es el Título 4 del
> Libro 17 y el reglamento de la EASPBV correspondiente. Lo que se gana esperando es
> estandarización, no habilitación.

## 6. Comentario sugerido a la SFC, reformulado

En vez de pedir que se adelante la fila de "servicios de iniciación de pagos" sin más,
el comentario técnicamente correcto es:

> Adelantar, con prioridad y de forma separada del resto del cronograma, **las reglas de
> autenticación y confirmación de la operación** previstas en el numeral 3 del artículo
> 2.17.4.1.3 del Decreto 2555 de 2010, y los **estándares de pagos recurrentes**. La
> actividad de iniciación de pagos ya está habilitada desde el Decreto 1297 de 2022 y opera
> hoy sobre infraestructura de pagos inmediatos (Bre-B); la ausencia de estándares comunes
> de la SFC no impide la actividad, pero sí genera fragmentación entre reglamentos de
> EASPBV y encarece la integración de los iniciadores, especialmente los no vigilados.
