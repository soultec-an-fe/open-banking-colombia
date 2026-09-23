# Instrucciones para el Agente — DOE Stack

> **DOE Stack** (**D**irective → **O**rchestration → **E**xecution) es el nombre de esta arquitectura: SOPs en Markdown que dirigen a un agente que decide, el cual llama scripts deterministas que hacen el trabajo. Ver [Aprendizajes del Agente](#aprendizajes-del-agente-mejora-continua) para el porqué del nombre.

> Si `CLAUDE.md`, `AGENTS.md` y `GEMINI.md` no existen en este directorio, créalos con el contenido íntegro de este archivo, y crea la estructura de carpetas descrita en "Organización de Archivos". Los tres archivos deben ser **siempre idénticos**: cualquier cambio o aprendizaje registrado en uno se replica de inmediato en los otros dos, para que el sistema cargue igual en cualquier entorno de IA agéntica (Claude Code, Codex/AGENTS.md, Gemini CLI).

## Aprendizajes del Agente (Mejora Continua)

> **INSTRUCCIÓN CRÍTICA — LEER PRIMERO:** Esta sección es tu memoria persistente. **Con cada ciclo de ejecución** (tarea completada, error resuelto, patrón descubierto, flujo ajustado) **y con cada actualización de cualquier Markdown** (directivas, `CLAUDE.md`/`AGENTS.md`/`GEMINI.md`, READMEs de scripts), agrega aquí un aprendizaje nuevo si surgió algo no trivial. El objetivo: que este archivo sea más útil y preciso con el tiempo, sin perderse entre sesiones.
>
> **Qué registrar:** restricciones de APIs descubiertas, rate limits reales, patrones que funcionan, errores que se repiten, decisiones tomadas con el usuario, supuestos falsos, atajos útiles, gotchas del entorno.
>
> **Qué NO registrar:** detalles efímeros de una sola tarea, cosas ya documentadas en la directiva correspondiente, cosas triviales derivables del código.
>
> **Formato:**
> ```
> - **YYYY-MM-DD — [Tema corto]:** Aprendizaje en 1-3 líneas. **Por qué importa:** consecuencia práctica.
> ```
>
> **Higiene:** si un aprendizaje queda obsoleto, actualízalo o elimínalo en vez de acumular ruido. Orden: más reciente arriba. Por encima de ~25 entradas, consolida las más antiguas o promuévelas a la directiva que corresponda.

### Registro de aprendizajes

- **2026-09-23 — La URF también publica proyectos de *decreto*, no solo la SFC circulares:** `urf.gov.co/normatividad/proyectos-de-decreto/2026` tiene ventanas de comentarios muy cortas (4 a 15 días) y puede modificar los decretos habilitantes mismos (D-0368/D-0977), no solo instrucciones de la SFC. **Por qué importa:** hay que vigilar esa página además de la de la SFC — un decreto pesa más que una circular y puede cambiar el punto de partida de todo `docs/09`. Caso real: proyecto de ampliación de plazos publicado 22-sep-2026, cierre 27-sep-2026. Ver `docs/09` §7 y memoria `colombia-proyecto-ampliacion-plazos-sep2026`.
- **2026-09-23 — Nunca asumir la fecha base de un plazo sin leer el artículo de vigencia:** el D-0368/2026 art. 5 fija su vigencia al día siguiente de publicación en el Diario Oficial (10-abr-2026), pero el propio documento técnico de la URF que sustenta su modificación calcula los mismos plazos desde la fecha de *expedición* (7-abr-2026) — 3 días de diferencia entre dos fuentes oficiales, sin explicación. **Por qué importa:** al citar cualquier fecha derivada de "X meses desde la vigencia", verificar el artículo de vigencia en la fuente primaria y no confiar en el cómputo de un documento secundario, aunque sea oficial; si hay discrepancia, documentarla explícitamente en vez de elegir una.
- **2026-09-11 — Las cipher suites de la CE 004/2024 obligan a certificados RSA:** ambas suites permitidas son `ECDHE_RSA`, así que un certificado mTLS con llave ECDSA no negocia y deja a la entidad fuera de norma; además son suites de TLS 1.2, la lista no cubre 1.3. **Por qué importa:** hay que pedir explícitamente RSA (≥2048, 3072 recomendado) a la ECD acreditada por ONAC, y soportar 1.2 + 1.3 en paralelo. Detalle en `docs/11`, sección 6-bis.
- **2026-09-10 — Este repo es de investigación normativa, no de software:** el "producto" son los Markdown de `docs/` y los textos oficiales de `fuentes-primarias/`; no hay build ni suite de tests. **Por qué importa:** la "verificación externa" aquí no es correr tests, es contrastar cada afirmación contra la fuente primaria descargada o contra el sitio oficial, y dejar la URL en `docs/99-fuentes.md`.
- **2026-09-10 — Vigente vs. proyecto es la distinción que más se rompe:** hay normas expedidas (D-0368/2026, D-0977/2026, CE 004/2024) conviviendo con borradores en consulta (Carta Circular de cronograma, Capítulo IX CBJ). **Por qué importa:** confundirlas invalida el documento entero; todo texto en trámite se marca explícitamente como **no vigente** y con su fecha de corte.
- **2026-09-10 — Hay relojes corriendo (actualizado 2026-09-23):** la consulta del cronograma cerró el 15-sep-2026 (aún sin expedir); ahora corre además el proyecto de decreto de ampliación de plazos, cierre **27-sep-2026 11:59 p.m.**; y el tope legal vigente para el cronograma es 7 u 10 de oct-2026 (discrepancia sin resolver, ver entrada de arriba). **Por qué importa:** cualquier sesión posterior a esas fechas debe verificar primero en `urf.gov.co` y el Diario Oficial si ya se expidió algo antes de reusar fechas de aquí — este es el tercer round de plazos moviéndose en menos de dos semanas.

<!-- Agrega nuevas entradas arriba de esta línea. -->

---

Operas dentro de una arquitectura de 3 capas que separa responsabilidades para maximizar la confiabilidad. Los LLM son probabilísticos; la mayoría de la lógica de negocio es determinista y exige consistencia. Un 90% de precisión por paso da solo 59% de éxito acumulado en 5 pasos — por eso la complejidad se empuja hacia código determinista y tú te concentras en la toma de decisiones.

## La Arquitectura de 3 Capas (DOE Stack)

**Capa 1 — Directiva (Qué hacer):** SOPs en Markdown, en `directives/`. Objetivo, entradas, herramientas/scripts a usar, salidas, casos extremos. Lenguaje natural, como instrucciones a un empleado de nivel medio.

**Capa 2 — Orquestación (Tú):** Enrutamiento inteligente. Lees directivas, llamas scripts de ejecución en el orden correcto, manejas errores, pides aclaraciones solo cuando estás genuinamente bloqueado, actualizas directivas con lo aprendido. Eres el puente entre intención y ejecución: no hagas scraping por tu cuenta — lee `directives/scrape_website.md`, define entradas/salidas y ejecuta `execution/scrape_single_site.py`.

**Capa 3 — Ejecución (Scripts):** Python determinista en `execution/`. Llamadas a APIs, procesamiento de datos, operaciones de archivos, interacciones con bases de datos. Confiables, testeables, rápidos.

## Principios de Operación

**1. Contexto justo a tiempo, no precarga completa.** No leas todas las directivas al iniciar una tarea. Consulta primero `directives/INDEX.md` (una línea por directiva) para decidir cuál abrir, y carga solo la relevante. Esto reduce ruido de contexto y evita decisiones basadas en información desactualizada de otras directivas.

**2. Revisa primero si existen herramientas.** Antes de escribir un script nuevo, revisa `execution/` según tu directiva. Cada script debe tener un propósito único y claro — evita crear dos scripts que hagan casi lo mismo; si un humano no podría decidir cuál usar, tú tampoco podrás.

**3. Progreso incremental, no completitud de una sola vez.** Completa y verifica una directiva o funcionalidad a la vez antes de pasar a la siguiente. No dejes estados a medio terminar ni intentes resolver todo el flujo en un solo paso largo sin checkpoints.

**4. Verificación externa obligatoria.** No declares éxito basándote en tu propio resumen. Ejecuta el script, corre el test, lee la salida real. Si la evidencia es incompleta, dilo explícitamente en vez de asumir que funcionó.

**5. Auto-corrección cuando algo falla.**
- Lee el error y el stack trace completo.
- Corrige el script y pruébalo de nuevo (si usa tokens/créditos de pago, consulta primero con el usuario).
- Actualiza la directiva con lo aprendido (límites de API, tiempos, casos extremos).
- Ejemplo: rate limit de una API → investigas la API → encuentras un endpoint batch → reescribes el script → pruebas → actualizas la directiva.

**6. Actualiza las directivas a medida que aprendes.** Son documentos vivos: cuando descubras restricciones de API, mejores enfoques, errores comunes o tiempos esperados, actualízalas. No crees ni sobreescribas directivas sin preguntar, salvo instrucción explícita — son tu conjunto de instrucciones y deben preservarse y mejorarse, no improvisarse y descartarse.

**7. Control de versiones incremental.** Si el directorio es un repositorio git, haz commits pequeños y descriptivos después de cada unidad de trabajo verificada (script nuevo, directiva actualizada, fix). Esto permite revertir a un estado funcional sin depender de tu memoria de lo que cambió.

## Ritual de Inicio de Sesión

Antes de ejecutar cualquier tarea nueva:
1. Lee el "Registro de aprendizajes" de este archivo.
2. Revisa `directives/INDEX.md` para ubicar la directiva relevante.
3. Verifica el entorno (`.env` presente, dependencias instaladas) antes de asumir que algo funciona.
4. Si hay trabajo previo en `.tmp/`, revisa si es reutilizable o si debe regenerarse.

## Ciclo de Auto-corrección

1. Corrige el problema.
2. Actualiza la herramienta.
3. Pruébala y confirma que funciona (verificación externa, no autoevaluación).
4. Actualiza la directiva con el nuevo flujo.
5. Registra el aprendizaje si fue no trivial.
6. El sistema queda más robusto.

## Organización de Archivos

- `.tmp/` — archivos intermedios (dossiers, datos scrapeados, exportaciones temporales). Nunca se suben al repositorio; siempre se regeneran.
- `directives/` — SOPs en Markdown (el conjunto de instrucciones).
- `directives/INDEX.md` — una línea por directiva: nombre, propósito, cuándo usarla. Se actualiza cada vez que se agrega o cambia una directiva.
- `execution/` — scripts de Python (las herramientas deterministas).
- `.env` — variables de entorno y claves de API. Nunca se sube al repositorio.
- `credentials.json`, `token.json` — credenciales OAuth de Google (solo cuando el flujo los requiera; en `.gitignore`).

**Principio clave:** los archivos intermedios viven en `.tmp/` y pueden borrarse siempre. Cualquier salida del flujo debe ser reproducible ejecutando el flujo de nuevo, nunca editada a mano.

**Nota sobre plantillas genéricas:** este archivo es una plantilla base reusable entre proyectos. Al instanciarlo en un repositorio concreto, agrega ahí — no aquí — los comandos exactos de build/test, convenciones de estilo con un ejemplo de código por convención, y límites del proyecto (qué no tocar). Una instrucción específica con un ejemplo vale más que un párrafo genérico.

## Resumen

Estás entre la intención humana (directivas) y la ejecución determinista (scripts de Python). Lee instrucciones, toma decisiones, llama herramientas, maneja errores y mejora el sistema continuamente.

Sé pragmático. Sé confiable. Auto-corrígete.

---

# Instancia del proyecto — Open Banking / Open Finance

> Esta sección es lo específico de **este** repositorio. Lo de arriba es la plantilla base;
> lo de aquí abajo manda cuando hay conflicto.

## Qué es este repositorio

Investigación y documentación de referencia sobre Open Banking / Open Finance en 8
jurisdicciones (UK, UE, EE. UU., Brasil, Chile, México, Australia, India), el estándar
**FAPI** de la OpenID Foundation, y el estado **actual** de la regulación colombiana.
El entregable es prosa técnica en Markdown, no software. Ver `README.md` para el índice.

**Fecha de corte vigente de la investigación: 10 de septiembre de 2026.**

## Estructura real

| Ruta | Qué contiene | Regla |
|---|---|---|
| `README.md` | Índice maestro + hallazgo principal + tablero de ventanas regulatorias abiertas | Se actualiza cada vez que se agrega o renombra un documento de `docs/` |
| `docs/00`–`docs/99` | Los 16 documentos de la investigación, numerados | Cada archivo abre con su fecha de corte |
| `fuentes-primarias/` | Textos oficiales completos (`.txt` extraídos, `.pdf` originales) | **Solo lectura.** Nunca se editan a mano |
| `directives/` | SOPs de los flujos repetibles de este repo | `INDEX.md` primero, luego la directiva |
| `execution/` | Scripts Python deterministas (descarga, extracción, verificación de enlaces) | Un propósito por script |
| `entregables/` | Documentos finales para terceros (p. ej. la matriz de comentarios al proyecto de cronograma) | Se generan desde `docs/`; no son fuente de verdad |
| `.tmp/` | Descargas crudas, HTML intermedio, diffs de normas | Desechable, siempre regenerable |

## Comandos

No hay build ni suite de tests. La "verificación" de este repo es documental:

```bash
# Verificar que los enlaces de docs/99-fuentes.md siguen vivos
python3 execution/verify_links.py docs/99-fuentes.md   # (script pendiente de crear)

# Buscar una afirmación en las fuentes primarias antes de citarla
grep -rn "portabilidad" fuentes-primarias/*.txt

# Extraer texto de un PDF nuevo hacia fuentes-primarias/
python3 execution/extract_pdf_text.py fuentes-primarias/<archivo>.pdf   # (script pendiente)
```

Si un script referido arriba no existe todavía, créalo bajo su directiva — no improvises
el trabajo a mano en una sesión y lo repitas distinto en la siguiente.

## Convenciones de escritura (con ejemplo cada una)

**1. Toda afirmación normativa va fechada y con su norma.**
```markdown
El régimen es obligatorio desde el **7 de abril de 2026** (Decreto 0368 de 2026, art. 2.35.8.1.1).
```

**2. Lo que está en trámite se marca como no vigente, en negrita, siempre.**
```markdown
> **NO VIGENTE.** Proyecto de Carta Circular en consulta pública hasta el
> **15 de septiembre de 2026, 5:00 p.m.** No genera obligaciones mientras no se expida.
```

**3. Las fechas límite van en tabla, no en prosa suelta.**
```markdown
| Qué | Estado | Fecha límite |
|---|---|---|
| Cronograma definitivo de la SFC | Obligación legal | 10 de octubre de 2026 |
```

**4. Cada dato duro se ancla a la fuente primaria descargada, por archivo.**
```markdown
Ver `fuentes-primarias/proyecto-capitulo-IX-CBJ-2026.txt`, numeral 2.3 (perfil FAPI 2.0).
```

**5. Encabezado de fecha de corte al inicio de cada documento de `docs/`.**
```markdown
> **Fecha de corte: 10 de septiembre de 2026.**
```

## Límites del proyecto (qué NO tocar)

- **No editar `fuentes-primarias/`.** Son textos oficiales tal como se publicaron. Si algo
  está mal extraído, se vuelve a extraer del original y se anota en la directiva; no se
  corrige a mano el `.txt`.
- **No mover una norma de "proyecto" a "vigente"** sin haber visto la expedición en el
  Diario Oficial o en el sitio de la SFC, y sin dejar la URL en `docs/99-fuentes.md`.
- **No cambiar una fecha de corte** sin haber re-verificado el contenido que cubre.
- **No inventar números de artículo, circulares ni fechas.** Si no está en
  `fuentes-primarias/` ni verificado en la fuente oficial, se escribe como pendiente de
  verificación, no como hecho.
- **No renumerar los archivos de `docs/`** — el `README.md` y las referencias cruzadas
  dependen de esa numeración.

## Mantenimiento recurrente

1. Sección *Proyectos de normatividad* de la SFC (`superfinanciera.gov.co`) — ahí aparecen
   primero los estándares.
2. Diario Oficial, para decretos MinHacienda/URF que modifiquen el Decreto 2555 de 2010,
   Libro 35, Parte 2, **Título 8**.
3. FAPI Working Group de la OpenID Foundation (`openid.net/wg/fapi/`) — la SFC ancla su
   perfil de seguridad ahí, con actualización automática si el estándar se declara obsoleto.
4. Al actualizar cualquier documento, mover su fecha de corte y registrar el aprendizaje si
   fue no trivial.
