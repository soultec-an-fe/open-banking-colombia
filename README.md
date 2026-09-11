# Open Banking / Open Finance — Investigación y Documentación de Referencia

> **Fecha de corte de la investigación: 10 de septiembre de 2026.**
> Toda afirmación normativa está fechada. Las normas en trámite (proyectos, consultas
> públicas, litigios) se marcan explícitamente como **no vigentes**.

Este repositorio documenta cómo se aplica Open Banking / Open Finance en las
jurisdicciones de referencia (Reino Unido, Unión Europea, Estados Unidos, Brasil,
Chile, México, Australia, India), el estándar de seguridad **FAPI** de la OpenID
Foundation, y el estado **actual y detallado de la regulación colombiana**, que
cambió radicalmente en 2026.

---

## Hallazgo principal

Colombia pasó de un esquema **voluntario** a uno **obligatorio** el 7 de abril de 2026
con el **Decreto 0368 de 2026**, y añadió **portabilidad financiera** como servicio del
sistema con el **Decreto 0977 de 2026** (4 de agosto de 2026).

Hay **dos ventanas regulatorias abiertas ahora mismo**:

| Qué | Estado | Fecha límite |
|---|---|---|
| Proyecto de Carta Circular — **cronograma de estándares** | En consulta pública | **15 de septiembre de 2026, 5:00 p.m.** → `finanzasabiertas@superfinanciera.gov.co` |
| Proyecto de Circular Externa — **nuevo Capítulo IX CBJ** (mantiene **FAPI 2.0**, añade OpenAPI 3.1) | Comentarios cerrados el 11-ago-2026; **aún no expedida** | Expedición pendiente |
| SFC debe publicar el cronograma definitivo | Obligación legal | **10 de octubre de 2026** (6 meses desde vigencia del D-0368) |

👉 Ver el detalle y el análisis crítico del cronograma en
[`docs/09-colombia-cronograma-y-analisis.md`](docs/09-colombia-cronograma-y-analisis.md).

---

## Índice de la documentación

| # | Documento | Contenido |
|---|---|---|
| 00 | [Resumen ejecutivo](docs/00-resumen-ejecutivo.md) | Lo esencial en 5 minutos + tablero de fechas |
| 01 | [Conceptos y modelos regulatorios](docs/01-conceptos-y-modelos.md) | Open Banking vs Open Finance vs Open Data; modelos regulatorio / de mercado / híbrido |
| 02 | [Reino Unido](docs/02-reino-unido.md) | CMA Order, OBIE→OBL, JROC, Future Entity, cVRP/UKPI, DUAA 2025, roadmap FCA a 2030 |
| 03 | [Unión Europea](docs/03-union-europea.md) | PSD2/RTS-SCA, PSD3+PSR, FIDA (estancado), eIDAS 2 |
| 04 | [Estados Unidos](docs/04-estados-unidos.md) | CFPB §1033, litigio Forcht Bank, ANPR/NPRM 2026, FDX, Nueva York, GUARD Act |
| 05 | [Brasil](docs/05-brasil.md) | Open Finance Brasil por fases, JSR, portabilidad de crédito, FAPI |
| 06 | [Otros mercados](docs/06-otros-mercados.md) | Chile (Ley Fintec/NCG 514), México, Australia (CDR), India (AA), BIS Project Aperta |
| 07 | [FAPI y seguridad de APIs](docs/07-fapi-y-seguridad.md) | FAPI 1.0 vs 2.0, PAR, DPoP, mTLS, private_key_jwt, conformance, checklist |
| 08 | [Colombia — marco normativo](docs/08-colombia-marco-normativo.md) | Ley 2294/2023, D-1297/2022, **D-0368/2026**, **D-0977/2026**, CE 004/2024 y sus prórrogas |
| 09 | [Colombia — cronograma y análisis crítico](docs/09-colombia-cronograma-y-analisis.md) | Cronograma borrador de la SFC, fechas proyectadas, riesgos e inconsistencias detectadas |
| 10 | [Comparativa internacional](docs/10-comparativa-internacional.md) | Tabla lado a lado de 8 jurisdicciones |
| 11 | [Arquitectura de referencia](docs/11-arquitectura-de-referencia.md) | Blueprint técnico para implementar en Colombia |
| 12 | [Plan de trabajo](docs/12-plan-de-trabajo.md) | Backlog priorizado, decisiones abiertas, entregables |
| 13 | [Glosario de siglas](docs/13-glosario-de-siglas.md) | OBIE, OBL, FAPI, PAR, SEDPE, SARLAFT… todas las siglas de los 8 mercados y del stack técnico |
| 14 | [Iniciación de pagos en Colombia](docs/14-iniciacion-de-pagos-colombia.md) | El régimen **ya vigente** del Título 4 Libro 17, Bre-B, y qué falta realmente |
| 99 | [Fuentes](docs/99-fuentes.md) | Bibliografía completa con URLs verificadas |

---

## Cómo mantener esto vivo

1. **Suscribirse** a la sección *Proyectos de normatividad* de la SFC
   (`superfinanciera.gov.co`) — es donde aparecen primero los estándares.
2. Revisar el **Diario Oficial** para decretos del MinHacienda/URF que modifiquen el
   Decreto 2555 de 2010, Libro 35, Parte 2, **Título 8**.
3. Seguir el **FAPI Working Group** de la OpenID Foundation (`openid.net/wg/fapi/`) —
   la SFC ancla su perfil de seguridad ahí, con cláusula de actualización automática
   cuando el estándar se declare obsoleto.
4. Cada vez que se actualice un documento, dejar la fecha de corte en el encabezado.

---

## Fuentes primarias descargadas

En [`fuentes-primarias/`](fuentes-primarias/) quedan los textos completos extraídos
directamente de la fuente oficial, para consulta sin conexión:

| Archivo | Contenido |
|---|---|
| `decreto-0368-2026-texto.txt` | Texto íntegro del Decreto 0368 de 2026 (considerandos + arts. 2.35.8.x) |
| `decreto-0977-2026-texto.txt` | Texto íntegro del Decreto 977 de 2026 (portabilidad financiera) |
| `proyecto-carta-circular-cronograma-2026.txt` | Borrador del cronograma de estándares con la tabla de plazos |
| `proyecto-capitulo-IX-CBJ-2026.txt` | Borrador del nuevo Capítulo IX de la CBJ (estándares FAPI 2.0) |
| `proyecto-circular-externa-cap-IX-considerandos.txt` | Considerandos y régimen de transición del proyecto de circular |
| `fca-open-finance-roadmap-abril-2026.txt` | Roadmap de open finance de la FCA (abril 2026) |
| `circular-externa-004-2024.pdf` + `-anexo.pdf` | Circular Externa 004 de 2024 y su anexo con el Capítulo IX completo (PDF escaneado, leído por imagen) |
| `circular-externa-009-2025.pdf` · `circular-externa-001-2026.pdf` | Las dos prórrogas del plazo de adopción de los estándares |
| `urf-documento-tecnico-finanzas-abiertas-2024.txt` | Documento Técnico de la URF que sustentó el decreto — incluye reciprocidad y Terceros de Confianza, ambos eliminados |
| `decreto-1297-2022-texto.txt` | Decreto 1297 de 2022, incluido el Título 4 del Libro 17 (iniciación de pagos), **vigente** |
