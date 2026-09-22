***Después de la entrevista: supuestos

Supuestos que resultaron falsos

Supuesto original	Qué dijo el cliente	Ajuste
RNF-ESC-001 asumía "al menos 50 sucursales"	Hoy maneja 6, planea abrir 1 o 2 más en los próximos años	Bajar la métrica a algo como "10-15 sucursales" y anotar que el número anterior no tenía base real

Supuestos que se confirmaron

La demanda varía por sucursal y sube en fines de quincena (respalda RF-006, estacionalidad/día).
Hay proveedores con pedido mínimo (respalda RF-010).
Los productos perecederos sí generan merma, aunque "de vez en cuando", no a diario (respalda RF-009 como prioridad Importante, no Imprescindible — el nivel actual parece correcto).
Le preocupan tanto el faltante como el exceso, dependiendo del producto y momento (respalda el conflicto de usuarios y RF-012).
Unos segundos de espera al consultar son aceptables (respalda el orden de magnitud de RNF-REN-001/002, aunque los valores exactos de 10s/3s siguen siendo supuesto propio).
Hoy no tiene visibilidad cruzada entre sucursales hasta que el encargado avisa (confirma que RF-011/RF-012 responden a una necesidad real, no inventada).

Lo que apareció sin que lo esperaras

La fuente de datos hoy es manual (WhatsApp de cada encargado), no un POS. No cambia el alcance, pero sí importa para cuando definas la integración de datos: probablemente el prototipo necesite aceptar carga manual, no solo automática.
Cuando un proveedor entrega menos de lo pedido, el dueño decide a qué sucursal mandarlo. Es una decisión de reparto que hoy es manual y que sigue fuera de alcance (OrderFlow no gestiona logística de distribución), pero confirma que mostrar el riesgo por sucursal (RF-012) le serviría como apoyo para tomar esa decisión, aunque el sistema no la ejecute.
Los encargados de sucursal también quieren ver recomendaciones — y no solo las de su propia tienda, sino "qué está pasando en las otras". Esto no estaba en la Visión original como necesidad explícita y abre una pregunta: ¿un encargado debería ver información de otras sucursales de la misma cadena? Eso no lo resuelve RNF-SEG-002 (que solo separa información entre empresas distintas). Hay que validarlo con el cliente antes de asumir permisos.
A veces sobra producto en una sucursal que otra necesita. Sigue fuera de alcance (no gestiona transferencias), pero es un punto a futuro: mostrar esa información sin gestionar el movimiento físico podría tener valor.

Actualización del campo Origen (ejemplo de cómo se vería en tu tabla de requisitos):

Requisito	Origen actualizado
RF-006	Visión del producto, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026.
RF-009	Regla de negocio de la Visión, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026.
RF-010	Regla de negocio de la Visión, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026.
RF-011, RF-012	Visión del producto, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026.
RNF-REN-001, RNF-REN-002	Derivado del tipo de sistema. Orden de magnitud confirmado en entrevista, 22/09/2026; valores exactos siguen siendo supuesto propio.
RNF-ESC-001	Visión del producto, 18 de agosto de 2026. Corregido tras entrevista de elicitación, 22/09/2026: el número de sucursales real es 6, no 50; se ajusta la métrica.
2. Actores

Aplicando la prueba de "si dos personas hacen lo mismo, son un solo actor": el dueño de una tienda de abarrotes independiente y el encargado de una sucursal dentro de una cadena hacen lo mismo frente al sistema (consultar lo de su propia tienda), así que se fusionan en un solo rol.

Responsable de sucursal — dueño de una tienda independiente o encargado de una sucursal dentro de una cadena. Consulta ventas, inventario y recomendaciones de su propia sucursal.
Responsable de compras de la cadena — dueño o encargado que consolida varias sucursales. Consulta recomendaciones de múltiples sucursales y genera pedidos agrupados por proveedor.

(Dejo fuera un tercer actor tipo "Sistema POS" porque la integración de datos automática sigue sin definirse; por ahora la carga de ventas/inventario es una entrada que recibe el sistema, no una interacción de un actor identificado.)

3. Casos de uso (6, dentro del rango 5-8)
Consultar información de ventas e inventario
Consultar información de productos y proveedores
Generar recomendaciones de compra
Consultar y analizar una recomendación (justificación + riesgo)
Generar pedidos agrupados por proveedor
Consultar información consolidada de sucursales

Prueba aplicada: en los 6, al terminar la persona se puede ir satisfecha (obtuvo la información o el resultado que buscaba). Coinciden con los CU-01 a CU-06 que ya tenías en la tabla del punto 5 de tu especificación de requisitos, así que no aparecen huérfanos por ahora.
