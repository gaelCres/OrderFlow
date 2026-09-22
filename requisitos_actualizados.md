# Especificación de requisitos

**Sistema:** OrderFlow
**Autor:** Gael Crespo Maceiras
**Versión:** 1.1
**Fecha de la última actualización:** 22/09/2026

---

## 1. Propósito y alcance

**Propósito del documento:**

Este documento especifica los requisitos funcionales y no funcionales de OrderFlow. Está dirigido a los responsables del desarrollo, validación y revisión del sistema, así como a los usuarios involucrados en el proceso de planeación de compras de las tiendas.

**Alcance del sistema:**

OrderFlow es una aplicación SaaS de datos y análisis que consulta información de ventas, inventario, productos, sucursales y proveedores para generar recomendaciones de compra. El sistema calcula cantidades recomendadas por producto y sucursal considerando la demanda esperada, inventario disponible, comportamiento histórico, estacionalidad, día de la semana, unidad de venta, presentación, tiempo de entrega del proveedor y, cuando corresponda, vida útil o fecha de caducidad.

El sistema también genera listas de compra agrupadas por proveedor y muestra información que permite al usuario comprender los factores utilizados para generar cada recomendación.

**Fuera del alcance:**

* Administrar movimientos operativos de inventario, como entradas, salidas, transferencias, ajustes o conteos físicos.
* Sustituir el sistema de punto de venta de la empresa.
* Realizar automáticamente compras o pagos a proveedores.
* Gestionar la logística de distribución o transporte de mercancía.
* Administrar contabilidad, facturación, nómina o finanzas generales.
* Optimizar automáticamente precios de venta o promociones.

La administración operativa del inventario queda fuera del alcance porque OrderFlow utiliza las existencias disponibles como información de entrada para calcular recomendaciones, pero su objetivo principal es apoyar la planeación de compras y no sustituir el control operativo del inventario.

---

## 2. Usuarios y su contexto

| Usuario                                                   | Qué hace hoy sin el sistema                                                                                                 | Qué espera del sistema                                                                                |
| --------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Dueño de una tienda de abarrotes                          | Revisa el inventario y las ventas recientes y determina las cantidades de compra principalmente con base en su experiencia. | Consultar recomendaciones de compra para su sucursal y conocer los datos utilizados para calcularlas. |
| Dueño o responsable de una cadena de tiendas de embutidos | Revisa las necesidades de diferentes sucursales y consolida las compras que deben realizarse a los proveedores.             | Consultar recomendaciones de múltiples sucursales y obtener pedidos agrupados por proveedor.          |

**Conflictos identificados entre usuarios:**

El responsable de una sucursal puede priorizar mantener suficiente inventario para reducir el riesgo de faltantes, mientras que el responsable de una cadena puede priorizar evitar compras excesivas, especialmente en productos perecederos. OrderFlow debe mostrar información sobre el riesgo de faltante o exceso para que el usuario pueda evaluar la recomendación antes de realizar el pedido.

**Confirmación por entrevista de elicitación (22/09/2026):** el cliente confirmó que las dos preocupaciones (faltante y exceso) le importan por igual, dependiendo del producto y del momento, lo que respalda este conflicto tal como estaba descrito.

---

## 3. Requisitos funcionales

### 3.1 Resumen

| ID     | Nombre                                       | Prioridad      | Origen                                                                                |
| ------ | --------------------------------------------- | -------------- | -------------------------------------------------------------------------------------- |
| RF-001 | Consulta de ventas                           | Imprescindible | Visión del producto, 18 de agosto de 2026                                              |
| RF-002 | Consulta de inventario                       | Imprescindible | Visión del producto, 18 de agosto de 2026                                              |
| RF-003 | Consulta de información de productos         | Imprescindible | Visión del producto, 18 de agosto de 2026                                              |
| RF-004 | Consulta de información de proveedores       | Imprescindible | Visión del producto, 18 de agosto de 2026                                              |
| RF-005 | Consulta de información por sucursal         | Imprescindible | Visión del producto, 18 de agosto de 2026                                              |
| RF-006 | Cálculo de demanda esperada                  | Imprescindible | Visión del producto, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026. |
| RF-007 | Generación de recomendación de compra        | Imprescindible | Visión del producto, 18 de agosto de 2026                                              |
| RF-008 | Consideración del tiempo de entrega          | Imprescindible | Regla de negocio de la Visión, 18 de agosto de 2026                                    |
| RF-009 | Consideración de caducidad                   | Importante     | Regla de negocio de la Visión, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026. |
| RF-010 | Aplicación de condiciones de compra          | Imprescindible | Regla de negocio de la Visión, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026. |
| RF-011 | Visualización de justificación               | Imprescindible | Visión del producto, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026. |
| RF-012 | Visualización de riesgo                      | Importante     | Visión del producto, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026. |
| RF-013 | Generación de pedidos por proveedor          | Imprescindible | Visión del producto, 18 de agosto de 2026                                              |
| RF-014 | Consulta de recomendaciones por sucursal     | Imprescindible | Visión del producto, 18 de agosto de 2026                                              |
| RF-015 | Consulta consolidada de múltiples sucursales | Importante     | Visión del producto, 18 de agosto de 2026                                              |

### 3.2 Fichas

#### RF-001 · Consulta de ventas

| Campo                      | Contenido                                                                                                                   |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema muestra las ventas registradas de un producto para una sucursal durante un periodo seleccionado.                 |
| **Origen**                 | Visión del producto, 18 de agosto de 2026.                                                                                  |
| **Prioridad**              | Imprescindible                                                                                                              |
| **Criterio de aceptación** | Al seleccionar un producto, una sucursal y un periodo válido, el sistema muestra las ventas correspondientes a ese periodo. |
| **Relacionado con**        | RF-006, RF-007                                                                                                              |

#### RF-002 · Consulta de inventario

| Campo                      | Contenido                                                                                                                                                        |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema muestra la cantidad disponible de un producto para una sucursal.                                                                                      |
| **Origen**                 | Visión del producto, 18 de agosto de 2026.                                                                                                                       |
| **Prioridad**              | Imprescindible                                                                                                                                                   |
| **Criterio de aceptación** | Al seleccionar un producto y una sucursal con información disponible, el sistema muestra la cantidad de inventario utilizada como entrada para la recomendación. |
| **Relacionado con**        | RF-007, RF-011                                                                                                                                                   |

#### RF-003 · Consulta de información de productos

| Campo                      | Contenido                                                                                                                                                                                               |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema muestra las características de un producto utilizadas para determinar su compra.                                                                                                             |
| **Origen**                 | Visión del producto, 18 de agosto de 2026.                                                                                                                                                              |
| **Prioridad**              | Imprescindible                                                                                                                                                                                          |
| **Criterio de aceptación** | Al consultar un producto, el sistema muestra las características registradas que sean necesarias para calcular su recomendación, incluyendo su unidad de venta y presentación cuando estén disponibles. |
| **Relacionado con**        | RF-007, RF-010                                                                                                                                                                                          |

#### RF-004 · Consulta de información de proveedores

| Campo                      | Contenido                                                                                                                                            |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema muestra la información de los proveedores asociada a los productos.                                                                       |
| **Origen**                 | Visión del producto, 18 de agosto de 2026.                                                                                                           |
| **Prioridad**              | Imprescindible                                                                                                                                       |
| **Criterio de aceptación** | Al consultar un producto con proveedor registrado, el sistema muestra el proveedor asociado y las condiciones de compra disponibles para el cálculo. |
| **Relacionado con**        | RF-008, RF-010, RF-013                                                                                                                               |

#### RF-005 · Consulta de información por sucursal

| Campo                      | Contenido                                                                                                                                                    |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Descripción**            | El sistema muestra la información de ventas, inventario y recomendaciones correspondiente a una sucursal seleccionada.                                       |
| **Origen**                 | Visión del producto, 18 de agosto de 2026.                                                                                                                   |
| **Prioridad**              | Imprescindible                                                                                                                                               |
| **Criterio de aceptación** | Al seleccionar una sucursal, el sistema muestra únicamente la información correspondiente a dicha sucursal en las consultas que admitan filtro por sucursal. |
| **Relacionado con**        | RF-001, RF-002, RF-007, RF-014                                                                                                                               |

#### RF-006 · Cálculo de demanda esperada

| Campo                      | Contenido                                                                                                                                                             |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema calcula la demanda esperada de un producto para una sucursal utilizando información histórica de ventas disponible.                                        |
| **Origen**                 | Visión del producto, 18 de agosto de 2026; derivado de la descripción del cálculo de recomendaciones. Confirmado en entrevista de elicitación, 22/09/2026: el cliente reportó que la venta varía notablemente entre sucursales y sube casi al doble en fines de quincena. |
| **Prioridad**              | Imprescindible                                                                                                                                                        |
| **Criterio de aceptación** | Al existir información histórica suficiente para un producto y sucursal, el sistema calcula y muestra una demanda esperada para el periodo de compra correspondiente. |
| **Relacionado con**        | RF-001, RF-007, RF-011                                                                                                                                                |

#### RF-007 · Generación de recomendación de compra

| Campo                      | Contenido                                                                                                                                                         |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema genera una cantidad recomendada de compra para cada producto y sucursal a partir de la demanda esperada y el inventario disponible.                    |
| **Origen**                 | Visión del producto, 18 de agosto de 2026.                                                                                                                        |
| **Prioridad**              | Imprescindible                                                                                                                                                    |
| **Criterio de aceptación** | Al existir los datos requeridos para un producto y sucursal, el sistema genera una recomendación que contiene el producto, la sucursal y la cantidad recomendada. |
| **Relacionado con**        | RF-002, RF-006, RF-008, RF-009, RF-010, RF-011, RF-012                                                                                                            |

#### RF-008 · Consideración del tiempo de entrega

| Campo                      | Contenido                                                                                                                                       |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema considera el tiempo de entrega del proveedor al generar la recomendación de compra.                                                  |
| **Origen**                 | Regla de negocio de la Visión del producto, 18 de agosto de 2026.                                                                               |
| **Prioridad**              | Imprescindible                                                                                                                                  |
| **Criterio de aceptación** | Al generar una recomendación de un producto con tiempo de entrega registrado, el cálculo utiliza dicho tiempo como uno de sus datos de entrada. |
| **Relacionado con**        | RF-004, RF-007                                                                                                                                  |

#### RF-009 · Consideración de caducidad

| Campo                      | Contenido                                                                                                                                                                             |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema considera la vida útil o fecha de caducidad registrada al generar la recomendación de productos perecederos.                                                               |
| **Origen**                 | Regla de negocio de la Visión del producto, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026: el cliente indicó que a un producto con menos de 3 días de vida útil el proveedor no lo cambia ni lo regresa, lo que respalda tratar la caducidad como un factor determinante del cálculo. |
| **Prioridad**              | Importante                                                                                                                                                                            |
| **Criterio de aceptación** | Al generar una recomendación para un producto identificado como perecedero y con información de vida útil o caducidad disponible, el sistema utiliza dicha información en el cálculo. |
| **Relacionado con**        | RF-003, RF-007, RF-012                                                                                                                                                                |

#### RF-010 · Aplicación de condiciones de compra

| Campo                      | Contenido                                                                                                                                               |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema ajusta la cantidad recomendada a las condiciones de compra registradas del proveedor.                                                        |
| **Origen**                 | Regla de negocio de la Visión del producto, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026: el cliente indicó que hay proveedores con pedido mínimo y con frecuencias de entrega distintas entre sí. |
| **Prioridad**              | Imprescindible                                                                                                                                          |
| **Criterio de aceptación** | Al existir una presentación, unidad de venta o pedido mínimo registrado para el producto, la cantidad recomendada respeta la condición correspondiente. |
| **Relacionado con**        | RF-003, RF-004, RF-007                                                                                                                                  |

#### RF-011 · Visualización de justificación

| Campo                      | Contenido                                                                                                                                                     |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema muestra los datos utilizados para justificar una recomendación de compra.                                                                          |
| **Origen**                 | Visión del producto, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026: el cliente reportó que hoy no tiene forma de saber, con lo que le reportan los encargados, si un producto está por caducar o si otra sucursal tiene de sobra, lo que respalda la necesidad de esta visualización. |
| **Prioridad**              | Imprescindible                                                                                                                                                |
| **Criterio de aceptación** | Al consultar una recomendación, el sistema muestra como mínimo la demanda estimada, la cantidad disponible utilizada en el cálculo y la cantidad recomendada. |
| **Relacionado con**        | RF-002, RF-006, RF-007                                                                                                                                        |

#### RF-012 · Visualización de riesgo

| Campo                      | Contenido                                                                                                                                                 |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema muestra el nivel de riesgo de faltante o exceso asociado a una recomendación de compra.                                                        |
| **Origen**                 | Visión del producto, 18 de agosto de 2026. Confirmado en entrevista de elicitación, 22/09/2026: el cliente indicó que le preocupan por igual el faltante y el exceso, dependiendo del producto y el momento, y que hoy solo se entera de un exceso cuando el encargado avisa o cuando el producto deja de moverse. |
| **Prioridad**              | Importante                                                                                                                                                |
| **Criterio de aceptación** | Al consultar una recomendación, el sistema muestra un nivel de riesgo de faltante o exceso cuando los datos necesarios para calcularlo están disponibles. |
| **Relacionado con**        | RF-007, RF-009, RF-011                                                                                                                                    |

#### RF-013 · Generación de pedidos por proveedor

| Campo                      | Contenido                                                                                                                                                                                             |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema genera una lista de compra agrupada por proveedor a partir de las recomendaciones seleccionadas.                                                                                           |
| **Origen**                 | Visión del producto, 18 de agosto de 2026.                                                                                                                                                            |
| **Prioridad**              | Imprescindible                                                                                                                                                                                        |
| **Criterio de aceptación** | Al seleccionar recomendaciones correspondientes a uno o más proveedores, el sistema genera una lista separada para cada proveedor e incluye los productos y cantidades recomendadas correspondientes. |
| **Relacionado con**        | RF-004, RF-007, RF-015                                                                                                                                                                                |

#### RF-014 · Consulta de recomendaciones por sucursal

| Campo                      | Contenido                                                                                                                                                                        |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema muestra las recomendaciones de compra correspondientes a una sucursal seleccionada.                                                                                   |
| **Origen**                 | Visión del producto, 18 de agosto de 2026.                                                                                                                                       |
| **Prioridad**              | Imprescindible                                                                                                                                                                   |
| **Criterio de aceptación** | Al seleccionar una sucursal, el sistema muestra las recomendaciones de compra de los productos correspondientes a esa sucursal y no incluye recomendaciones de otras sucursales. |
| **Relacionado con**        | RF-005, RF-007                                                                                                                                                                   |

#### RF-015 · Consulta consolidada de múltiples sucursales

| Campo                      | Contenido                                                                                                                                                       |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema muestra de forma consolidada las recomendaciones de compra correspondientes a múltiples sucursales de una misma empresa.                             |
| **Origen**                 | Visión del producto, 18 de agosto de 2026.                                                                                                                      |
| **Prioridad**              | Importante                                                                                                                                                      |
| **Criterio de aceptación** | Al seleccionar múltiples sucursales, el sistema muestra las recomendaciones correspondientes a cada una e identifica la sucursal asociada a cada recomendación. |
| **Relacionado con**        | RF-007, RF-013, RF-014                                                                                                                                          |

---

## 4. Requisitos no funcionales

### 4.1 Resumen

| ID          | Atributo       | Nombre                                   | Prioridad      | Origen                                                                                   |
| ----------- | -------------- | ----------------------------------------- | -------------- | ------------------------------------------------------------------------------------------ |
| RNF-REN-001 | Rendimiento    | Tiempo de generación de recomendaciones  | Imprescindible | Derivado del tipo de sistema. Orden de magnitud confirmado en entrevista, 22/09/2026.      |
| RNF-REN-002 | Rendimiento    | Tiempo de consulta de recomendaciones    | Importante     | Derivado del tipo de sistema. Orden de magnitud confirmado en entrevista, 22/09/2026.      |
| RNF-SEG-001 | Seguridad      | Protección del acceso                    | Imprescindible | Supuesto propio derivado del tipo de sistema                                              |
| RNF-SEG-002 | Seguridad      | Separación de información entre empresas | Imprescindible | Supuesto propio derivado del modelo SaaS                                                  |
| RNF-USA-001 | Usabilidad     | Identificación de recomendaciones        | Imprescindible | Derivado del objetivo del sistema                                                          |
| RNF-USA-002 | Usabilidad     | Comprensión de la justificación          | Imprescindible | Visión del producto, 18 de agosto de 2026                                                  |
| RNF-CON-001 | Confiabilidad  | Consistencia de las recomendaciones      | Imprescindible | Derivado del tipo de sistema                                                                |
| RNF-MAN-001 | Mantenibilidad | Modificación de reglas de cálculo        | Importante     | Supuesto propio derivado del tipo de sistema                                              |
| RNF-ESC-001 | Escalabilidad  | Soporte de múltiples sucursales          | Importante     | Visión del producto, 18 de agosto de 2026. Corregido tras entrevista de elicitación, 22/09/2026. |

### 4.2 Fichas

#### RNF-REN-001 · Tiempo de generación de recomendaciones

| Campo                   | Contenido                                                                                                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Atributo de calidad** | Rendimiento                                                                                                                                                                      |
| **Descripción**         | El sistema genera las recomendaciones de compra de una sucursal en menos de 10 segundos cuando procesa hasta 10,000 registros de ventas históricos.                              |
| **Métrica**             | Tiempo transcurrido entre la solicitud de generación y la disponibilidad de todas las recomendaciones, medido con hasta 10,000 registros de ventas históricos para una sucursal. |
| **Origen**              | Derivado del tipo de sistema: OrderFlow procesa datos para apoyar decisiones de compra durante la planeación de pedidos. En la entrevista de elicitación (22/09/2026), el cliente confirmó que esperar unos segundos por una recomendación es aceptable, lo que respalda el orden de magnitud del límite; el valor exacto de 10 segundos sigue siendo un supuesto propio sujeto a validación. |
| **Prioridad**           | Imprescindible                                                                                                                                                                   |
| **Por qué importa**     | Una generación demasiado lenta dificulta consultar las recomendaciones durante el proceso de planeación de compras.                                                              |
| **Afecta a**            | RF-006, RF-007, RF-012                                                                                                                                                           |

#### RNF-REN-002 · Tiempo de consulta de recomendaciones

| Campo                   | Contenido                                                                                                                             |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Atributo de calidad** | Rendimiento                                                                                                                           |
| **Descripción**         | El sistema muestra las recomendaciones de una sucursal en menos de 3 segundos después de solicitar la consulta.                       |
| **Métrica**             | Tiempo entre la solicitud de consulta y el despliegue de las recomendaciones, considerando una consulta de hasta 500 recomendaciones. |
| **Origen**              | Derivado del tipo de sistema. En la entrevista de elicitación (22/09/2026), el cliente confirmó que unos segundos de espera son aceptables, lo que respalda el orden de magnitud del límite; el valor exacto de 3 segundos sigue siendo un supuesto propio sujeto a validación. |
| **Prioridad**           | Importante                                                                                                                            |
| **Por qué importa**     | El usuario necesita consultar las recomendaciones sin esperar periodos prolongados durante la planeación de compras.                  |
| **Afecta a**            | RF-007, RF-011, RF-014                                                                                                                |

#### RNF-SEG-001 · Protección del acceso

| Campo                   | Contenido                                                                                                                                  |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Atributo de calidad** | Seguridad                                                                                                                                  |
| **Descripción**         | El sistema impide consultar información de la empresa sin una sesión de usuario autenticada.                                               |
| **Métrica**             | En el 100 % de las pruebas realizadas sin sesión autenticada, las consultas de ventas, inventario y recomendaciones deben ser rechazadas.  |
| **Origen**              | Supuesto propio derivado del tipo de sistema SaaS y de que maneja información comercial de empresas. Pendiente de validar con el cliente. |
| **Prioridad**           | Imprescindible                                                                                                                             |
| **Por qué importa**     | La información de ventas, inventario y compras puede ser información comercial que no debe quedar disponible para personas no autorizadas. |
| **Afecta a**            | RF-001, RF-002, RF-007, RF-011, RF-013, RF-015                                                                                             |

#### RNF-SEG-002 · Separación de información entre empresas

| Campo                   | Contenido                                                                                                                                                                              |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Atributo de calidad** | Seguridad                                                                                                                                                                              |
| **Descripción**         | El sistema impide que un usuario consulte información perteneciente a una empresa distinta de aquella a la que está asociado.                                                          |
| **Métrica**             | En el 100 % de las pruebas realizadas con dos empresas independientes, un usuario de una empresa no debe visualizar ventas, inventario, recomendaciones ni pedidos de la otra empresa. |
| **Origen**              | Supuesto propio derivado del modelo SaaS y del soporte para diferentes empresas. Su aplicación debe validarse con el cliente. La entrevista de elicitación (22/09/2026) planteó además una pregunta abierta: si un encargado de sucursal debería poder ver información de otras sucursales de la misma empresa; este supuesto no cubre ese punto y queda pendiente de definir. |
| **Prioridad**           | Imprescindible                                                                                                                                                                         |
| **Por qué importa**     | La exposición de información de una empresa a otra afectaría la confidencialidad de sus datos comerciales.                                                                             |
| **Afecta a**            | RF-001, RF-002, RF-005, RF-007, RF-013, RF-015                                                                                                                                         |

#### RNF-USA-001 · Identificación de recomendaciones

| Campo                   | Contenido                                                                                                               |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Atributo de calidad** | Usabilidad                                                                                                              |
| **Descripción**         | Cada recomendación de compra muestra de forma identificable el producto, la sucursal y la cantidad recomendada.         |
| **Métrica**             | En el 100 % de las recomendaciones mostradas, los tres datos deben estar visibles sin acceder a una pantalla adicional. |
| **Origen**              | Derivado del ejemplo de recomendación incluido en la Visión del producto.                                               |
| **Prioridad**           | Imprescindible                                                                                                          |
| **Por qué importa**     | El usuario necesita identificar rápidamente qué producto comprar, para qué sucursal y en qué cantidad.                  |
| **Afecta a**            | RF-007, RF-014, RF-015                                                                                                  |

#### RNF-USA-002 · Comprensión de la justificación

| Campo                   | Contenido                                                                                                                                                                              |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Atributo de calidad** | Usabilidad                                                                                                                                                                             |
| **Descripción**         | El sistema presenta la información utilizada para justificar una recomendación en una misma vista de consulta.                                                                         |
| **Métrica**             | En el 100 % de las recomendaciones consultadas, la demanda estimada, el inventario utilizado y la cantidad recomendada deben poder consultarse sin navegar a otra sección del sistema. |
| **Origen**              | Visión del producto, 18 de agosto de 2026.                                                                                                                                             |
| **Prioridad**           | Imprescindible                                                                                                                                                                         |
| **Por qué importa**     | El usuario debe poder comprender los factores principales de una recomendación antes de decidir si realiza el pedido.                                                                  |
| **Afecta a**            | RF-007, RF-011, RF-012                                                                                                                                                                 |

#### RNF-CON-001 · Consistencia de las recomendaciones

| Campo                   | Contenido                                                                                                                                                                     |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Atributo de calidad** | Confiabilidad                                                                                                                                                                 |
| **Descripción**         | El sistema genera la misma recomendación cuando se procesan los mismos datos de entrada sin modificaciones.                                                                   |
| **Métrica**             | En 100 ejecuciones consecutivas con el mismo conjunto de datos de ventas, inventario, producto y proveedor, la cantidad recomendada debe ser idéntica en las 100 ejecuciones. |
| **Origen**              | Derivado del objetivo de OrderFlow de generar recomendaciones basadas en datos disponibles. El número de ejecuciones es un supuesto propio para validación.                   |
| **Prioridad**           | Imprescindible                                                                                                                                                                |
| **Por qué importa**     | Resultados diferentes ante los mismos datos dificultarían confiar en las recomendaciones para planear las compras.                                                            |
| **Afecta a**            | RF-006, RF-007, RF-008, RF-009, RF-010                                                                                                                                        |

#### RNF-MAN-001 · Modificación de reglas de cálculo

| Campo                   | Contenido                                                                                                                                                                           |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Atributo de calidad** | Mantenibilidad                                                                                                                                                                      |
| **Descripción**         | El sistema permite modificar una regla de cálculo de recomendaciones sin modificar más de cinco componentes del software.                                                           |
| **Métrica**             | Durante una prueba de cambio, una modificación de una regla de cálculo debe requerir cambios en un máximo de cinco componentes del sistema.                                         |
| **Origen**              | Supuesto propio derivado de que las reglas de negocio de OrderFlow pueden cambiar después de la validación con los usuarios. Pendiente de validar con el cliente.                   |
| **Prioridad**           | Importante                                                                                                                                                                          |
| **Por qué importa**     | Las reglas de compra pueden cambiar cuando el cliente valide el funcionamiento del sistema. Una alta cantidad de componentes afectados aumentaría el costo y riesgo de los cambios. |
| **Afecta a**            | RF-006, RF-007, RF-008, RF-009, RF-010                                                                                                                                              |

#### RNF-ESC-001 · Soporte de múltiples sucursales

| Campo                   | Contenido                                                                                                                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Atributo de calidad** | Escalabilidad                                                                                                                                                                                    |
| **Descripción**         | El sistema procesa información de al menos 15 sucursales pertenecientes a una misma empresa sin impedir la generación de recomendaciones.                                                        |
| **Métrica**             | Una ejecución que incluya datos de 15 sucursales debe completar la generación de recomendaciones sin errores funcionales y producir resultados para todas las sucursales con información válida. |
| **Origen**              | Visión del producto, 18 de agosto de 2026. Corregido tras entrevista de elicitación, 22/09/2026: el cliente maneja hoy 6 sucursales y planea abrir 1 o 2 más en los próximos años; se ajusta la métrica de 50 a 15 sucursales para mantener margen de crecimiento sin partir de una cifra sin base real. La cifra sigue siendo un supuesto propio sujeto a validación adicional. |
| **Prioridad**           | Importante                                                                                                                                                                                       |
| **Por qué importa**     | OrderFlow debe ser útil para cadenas con múltiples puntos de venta y no limitarse a una sola sucursal.                                                                                           |
| **Afecta a**            | RF-005, RF-007, RF-013, RF-014, RF-015                                                                                                                                                           |

---

## 5. Casos de uso

### 5.1 Actores

Identificados a partir de la entrevista de elicitación del 22/09/2026, agrupando por rol y no por persona:

| Actor | Descripción |
|---|---|
| **Responsable de sucursal** | Dueño de una tienda de abarrotes independiente o encargado de una sucursal dentro de una cadena. Consulta ventas, inventario y recomendaciones de su propia sucursal. |
| **Responsable de compras de la cadena** | Dueño o encargado que consolida varias sucursales. Consulta recomendaciones de múltiples sucursales y genera pedidos agrupados por proveedor. |

### 5.2 Lista de casos de uso

Validados con el cliente después de la entrevista de elicitación del 22/09/2026:

| ID    | Caso de uso                                      | Requisitos relacionados                |
| ----- | ------------------------------------------------ | -------------------------------------- |
| CU-01 | Consultar información de ventas e inventario     | RF-001, RF-002, RF-005                 |
| CU-02 | Consultar información de productos y proveedores | RF-003, RF-004                         |
| CU-03 | Generar recomendaciones de compra                | RF-006, RF-007, RF-008, RF-009, RF-010 |
| CU-04 | Consultar y analizar una recomendación           | RF-011, RF-012, RF-014                 |
| CU-05 | Generar pedidos agrupados por proveedor          | RF-013, RF-015                         |
| CU-06 | Consultar información consolidada de sucursales  | RF-005, RF-014, RF-015                 |

No se identificaron requisitos sin caso de uso asociado ni casos de uso sin requisitos detrás.

### 5.3 Caso de uso detallado: CU-03 · Generar recomendación de compra

| Campo | Contenido |
|---|---|
| **Actor principal** | Responsable de sucursal |
| **Objetivo** | Obtener la cantidad recomendada a pedir de un producto para su sucursal, lista para incluirse en el pedido al proveedor. |
| **Precondición** | El producto y la sucursal tienen información de ventas, inventario y proveedor registrada. |
| **Escenario principal** | 1. El responsable de sucursal selecciona la sucursal y el producto para el que quiere una recomendación. 2. El sistema recupera el historial de ventas del producto en esa sucursal y calcula la demanda esperada, considerando patrones históricos, día de la semana y estacionalidad. 3. El sistema recupera la cantidad de inventario disponible del producto en la sucursal. 4. El sistema recupera las características del producto (unidad de venta, presentación, vida útil si aplica) y las condiciones del proveedor asociado (tiempo de entrega, pedido mínimo). 5. El sistema calcula la cantidad recomendada a partir de la demanda esperada y el inventario disponible, considerando el tiempo de entrega del proveedor y, si el producto es perecedero, su vida útil o caducidad. 6. El sistema ajusta la cantidad para respetar las condiciones de compra del proveedor. 7. El sistema muestra la recomendación con el producto, la sucursal y la cantidad recomendada. |
| **Flujos alternos** | 3a. No hay dato de inventario disponible para el producto en esa sucursal: el sistema no genera la recomendación y notifica al responsable de sucursal que falta ese dato de entrada.<br>5a. No hay suficiente historial de ventas para calcular una demanda confiable: el sistema genera la recomendación con la información disponible, marcándola como de baja confianza.<br>6a. La cantidad ajustada a las condiciones del proveedor (por ejemplo, redondeada al pedido mínimo) supera de forma notable la demanda esperada: el sistema marca esta diferencia como riesgo de exceso. |
| **Postcondición** | La recomendación queda generada y disponible para su consulta posterior. |
| **Requisitos que realiza** | RF-001, RF-002, RF-003, RF-004, RF-006, RF-007, RF-008, RF-009, RF-010. También activa RNF-CON-001 (consistencia) y RNF-REN-001 (tiempo de generación). |

---

## 6. Trazabilidad

| Requisito   | Origen                            | Caso de uso                                            | Elemento del prototipo              |
| ----------- | --------------------------------- | -------------------------------------------------------- | ------------------------------------ |
| RF-001      | Visión del producto               | CU-01 Consultar información de ventas e inventario     | Vista de ventas                     |
| RF-002      | Visión del producto               | CU-01 Consultar información de ventas e inventario     | Vista de inventario                 |
| RF-003      | Visión del producto               | CU-02 Consultar información de productos y proveedores | Vista de producto                   |
| RF-004      | Visión del producto               | CU-02 Consultar información de productos y proveedores | Vista de proveedor                  |
| RF-005      | Visión del producto               | CU-01 / CU-06                                          | Selector de sucursal                |
| RF-006      | Visión del producto; confirmado en entrevista | CU-03 Generar recomendaciones de compra                | Motor de cálculo                    |
| RF-007      | Visión del producto               | CU-03 Generar recomendaciones de compra                | Vista de recomendaciones            |
| RF-008      | Regla de negocio                  | CU-03 Generar recomendaciones de compra                | Parámetros del cálculo              |
| RF-009      | Regla de negocio; confirmado en entrevista | CU-03 Generar recomendaciones de compra                | Parámetros de productos perecederos |
| RF-010      | Regla de negocio; confirmado en entrevista | CU-03 Generar recomendaciones de compra                | Condiciones de compra               |
| RF-011      | Visión del producto; confirmado en entrevista | CU-04 Consultar y analizar una recomendación           | Detalle de recomendación            |
| RF-012      | Visión del producto; confirmado en entrevista | CU-04 Consultar y analizar una recomendación           | Indicador de riesgo                 |
| RF-013      | Visión del producto               | CU-05 Generar pedidos agrupados por proveedor          | Lista de pedidos                    |
| RF-014      | Visión del producto               | CU-04 Consultar y analizar una recomendación           | Filtro por sucursal                 |
| RF-015      | Visión del producto               | CU-05 / CU-06                                          | Vista consolidada                   |
| RNF-REN-001 | Derivado del tipo de sistema; orden de magnitud confirmado en entrevista | CU-03                                                  | Generación de recomendaciones       |
| RNF-REN-002 | Derivado del tipo de sistema; orden de magnitud confirmado en entrevista | CU-04                                                  | Consulta de recomendaciones         |
| RNF-SEG-001 | Supuesto propio                   | Todos                                                  | Autenticación                       |
| RNF-SEG-002 | Supuesto propio                   | CU-01 a CU-06                                          | Separación de información           |
| RNF-USA-001 | Derivado del objetivo del sistema | CU-04                                                  | Vista de recomendación              |
| RNF-USA-002 | Visión del producto               | CU-04                                                  | Detalle de recomendación            |
| RNF-CON-001 | Derivado del tipo de sistema      | CU-03                                                  | Motor de cálculo                    |
| RNF-MAN-001 | Supuesto propio                   | CU-03                                                  | Motor de cálculo                    |
| RNF-ESC-001 | Visión del producto; corregido en entrevista | CU-05 / CU-06                                          | Vista consolidada                   |

---

## 7. Registro de cambios

| Fecha      | Requisito | Qué cambió                                                        | Por qué                                                                  |
| ---------- | --------- | ------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| 17/09/2026 | Todos     | Creación de la primera versión de la especificación de requisitos | Elaboración inicial de los requisitos a partir de la Visión del producto |
| 22/09/2026 | RF-006, RF-009, RF-010, RF-011, RF-012 | Se actualizó el campo Origen para reflejar confirmación en la entrevista de elicitación | Resultado de la entrevista de elicitación con el cliente, 22/09/2026 |
| 22/09/2026 | RNF-REN-001, RNF-REN-002 | Se actualizó el campo Origen: el orden de magnitud de los tiempos de espera fue confirmado; los valores exactos siguen siendo supuesto propio | Resultado de la entrevista de elicitación con el cliente, 22/09/2026 |
| 22/09/2026 | RNF-ESC-001 | Se corrigió la descripción y la métrica de 50 a 15 sucursales | El cliente reportó que maneja 6 sucursales y planea crecer a 7 u 8; la cifra de 50 no tenía base real |
| 22/09/2026 | RNF-SEG-002 | Se agregó una nota sobre una pregunta abierta: si un encargado de sucursal debería ver información de otras sucursales de la misma empresa | Surgió en la entrevista de elicitación, no estaba contemplado en el supuesto original |
| 22/09/2026 | Sección 5 | Se agregaron actores, se confirmaron los seis casos de uso y se redactó completo CU-03 con escenario principal y flujos alternos | Resultado del ejercicio de casos de uso posterior a la entrevista de elicitación |

---

## Antes de entregar

* [x] Todos los requisitos tienen identificador único y ninguno está repetido.
* [x] Cada requisito expresa una sola idea.
* [x] Cada requisito funcional tiene criterio de aceptación comprobable.
* [x] Cada requisito no funcional tiene una métrica.
* [x] El campo Origen distingue lo confirmado de lo supuesto.
* [x] Se incluyen requisitos de rendimiento, seguridad, usabilidad, confiabilidad, mantenibilidad y escalabilidad.
* [x] Ningún requisito impone una solución técnica específica.
* [x] Todos los requisitos se encuentran dentro del alcance declarado.
* [x] No se identifican contradicciones entre los requisitos.
* [x] Los requisitos están redactados para reducir interpretaciones ambiguas.
* [ ] Los requisitos de origen "supuesto propio" fueron validados con el cliente. *(parcial: RNF-ESC-001 y el orden de magnitud de RNF-REN-001/002 ya se validaron; RNF-SEG-001, RNF-SEG-002 y RNF-MAN-001 siguen pendientes.)*
* [ ] La tabla de trazabilidad fue actualizada con los elementos definitivos del prototipo.
* [ ] La dupla revisó el documento y registró su revisión.
* [x] Los casos de uso fueron validados después de la entrevista de elicitación.
