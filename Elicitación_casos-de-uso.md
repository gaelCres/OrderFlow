# Notas post-entrevista y casos de uso · OrderFlow

## 1. Supuestos después de la entrevista

### Supuestos que resultaron falsos

| Supuesto original | Qué dijo el cliente | Ajuste |
|---|---|---|
| RNF-ESC-001 asumía "al menos 50 sucursales" | Hoy maneja 6, planea abrir 1 o 2 más en los próximos años | Bajar la métrica a algo como "10-15 sucursales" y anotar que el número anterior no tenía base real |

### Supuestos que se confirmaron

- La demanda varía por sucursal y sube en fines de quincena (respalda RF-006, estacionalidad/día).
- Hay proveedores con pedido mínimo (respalda RF-010).
- Los productos perecederos sí generan merma, aunque "de vez en cuando", no a diario (respalda RF-009 como prioridad Importante, no Imprescindible — el nivel actual parece correcto).
- Preocupan tanto el faltante como el exceso, dependiendo del producto y momento (respalda el conflicto de usuarios y RF-012).
- Unos segundos de espera al consultar son aceptables (respalda el orden de magnitud de RNF-REN-001/002, aunque los valores exactos de 10s/3s siguen siendo supuesto propio).
- Hoy no hay visibilidad cruzada entre sucursales hasta que el encargado avisa (confirma que RF-011/RF-012 responden a una necesidad real, no inventada).

### Lo que apareció sin que se esperara

- Resultó que la fuente de datos hoy es manual (WhatsApp de cada encargado), no un POS. No cambia el alcance, pero sí importa para cuando se defina la integración de datos: probablemente el prototipo necesite aceptar carga manual, no solo automática.
- Resultó que cuando un proveedor entrega menos de lo pedido, el dueño decide a qué sucursal mandarlo. Es una decisión de reparto que hoy es manual y que sigue fuera de alcance (OrderFlow no gestiona logística de distribución), pero confirma que mostrar el riesgo por sucursal (RF-012) sería un apoyo para tomar esa decisión, aunque el sistema no la ejecute.
- Resultó que los encargados de sucursal también quieren ver recomendaciones — y no solo las de su propia tienda, sino qué está pasando en las otras. Esto no estaba en la Visión original como necesidad explícita y abre una pregunta: ¿un encargado debería ver información de otras sucursales de la misma cadena? Eso no lo resuelve RNF-SEG-002 (que solo separa información entre empresas distintas). Queda pendiente de validar antes de asumir permisos.
- Resultó que a veces sobra producto en una sucursal que otra necesita. Sigue fuera de alcance (no gestiona transferencias), pero es un punto a futuro: mostrar esa información sin gestionar el movimiento físico podría tener valor.

### Actualización del campo Origen

| Requisito | Origen actualizado |
|---|---|
| RF-006 | Visión del producto, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026. |
| RF-009 | Regla de negocio de la Visión, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026. |
| RF-010 | Regla de negocio de la Visión, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026. |
| RF-011, RF-012 | Visión del producto, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026. |
| RNF-REN-001, RNF-REN-002 | Derivado del tipo de sistema. Orden de magnitud confirmado en entrevista, 22/09/2026; valores exactos siguen siendo supuesto propio. |
| RNF-ESC-001 | Visión del producto, 18 de agosto de 2026. Corregido tras entrevista de elicitación, 22/09/2026: el número de sucursales real es 6, no 50; se ajusta la métrica. |

---

## 2. Actores

Aplicando la prueba de "si dos personas hacen lo mismo, son un solo actor": el dueño de una tienda de abarrotes independiente y el encargado de una sucursal dentro de una cadena hacen lo mismo frente al sistema (consultar lo de su propia tienda), así que se fusionaron en un solo rol.

1. **Responsable de sucursal** — dueño de una tienda independiente o encargado de una sucursal dentro de una cadena. Consulta ventas, inventario y recomendaciones de su propia sucursal.
2. **Responsable de compras de la cadena** — dueño o encargado que consolida varias sucursales. Consulta recomendaciones de múltiples sucursales y genera pedidos agrupados por proveedor.

Se dejó fuera un tercer actor tipo "Sistema POS" porque la integración de datos automática sigue sin definirse; por ahora la carga de ventas/inventario es una entrada que recibe el sistema, no una interacción de un actor identificado.

---

## 3. Casos de uso

1. Consultar información de ventas e inventario
2. Consultar información de productos y proveedores
3. Generar recomendaciones de compra
4. Consultar y analizar una recomendación (justificación + riesgo)
5. Generar pedidos agrupados por proveedor
6. Consultar información consolidada de sucursales

Se aplicó la prueba: en los 6, al terminar la persona se puede ir satisfecha (obtuvo la información o el resultado que buscaba). Coinciden con los CU-01 a CU-06 que ya figuraban en la tabla del punto 5 de la especificación de requisitos, así que no aparecen huérfanos por ahora.

---

## 4. Caso de uso más importante, completo

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

Nota: se dejó fuera de este caso de uso a RF-011 (justificación) y RF-012 (riesgo mostrado al consultar) porque pertenecen a "Consultar y analizar una recomendación" (CU-04) — no sobra ni falta nada, solo pertenecen a otro caso de uso ya identificado.
