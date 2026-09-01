# Visión del producto

> **Plantilla del curso · Ingeniería de Software I · SIS3407**
> Este documento es el primer entregable del semestre y la base de todo lo que viene después.
> Se entrega completo en la **semana 4** y se presenta ante el grupo.
>
> **Cómo usarla:** copia este archivo a tu repositorio como `docs/vision-del-producto.md`, borra las instrucciones en gris de cada apartado y escribe tu contenido en su lugar. Conserva los títulos.

---

**Autor:** Gael Crespo Maceiras  
**Fecha de la última versión:** 18/08/2026  
**Repositorio:** https://github.com/gaelCres/OrderFlow

---

## 1. Descripción del sistema

**Nombre del sistema:** OrderFlow

**Descripción:** OrderFlow es una herramienta digital que ayuda a las cadenas de tiendas de embutidos y abarrotes a decidir cuánto producto deben pedir a cada proveedor para mantener suficiente mercancía disponible sin comprar más de la necesaria. El sistema analiza las ventas, el inventario, el comportamiento de cada sucursal, la temporada y otros factores para generar recomendaciones de compra por producto, sucursal y proveedor. Su objetivo es reducir tanto las pérdidas ocasionadas por productos que no se venden antes de caducar como las ventas perdidas por falta de existencias.

---

## 2. Problema y usuarios

**El problema:** La falta de precisión en el inventario y en la planeación de compras puede provocar pérdidas económicas de dos formas principales. Por un lado, se compra más producto del que una sucursal puede vender, provocando acumulación de inventario y, en el caso de productos perecederos, merma por caducidad o deterioro. Por otro lado, se puede comprar menos producto del necesario y provocar faltantes, haciendo que la tienda pierda ventas cuando un cliente busca un producto que ya no está disponible. Este problema se vuelve más difícil de administrar cuando una empresa cuenta con varias sucursales, ya que cada una puede tener diferentes niveles y patrones de demanda.

**Cómo se resuelve hoy sin el sistema:** El encargado o dueño de cada sucursal realiza el pedido principalmente con base en su experiencia, memoria y observación del inventario disponible. Puede revisar las vitrinas, la bodega y las ventas recientes para estimar cuánto producto necesitará, y posteriormente comunicar la cantidad solicitada al responsable de compras o directamente al proveedor. En algunos casos, este proceso puede apoyarse en sistemas de punto de venta, hojas de cálculo o registros manuales, pero la decisión final sobre cuánto pedir sigue dependiendo en gran medida de la experiencia de la persona encargada.

**Usuarios del sistema:**

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
| **Encargado de sucursal** | Conocer cuánto producto debe solicitar para su sucursal y consultar las recomendaciones de compra. | Quedarse sin productos importantes o acumular mercancía que no se venda. |
| **Responsable de compras** | Obtener una lista consolidada de productos que deben solicitarse a cada proveedor considerando todas las sucursales. | Comprar cantidades incorrectas, generar exceso de inventario o no cubrir la demanda de las sucursales. |
| **Administrador o dueño** | Consultar el comportamiento de ventas, inventario, faltantes y merma de las sucursales para evaluar el desempeño de las compras. | Perder dinero por inventario excesivo, merma o ventas perdidas y no tener visibilidad de la operación. |

**Un conflicto entre usuarios:** El encargado de una sucursal puede preferir solicitar una cantidad mayor de producto para reducir el riesgo de quedarse sin mercancía durante el periodo de venta. En cambio, el responsable de compras puede buscar mantener los pedidos lo más ajustados posible para evitar exceso de inventario, costos innecesarios y, en productos perecederos, merma por caducidad. OrderFlow deberá buscar un equilibrio entre ambas necesidades mediante recomendaciones basadas en datos, en lugar de depender únicamente de la intuición de alguno de los usuarios.

---

## 3. Alcance

### Dentro del alcance

- **Consulta información de ventas e inventario**, incluyendo las cantidades vendidas y disponibles por producto y sucursal.

- **Generación de recomendaciones de compra**, calculando la cantidad sugerida que debe solicitarse para cada producto y sucursal con base en la demanda esperada, inventario disponible y otros factores relevantes. Ej. "Para la sucursal A, del producto X, al proveedor Y, recomiendo comprar 18 kg."

- **Generación de pedidos agrupados por proveedor**, permitiendo transformar las recomendaciones de las diferentes sucursales en una lista de compra para cada proveedor.

- **Consideración de patrones de demanda y características de los productos**, incluyendo factores como comportamiento histórico de ventas, día de la semana y estacionalidad, unidad de venta, presentación, tiempo de entrega del proveedor y, cuando aplique, vida útil o fecha de caducidad.

- **Visualización de información que justifique las recomendaciones de compra**, como demanda estimada, cantidad disponible utilizada para el cálculo, cantidad recomendada y nivel de riesgo de faltante o exceso.

- **Soporte para múltiples sucursales**, permitiendo que una misma empresa administre y compare la información de diferentes puntos de venta.

### Explícitamente fuera del alcance

- **Administrar el inventario operativo de la empresa**, incluyendo entradas, salidas, transferencias, ajustes, conteos físicos o movimientos de almacén. 

- **Sustituir el sistema de punto de venta (POS) de la empresa.** OrderFlow utilizará información de ventas e inventario proporcionada por la empresa o mediante una integración definida posteriormente.

- **Realizar automáticamente las compras o efectuar pagos a los proveedores.** El sistema generará recomendaciones y listas de compra, pero la decisión final y la realización del pedido serán responsabilidad de la empresa.

- **Gestionar la logística de distribución o transporte de mercancía entre proveedores y sucursales.**

- **Administrar contabilidad, facturación, nómina o finanzas generales de la empresa.**

- **Optimizar automáticamente los precios de venta o realizar promociones.**

*Instrucción: para al menos una de las exclusiones, explica la razón. Puede ser tiempo, complejidad, o que no aporta al problema central.*

---

## 4. Tipo de sistema y restricciones

**Tipo de sistema:** Web y SaaS, con características de sistema de información y de datos y análisis.

**Por qué es de ese tipo:** OrderFlow será una aplicación web utilizada por diferentes usuarios de una empresa para consultar información y generar recomendaciones de compra a partir de datos históricos y operativos. Se considera un sistema Web y SaaS porque estará disponible a través de un navegador y estará diseñado para que una misma plataforma pueda ser utilizada por diferentes sucursales de una empresa. También tiene características de sistema de información y de datos y análisis porque recibe, procesa y presenta información de ventas, existencias, productos, proveedores y comportamiento de la demanda para apoyar la toma de decisiones de compra. Al generar recomendaciones que pueden influir directamente en las cantidades que una empresa compra, el sistema debe proporcionar resultados consistentes, información comprensible y mecanismos que permitan al usuario revisar los datos utilizados para generar cada recomendación.

**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
| **Exactitud** | Las recomendaciones de compra deben calcularse correctamente a partir de los datos disponibles, ya que una cantidad incorrecta puede provocar exceso de producto o faltantes. | Se pueden generar pérdidas económicas por compras innecesarias, merma o ventas perdidas. |
| **Disponibilidad** | Los responsables de las sucursales y de compras necesitan consultar las recomendaciones cuando realizan sus pedidos. | Si el sistema no está disponible, los usuarios tendrán que volver a realizar las compras mediante métodos manuales. |
| **Rendimiento** | El sistema debe procesar la información de ventas e inventario y generar las recomendaciones en un tiempo razonable, incluso cuando existan varias sucursales y productos. | Los usuarios pueden tener que esperar demasiado para obtener sus recomendaciones, afectando el proceso de compra. |
| **Seguridad** | La información de ventas, inventario, proveedores y compras es información interna de la empresa y no debe estar disponible para usuarios no autorizados. | Personas no autorizadas podrían consultar o modificar información sensible de la empresa. |
| **Usabilidad** | Los usuarios principales pueden ser encargados de sucursal o responsables de compras que no necesariamente tienen conocimientos técnicos. Las recomendaciones deben ser fáciles de interpretar. | Los usuarios podrían interpretar incorrectamente las recomendaciones o preferir continuar utilizando métodos manuales. |

**Reglas de negocio que ya identifiqué:**

1. **Una recomendación de compra debe considerar la existencia disponible del producto en la sucursal**, ya que la cantidad que se debe solicitar no depende únicamente de la demanda esperada.

2. **La recomendación de compra debe considerar el tiempo de entrega del proveedor**, debido a que un proveedor que tarda varios días en entregar requiere una cantidad diferente a uno que puede surtir el producto rápidamente.

3. **Los productos perecederos deben considerar su vida útil o fecha de caducidad al calcular la cantidad recomendada**, para evitar que se solicite una cantidad que probablemente no pueda venderse antes de que el producto se deteriore o caduque.

4. **La recomendación debe calcularse de manera independiente para cada combinación de producto y sucursal**, ya que un mismo producto puede tener diferentes patrones de demanda dependiendo de la ubicación de la sucursal.

5. **La cantidad recomendada debe respetar las condiciones de compra establecidas por el proveedor**, como unidad de venta, presentación o cantidad mínima de pedido, cuando estas condiciones existan.
---

## 5. Ciclo de vida elegido

*Instrucción: este apartado se trabaja en la semana 3, después de ver los modelos de desarrollo. La justificación pesa más que la elección: no hay un modelo correcto, hay uno defendible para tu caso.*

**Modelo elegido:**

**Por qué le conviene a este proyecto:**

*Instrucción: argumenta con las características reales de tu caso. Estabilidad de los requisitos, disponibilidad del cliente, nivel de riesgo, tamaño del equipo, frecuencia de entregas esperada.*

### Alternativas descartadas

**Alternativa 1:**

*Por qué la descarté:*

**Alternativa 2:**

*Por qué la descarté:*

---

## Antes de entregar

Reviso que el documento cumpla lo siguiente:

- [ ] La descripción del apartado 1 se entiende sin ser del área
- [ ] Hay al menos dos tipos de usuario con necesidades distintas
- [ ] Identifiqué un conflicto real entre usuarios
- [ ] El alcance dice qué queda fuera, no solo qué queda dentro
- [ ] Las exclusiones son específicas, no genéricas
- [ ] Identifiqué el tipo de sistema y al menos dos atributos de calidad
- [ ] Anoté al menos tres reglas de negocio no obvias
- [ ] Justifiqué el ciclo de vida contra dos alternativas descartadas
- [ ] El documento está en mi repositorio y se puede leer desde el navegador
- [ ] Borré todas las instrucciones en cursiva de la plantilla
