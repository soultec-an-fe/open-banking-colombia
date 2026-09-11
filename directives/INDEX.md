# Índice de Directivas

> Una línea por directiva. Consulta **este archivo primero** para decidir qué directiva
> abrir; carga solo la relevante (contexto justo a tiempo, no precarga completa).
> Se actualiza cada vez que se agrega, renombra o cambia el alcance de una directiva.

| Directiva | Propósito | Cuándo usarla |
|---|---|---|
| _(ninguna todavía)_ | — | — |

---

## Cómo agregar una directiva

1. Crea `directives/<nombre_en_snake_case>.md` con esta estructura mínima:

```markdown
# <Nombre de la directiva>

**Objetivo:** qué debe quedar hecho cuando esta directiva termina.

**Entradas:** qué necesitas antes de empezar (archivos, URLs, variables de `.env`).

**Herramientas / scripts:** qué de `execution/` se usa y en qué orden.

**Pasos:** el flujo, numerado.

**Salidas:** qué archivos quedan y dónde (`docs/`, `fuentes-primarias/`, `.tmp/`).

**Casos extremos:** qué hacer cuando la fuente cambió, el PDF es escaneado,
la norma sigue en proyecto, el enlace murió.

**Verificación:** cómo se comprueba externamente que salió bien.
```

2. Agrega su fila en la tabla de arriba.
3. Si la directiva estrenó un script, documéntalo también en `execution/README.md`.

## Candidatas identificadas para este repo

Aún no escritas — proponer al usuario antes de crearlas (ver principio 6 de `CLAUDE.md`):

- **`actualizar_normativa_sfc.md`** — revisar *Proyectos de normatividad* de la SFC y el
  Diario Oficial, detectar si un proyecto pasó a vigente, actualizar `docs/08` y `docs/09`.
- **`descargar_fuente_primaria.md`** — descargar una norma oficial, extraer su texto a
  `fuentes-primarias/`, registrarla en `docs/99-fuentes.md` y en el README.
- **`verificar_enlaces_fuentes.md`** — comprobar que las URLs de `docs/99-fuentes.md`
  siguen vivas y que el contenido no cambió bajo la misma URL.
- **`agregar_jurisdiccion.md`** — incorporar un mercado nuevo a `docs/06` y a la
  comparativa de `docs/10` con la misma estructura de los existentes.
