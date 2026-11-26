# Descripción del cambio y justificación del DSS-03

**Origen:** [EVALUACION2.md](./EVALUACION2.md) — incorporación de una asignatura en estado **tentativo**, visible en el planificador y confirmable desde ahí.
**Artefacto generado:** [DSS-03 — Registro y confirmación de asignatura en estado tentativo](./10-diagramas-secuencia.md#dss-03--registro-y-confirmación-de-asignatura-en-estado-tentativo-evaluación-2) · [fuente Mermaid](./diagramas/dss_03.md) · [imagen](./diagramas/dss_03.png)

## 1. Qué se pidió cambiar

El enunciado de Evaluación 2 solicita que el sistema permita sugerir una asignatura para agilizar el avance académico, dejarla registrada en estado `tentativo`, mostrar ese estado en el planificador y permitir confirmarla desde esa misma pantalla. El cambio alcanza a tres historias y a varios artefactos:

| Artefacto | Cambio |
|-----------|--------|
| HU-02 | CA4: inscribir una asignatura y dejarla en estado tentativo; verse en el planificador y confirmarse ahí. |
| HU-05 | CA5: mostrar qué asignaturas posibles se pueden tomar para agilizar el avance; nuevos escenarios de navegación. |
| HU-04 / CU-15 | Marcar la asignatura como tentativa desde la malla. |
| CU-06 | Confirmar la asignatura tentativa desde el planificador; extensión «Informar sobre asignaturas en estado tentativa» sobre CU-07. |
| Modelo de dominio | Nueva clase *Recomendación de Asignatura* con `estado` (`tentativo`/`confirmada`), `fechaPropuesta` y `fechaConfirmacion`, asociada a *Simulación de Horario* y a *Asignatura*. |
| Diagrama de estado | Nuevo [EST-02](./11-diagramas-estado.md#est-02--ciclo-de-vida-de-recomendación-de-asignatura-avance-académico) para el ciclo de vida de la recomendación. |

## 2. Por qué hace falta un diagrama de secuencia nuevo

1. **El flujo es nuevo y transversal, no una variante local.** DSS-01 modela la carga y validación de la oferta (HU-01) y DSS-02 la simulación de horario (HU-02). El cambio, en cambio, recorre tres puntos del sistema en orden: el cálculo de tiempo restante (HU-05) propone asignaturas, la malla (HU-04) registra la marca tentativa y el planificador (HU-02) la muestra y la confirma. Incluir esto dentro de DSS-02 mezclaría responsabilidades y ocultaría el orden de los mensajes entre subsistemas.

2. **La secuencia es justamente lo que los otros artefactos no muestran.** Los casos de uso describen *qué* hace el actor, el modelo de dominio describe *qué datos* se guardan y EST-02 describe *por qué estados* pasa la recomendación. Ninguno precisa *cuándo* y *en qué orden* se intercambian los mensajes, quién persiste la recomendación ni dónde se reutilizan las validaciones de carga. Un diagrama de secuencia es el artefacto que cubre esa brecha.

3. **Fija una decisión de modelado que estaba ambigua.** El Gherkin decía literalmente «agregar la asignatura a la malla», pero el modelo de dominio asocia la recomendación a la *Simulación de Horario*. El DSS-03 materializa el acuerdo: se crea una *Recomendación de Asignatura* en estado `tentativo` vinculada a la simulación activa; se visualiza en la malla y en el planificador, pero **no modifica la malla en forma permanente**. Sin el diagrama, esa diferencia no quedaría registrada en ningún artefacto de interacción.

4. **Hace explícita la reutilización de reglas existentes.** La confirmación no inventa validaciones nuevas: el DSS-03 referencia DSS-02 para el choque de horario y el tope de créditos (CU-10/CU-11), de modo que el cambio no duplica lógica ya modelada.

5. **Conecta la interacción con el estado y la persistencia.** Cada mensaje relevante deja rastro en los atributos nuevos: `fechaPropuesta` al marcar como tentativa y `fechaConfirmacion` al confirmar. Estos son exactamente los eventos que EST-02 representa, por lo que DSS-03 y EST-02 se validan entre sí.

## 3. Variantes representadas en el diagrama

- **alt — Confirmación del estudiante:** si confirma (CU-06), la recomendación pasa a `confirmada` y registra `fechaConfirmacion`; si no, permanece `tentativo` sin incorporarse a la simulación.
- **opt — Sección ofertada:** la incorporación a la grilla solo se intenta si la asignatura tiene sección en la oferta activa, porque una asignatura de la malla puede no tenerla.
- **alt — Validación de carga:** se aplican las reglas de DSS-02; ante un choque o exceso de créditos se informa el motivo y no se agrega la sección, aunque la recomendación quede confirmada.

## 4. Trazabilidad del cambio

| Cambio | Historia / criterio | Caso de uso | Secuencia | Estado | Clase del dominio |
|--------|---------------------|-------------|-----------|--------|-------------------|
| Asignatura en estado tentativo | HU-04 + HU-02 CA4 | CU-15, CU-06, extensión de CU-07 | DSS-03 | EST-02 | Recomendación de Asignatura, Simulación de Horario |
| Recomendación de avance | HU-05 CA5 | CU-17 | DSS-03 | EST-02 | Recomendación de Asignatura, Asignatura |

## 5. Conclusión

El DSS-03 no reemplaza a los diagramas existentes: los complementa. Se agregó porque el cambio introdujo una interacción que cruza HU-05, HU-04 y HU-02, resolvió una ambigüedad entre el Gherkin y el modelo de dominio, y necesitaba quedar conectada con el nuevo EST-02 y con la persistencia de `fechaPropuesta`/`fechaConfirmacion`. Es el artefacto que permite verificar, de un vistazo, que el flujo pedido en Evaluación 2 está completo y es coherente con el resto de la documentación.
