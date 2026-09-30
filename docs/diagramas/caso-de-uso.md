## Caso de uso más importante, completo

### CU-03 · Generar recomendación de compra

| Campo | Contenido |
|---|---|
| **Actor principal** | Responsable de sucursal |
| **Objetivo** | Obtener la cantidad recomendada a pedir de un producto para su sucursal, lista para incluirse en el pedido al proveedor. |
| **Precondición** | El producto y la sucursal tienen información de ventas, inventario y proveedor registrada. |
| **Escenario principal** | 1. El responsable de sucursal selecciona la sucursal y el producto para el que quiere una recomendación. 2. El sistema recupera el historial de ventas del producto en esa sucursal y calcula la demanda esperada, considerando patrones históricos, día de la semana y estacionalidad. 3. El sistema recupera la cantidad de inventario disponible del producto en la sucursal. 4. El sistema recupera las características del producto (unidad de venta, presentación, vida útil si aplica) y las condiciones del proveedor asociado (tiempo de entrega, pedido mínimo). 5. El sistema calcula la cantidad recomendada a partir de la demanda esperada y el inventario disponible, considerando el tiempo de entrega del proveedor y, si el producto es perecedero, su vida útil o caducidad. 6. El sistema ajusta la cantidad para respetar las condiciones de compra del proveedor. 7. El sistema muestra la recomendación con el producto, la sucursal y la cantidad recomendada. |
| **Flujos alternos** | 3a. No hay dato de inventario disponible para el producto en esa sucursal: el sistema no genera la recomendación y notifica al responsable de sucursal que falta ese dato de entrada.<br>5a. No hay suficiente historial de ventas para calcular una demanda confiable: el sistema genera la recomendación con la información disponible, marcándola como de baja confianza.<br>6a. La cantidad ajustada a las condiciones del proveedor (por ejemplo, redondeada al pedido mínimo) supera de forma notable la demanda esperada: el sistema marca esta diferencia como riesgo de exceso. |
| **Postcondición** | La recomendación queda generada y disponible para su consulta posterior. |
| **Requisitos que realiza** | RF-001, RF-002, RF-003, RF-004, RF-006, RF-007, RF-008, RF-009, RF-010. También activa RNF-CON-001 (consistencia) y RNF-REN-001 (tiempo de generación). |

