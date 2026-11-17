# AI-001 · Análisis de impacto de comparar proveedores

## Solicitud que se analiza

[SC-001 · Comparar opciones de proveedores para un producto](sc-001-comparar-proveedores.md), de Iker Gabriel Fonseca Fernández, dupla de revisión. El documento recibido carece de fecha. La solicitud es de mantenimiento **perfectivo** y el solicitante propone prioridad **media**.

**Responsable del análisis:** Gael Crespo Maceiras

**Fecha del análisis y de la decisión:** 17/11/2026

## Traducción a términos de OrderFlow

En el detalle de una recomendación de **un producto de una sucursal**, el usuario podría comparar dos condiciones de compra del mismo producto. El modelo ya permite que un producto tenga varias instancias de **CondicionProveedor**, aunque cada **RecomendacionCompra** referencia una sola condición aplicada. La comparación reutilizaría esos datos y mostraría proveedor, tamaño del paquete, plazo de entrega, cantidad de paquetes y cantidad total por opción.

Las dos opciones partirían de la **misma necesidad calculada antes de aplicar los ajustes comerciales** de la condición vigente. Así, los 45 kg del ejemplo se conservan como referencia común aunque el plazo de cada proveedor sea distinto. OrderFlow muestra esos plazos para apoyar la elección. Evaluar si la mercancía llegaría a tiempo requeriría criterios adicionales para cada opción.

### Comprobación del criterio de aceptación

| Proveedor | Necesidad | Tamaño del paquete | Plazo | Paquetes requeridos | Cantidad total |
|---|---:|---:|---:|---:|---:|
| A | 45 kg | 20 kg | 2 días | 3 | 60 kg |
| B | 45 kg | 15 kg | 4 días | 3 | 45 kg |

En esos datos de prueba, los paquetes son enteros y se redondean hacia arriba para cubrir la necesidad. Al generalizar el cálculo se aplican las unidades compatibles, los mínimos y los múltiplos que correspondan a cada condición. La persona toma la decisión de compra.

## Impacto directo

| Artefacto | Unidad | Qué habría que cambiar |
|---|---|---|
| [Requisitos funcionales](../../docs/especificacion-requisitos.md#3-requisitos-funcionales) | 2 | Definir un requisito de comparación y su criterio de aceptación. Vincularlo con RF-004 para datos de proveedores, RF-008 para el plazo mostrado, RF-010 para el ajuste por condición y RF-011 para presentar los resultados en el detalle. |
| [Caso de uso y prototipo](../../docs/diagramas/caso-de-uso.md) | 2 | Extender CU-04 con la selección y comparación de dos proveedores del mismo producto. Añadir al diagrama de casos de uso la extensión de CU-04 y al prototipo la vista que presenta los resultados y avisa cuando faltan datos para comparar. |
| [Diagrama de clases](../arch/modelo-uml.md#clases) | 4 | Reutilizar Producto, Proveedor y CondicionProveedor. Modelar el resultado de cada opción y el acceso a la necesidad previa al ajuste. La cantidad final guardada por sí sola no permite recuperar esa necesidad si el redondeo o el mínimo la aumentaron. |
| [Diagramas de secuencia](../arch/modelo-uml.md#secuencia--cu-03-generar-recomendaciones) | 4 | Dibujar la secuencia de comparación de CU-04. La vista pide comparar, Persistencia recupera las dos condiciones y Recomendaciones calcula los ajustes por separado. Con la alternativa B también se actualiza la secuencia de CU-03. |

## Impacto indirecto

| Artefacto | Unidad | Por qué se vería afectado |
|---|---|---|
| [ADR-001](../arch/adr-001-monolito-modular.md) | 3 | Sin impacto sobre la decisión de usar una aplicación modular y PostgreSQL. Revisar el costo previsto de generar por sucursal si se precalculan las opciones. |
| [C4 de contexto y contenedores](../arch/diagramas.md#c4-nivel-2--contenedores) | 3 | Sin impacto sobre los actores, los sistemas externos y los contenedores. La comparación se resolvería dentro de la aplicación web. |
| [C4 de componentes y paquetes](../arch/diagramas.md#c4-nivel-3--componentes-de-la-aplicación-web) | 3 y 4 | Actualizar las responsabilidades de Presentación, Recomendaciones y Persistencia para documentar la consulta y el cálculo de las opciones. |
| RNF-REN-001 y actividad de CU-03 | 2 y 4 | Con A, sin impacto en la generación por sucursal. Con B, agregar la precalculación al flujo de actividad y volver a medir el tiempo de la generación completa. |
| RNF-SEG-002 | 2 | Mantener las consultas de ambas condiciones acotadas al catálogo y a la empresa de la sesión. Incluir una prueba con datos de dos empresas. |
| CU-05 y RF-013 | 2 | Sin impacto con una comparación informativa. Habría que revisarlos si una solicitud posterior permite trasladar el proveedor elegido a la lista de compra. |
| Máquina de estados | 4 | Sin impacto. El resultado de la comparación se presenta para consulta y carece de un ciclo de vida propio. |

## Alcance del diseño vigente

El pronóstico desde el historial de ventas, el cálculo individual, su reutilización para toda la sucursal y la agrupación de recomendaciones en listas por proveedor siguen siendo el alcance de la entrega. La empresa conserva la decisión y la ejecución de la compra.

## Riesgos

| Riesgo | Probabilidad estimada | Consecuencia y tratamiento |
|---|---|---|
| Confundir la cantidad final redondeada con la necesidad original | Alta si se reutiliza solo el valor guardado | Las opciones partirían de una base incorrecta. Guardar la necesidad antes del ajuste cuando se genera una recomendación nueva y mostrar una incidencia si una recomendación previa carece de ese dato. |
| Interpretar tres paquetes como opciones equivalentes | Media | Pueden representar cantidades totales y plazos distintos. Mostrar los kilos y los días junto a los paquetes y conservar la necesidad de referencia a la vista. |
| Condiciones de proveedor incompletas o en unidades incompatibles | Media | La comparación puede producir cantidades engañosas. Validar unidades, disponibilidad de la condición y tamaño de paquete antes de calcular. |
| Aumentar la latencia de generación por sucursal | Media con B | El precálculo se repite por producto. Medir con el volumen de RNF-REN-001. |
| Mantener la comparación manual al rechazar el cambio | Media | Cuando haya dos proveedores, la persona seguirá revisando sus condiciones y calculando por separado, con riesgo de error. Se conserva el apoyo de la recomendación básica. |

## Alternativas consideradas

1. **A · Comparación bajo demanda.** Desde CU-04, el usuario elige dos proveedores y compara sus condiciones en ese momento. Usa la necesidad registrada y aplica los ajustes por separado. Agrega una consulta y una vista, con esfuerzo concentrado en el detalle de la recomendación. Si se decidiera incorporar la función, esta sería la opción preferible porque limita el cálculo adicional a los productos que alguien compara.
2. **B · Precalcular y mostrar en el detalle.** Durante CU-03 se calculan y guardan las opciones para mostrarlas al abrir la recomendación. La consulta del detalle puede leer resultados preparados, pero la generación por sucursal hace más trabajo. Hay que definir cómo actualizar las opciones guardadas si cambian las condiciones del proveedor.

## Estimación de esfuerzo

| Concepto | A · Bajo demanda | B · Precalculada |
|---|---:|---:|
| Diseño y documentación | 3–4 h | 4–6 h |
| Implementación | 7–10 h | 11–15 h |
| Pruebas | 3–5 h | 5–7 h |
| **Total** | **13–19 h** | **20–28 h** |

La estimación desglosa el trabajo para una persona y parte de una implementación base de OrderFlow con catálogos y recomendaciones consultables. En A, las 7–10 h de implementación se reparten entre exponer la necesidad y consultar condiciones (2–3 h), reutilizar el ajuste de cantidades (2–3 h) y construir la interfaz (3–4 h). B suma 4–5 h de implementación para calcular y guardar las opciones durante la generación. También requiere más diseño y pruebas por la persistencia y por el rendimiento del flujo completo.

Las pruebas incluyen el ejemplo de 45 kg, condiciones incompletas, unidades incompatibles, mínimos y múltiplos, y aislamiento entre empresas. B añade pruebas de actualización y generación por sucursal. El cálculo cubre el incremento sobre la base y usa estimaciones de planificación. Deja fuera la construcción inicial del sistema, la recuperación retroactiva de necesidades que no se guardaron y cualquier compra automática.

## Recomendación

**Rechazar SC-001 para la entrega actual.** La comparación ayudaría cuando existen dos proveedores con datos completos. La opción más acotada todavía requiere **13–19 horas** y depende de conservar la necesidad previa a los ajustes de compra. El esfuerzo se concentra en una decisión posterior a la recomendación, mientras que el valor principal de esta entrega está en generar una cantidad explicable por producto y sucursal y preparar listas de compra revisables. La consecuencia aceptada es que la comparación entre proveedores sigue siendo manual.
