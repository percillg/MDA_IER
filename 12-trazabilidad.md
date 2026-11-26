# Trazabilidad

## Matriz de trazabilidad

| Requisito (Entrega 1) | Historia | Caso de uso | Secuencia | Estado | Clases del dominio |
|-----------------------|----------|-------------|-----------|--------|---------------------|
| RP-01 | HU-01 | CU-01 | DSS-01 | EST-01 | Oferta Académica, Sección, Asignatura |
| RP-02 | HU-01 | CU-02, CU-03, CU-04 | DSS-01 | EST-01 | Oferta Académica, Sección, Asignatura |
| RP-03 | HU-04 | CU-14, CU-16 | — | — | Asignatura, Asignatura Cursada, Carrera |
| RP-04 | HU-05 | CU-17, CU-18 | — | — | Estudiante, Asignatura, Asignatura Cursada, Carrera |
| RP-05 | HU-02 | CU-05 | DSS-02 | — | Simulación de Horario, Sección, Asignatura |
| RP-06 | HU-03 | CU-12, CU-13 | — | — | Evaluación, Asignatura, Sección |
| RP-07 | HU-02 | CU-05 | DSS-02 | — | Simulación de Horario, Sección, Carrera |
| RP-08 | — | — | — | — | Simulación de Horario |
| RP-09 | HU-01 | CU-01, CU-02 | DSS-01 | EST-01 | Oferta Académica, Sección |
| RP-10 | HU-02 | CU-05 | DSS-02 | — | Simulación de Horario, Sección |
| RP-11 | HU-03 | — | — | — | Evaluación, Asignatura |
| RP-12 | — | — | — | — | Evaluación |

## Trazabilidad del cambio (Evaluación 2)

| Cambio | Historia / criterio | Caso de uso | Secuencia | Estado | Clases del dominio |
|--------|---------------------|-------------|-----------|--------|--------------------|
| Asignatura en estado `tentativo` visible en el planificador y confirmable desde ahí | HU-04 (marca en malla) + HU-02 CA4 (confirma) | CU-15, CU-06, extensión de CU-07 | DSS-03 | EST-02 | Recomendación de Asignatura, Simulación de Horario, Asignatura |
| Recomendación de avance académico | HU-05 CA5 | CU-17 | DSS-03 | EST-02 | Recomendación de Asignatura, Asignatura, Asignatura Cursada |

## Observaciones

- RP-08 (guardar o exportar la simulación) y RP-12 (moderación de comentarios) no tienen historia de usuario en la Entrega 1, por lo que no se asocian a casos de uso ni a diagramas. RP-08 corresponde a la actividad "Confirmar e inscribir horario simulado" del TO-BE.
- La numeración vigente es la de `09-casos-uso.md`: HU-01 CU-01…CU-04; HU-02 CU-05…CU-11; HU-03 CU-12…CU-13; HU-04 CU-14…CU-16; HU-05 CU-17…CU-18. El cambio de Evaluación 2 conserva esa numeración (CU-06 confirmar tentativa, CU-15 marcar tentativa) y agrega DSS-03 y EST-02 para HU-02/HU-04/HU-05.
- Simulación de Horario sigue sin diagrama de estado propio (en armado y confirmada); el cambio de Evaluación 2 incorporó EST-02 para la clase Recomendación de Asignatura, con transición `tentativo` → `confirmada` → fin o `descartada` → fin.
## Trazabilidad HU → código → prueba

| Historia | Ruta | Código | Prueba | Cambio OpenSpec archivado |
|---|---|---|---|---|
| HU-01 | /carga | `src/vistas/parciales/cabecera.ejs`, `src/vistas/parciales/mensajes.ejs` *( rutas/controladores según repo)* | `tests/hu-01.test.ts`, `tests/integracion-hu-01.test.ts`, `tests/leer-planilla.test.ts`, `tests/parsear-oferta.test.ts`, `tests/publicar-oferta.test.ts` | *(revisar carpeta en archive/)* |
| HU-02 | /planificador | `src/vistas/planificador.ejs`, `src/vistas/parciales/grilla.ejs`, `src/vistas/parciales/secciones.ejs` | `tests/hu-02.test.ts`, `tests/oferta-activa.test.ts`, `tests/estado-oferta.test.ts` | *(revisar carpeta en archive/)* |
| HU-03 | /planificador | `src/vistas/planificador.ejs`, `src/vistas/parciales/grilla.ejs`, `src/vistas/parciales/secciones.ejs`, `src/vistas/parciales/metricas.ejs` | `tests/hu-03.test.ts` | *(revisar carpeta en archive/)* |
| HU-04 | /malla | `src/vistas/parciales/grilla.ejs`, `src/vistas/parciales/metricas.ejs` | `tests/hu-04.test.ts` | *(revisar carpeta en archive/)* |
| HU-05 | /tiempo-restante | `src/vistas/tiempo-restante.ejs`, `src/vistas/parciales/cabecera.ejs`, `src/vistas/parciales/metricas.ejs` | `tests/hu-05.test.ts` | `2026-11-23-hu-05-tiempo-restante` |
- HU-01 publica la oferta activa que consume HU-02 y conserva la identidad de **Asignatura** (código único) para que las evaluaciones de HU-03 y la malla/avance de HU-04/HU-05 sigan asociados. HU-01 no modifica `carrera_asignatura`, `asignatura_prerrequisito` ni `asignatura_cursada`.
- Las clases Oferta Académica y Recomendación de Asignatura tienen diagrama de estado (EST-01 y EST-02, respectivamente).
- RP-09 a RP-12 son requisitos no funcionales (tiempos de respuesta, anonimato y moderación). Se asocian a la historia donde aplican, pero no tienen diagrama propio, porque son restricciones de calidad y no flujos de interacción.
