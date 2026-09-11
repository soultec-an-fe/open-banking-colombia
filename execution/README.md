# execution/ — Scripts deterministas

Herramientas de Capa 3 del DOE Stack: Python determinista, un propósito claro por script,
sin lógica de decisión (esa es tuya, Capa 2).

**Reglas:**
- Un script = un propósito. Si dudas entre dos scripts existentes, sobra uno.
- Entradas y salidas explícitas por argumento; nada de rutas hardcodeadas fuera del repo.
- Los intermedios van a `.tmp/`; los entregables a `docs/` o `fuentes-primarias/`.
- Secretos solo desde `.env`, nunca en el código.
- Cada script se documenta en la tabla de abajo y se referencia desde su directiva.

| Script | Qué hace | Uso |
|---|---|---|
| _(ninguno todavía)_ | — | — |
