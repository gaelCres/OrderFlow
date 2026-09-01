# Visión del producto

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

**Tipo de sistema:** 
SaaS / de datos y análisis

OrderFlow será una aplicación que recibe y analiza información de ventas, existencias, productos y proveedores para generar recomendaciones de compra. El sistema deberá mostrar resultados claros y consistentes para apoyar la toma de decisiones.

**Atributos de calidad**

| Atributo | Descripción |
|---|---|
| **Exactitud** | Las recomendaciones deben basarse correctamente en los datos disponibles. |
| **Disponibilidad** | La información y las recomendaciones deben estar disponibles cuando se necesiten realizar pedidos. |
| **Rendimiento** | El sistema debe procesar los datos y generar recomendaciones en un tiempo razonable. |
| **Seguridad** | El acceso a la información debe estar protegido según el tipo de usuario. |
| **Usabilidad** | Las recomendaciones deben ser fáciles de entender y consultar. |

### Reglas de negocio iniciales

1. La recomendación debe considerar la cantidad disponible del producto.
2. Debe considerar el tiempo de entrega del proveedor.
3. Para productos perecederos, debe considerar su vida útil o caducidad.
4. La recomendación se calculará por producto y sucursal.
5. La cantidad recomendada debe respetar las condiciones de compra del proveedor, como unidad, presentación o pedido mínimo.

---

## 5. Ciclo de vida elegido

**Modelo elegido:** 
Prototipado rápido.

**Por qué le conviene a este proyecto:** OrderFlow todavía requiere validar algunos requisitos y reglas de negocio con los usuarios y el cliente. El prototipado rápido permite construir una versión inicial del sistema, mostrarla a los usuarios y utilizar su retroalimentación para ajustar las funciones antes de desarrollar la solución completa. Esto reduce el riesgo de construir una solución que no se adapte a las necesidades reales del proceso de compras.

**Alternativas descartadas**

**Alternativa 1: Modelo V**

**Por qué la descarté:** 
Aunque facilita la verificación y validación mediante pruebas asociadas a cada etapa, es más adecuado cuando los requisitos son estables. En OrderFlow todavía pueden surgir cambios a partir de la retroalimentación del cliente y los usuarios.

**Alternativa 2: Espiral**

**Por qué la descarté:** 
Permite gestionar riesgos mediante ciclos iterativos, pero requiere un análisis de riesgos más formal y una mayor complejidad de gestión. 

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
