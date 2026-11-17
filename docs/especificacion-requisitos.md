# Especificación de requisitos

**Sistema:** OrderFlow
**Autor:** Gael Crespo Maceiras
**Versión:** 1.2
**Fecha de la última actualización:** 17/11/2026

---

## 1. Propósito y alcance

**Propósito del documento:**

Este documento especifica los requisitos funcionales y no funcionales de OrderFlow. Está dirigido a los responsables del desarrollo, validación y revisión del sistema, así como a los usuarios involucrados en el proceso de planeación de compras de las tiendas.

**Alcance del sistema:**

OrderFlow es una aplicación SaaS de datos y análisis que consulta información de ventas, inventario, productos, sucursales y proveedores para generar recomendaciones de compra. El sistema calcula cantidades recomendadas por producto y sucursal considerando la demanda esperada, inventario disponible, comportamiento histórico, estacionalidad, día de la semana, unidad de venta, presentación, tiempo de entrega del proveedor y, cuando corresponda, vida útil o fecha de caducidad.

El usuario puede pedir el cálculo para un producto o para todos los productos de una sucursal. En ambos casos se utiliza el mismo cálculo por producto y sucursal.

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

Este conflicto se exploró en un ejercicio de elicitación (22/09/2026), donde surgió como una preocupación plausible que depende del producto y del momento.

---

## 3. Requisitos funcionales

### 3.1 Resumen

| ID     | Nombre                                       | Prioridad      | Origen                                                                     |
| ------ | --------------------------------------------- | -------------- | ---------------------------------------------------------------------------- |
| RF-001 | Consulta de ventas                           | Imprescindible | Visión del producto, 18 de agosto de 2026                                   |
| RF-002 | Consulta de inventario                       | Imprescindible | Visión del producto, 18 de agosto de 2026                                   |
| RF-003 | Consulta de información de productos         | Imprescindible | Visión del producto, 18 de agosto de 2026                                   |
| RF-004 | Consulta de información de proveedores       | Imprescindible | Visión del producto, 18 de agosto de 2026                                   |
| RF-005 | Consulta de información por sucursal         | Imprescindible | Visión del producto, 18 de agosto de 2026                                   |
| RF-006 | Cálculo de demanda esperada                  | Imprescindible | Visión del producto, 18 de agosto de 2026; explorado en elicitación, 22/09/2026 |
| RF-007 | Generación de recomendación de compra        | Imprescindible | Visión del producto, 18 de agosto de 2026                                   |
| RF-008 | Consideración del tiempo de entrega          | Imprescindible | Regla de negocio de la Visión, 18 de agosto de 2026                         |
| RF-009 | Consideración de caducidad                   | Importante     | Regla de negocio de la Visión, 18 de agosto de 2026; explorado en elicitación, 22/09/2026 |
| RF-010 | Aplicación de condiciones de compra          | Imprescindible | Regla de negocio de la Visión, 18 de agosto de 2026; explorado en elicitación, 22/09/2026 |
| RF-011 | Visualización de justificación               | Imprescindible | Visión del producto, 18 de agosto de 2026; explorado en elicitación, 22/09/2026 |
| RF-012 | Visualización de riesgo                      | Importante     | Visión del producto, 18 de agosto de 2026; explorado en elicitación, 22/09/2026 |
| RF-013 | Generación de listas de compra por proveedor          | Imprescindible | Visión del producto, 18 de agosto de 2026                                   |
| RF-014 | Consulta de recomendaciones por sucursal     | Imprescindible | Visión del producto, 18 de agosto de 2026                                   |
| RF-015 | Consulta consolidada de múltiples sucursales | Importante     | Visión del producto, 18 de agosto de 2026                                   |

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
| **Descripción** | El sistema calcula la demanda esperada por producto y sucursal con el historial de ventas disponible. |
| **Origen**                 | Visión del producto, 18 de agosto de 2026; derivado de la descripción del cálculo de recomendaciones. Explorado en ejercicio de elicitación, 22/09/2026, como posible necesidad real. |
| **Prioridad**              | Imprescindible                                                                                                                                                        |
| **Criterio de aceptación** | Con historial utilizable, el sistema calcula la demanda esperada para el periodo de compra. Si el historial es insuficiente para una estimación confiable pero permite estimar, identifica el resultado como de baja confianza. Si los datos no permiten estimar, informa la incidencia del producto. |
| **Relacionado con**        | RF-001, RF-007, RF-011                                                                                                                                                |

#### RF-007 · Generación de recomendación de compra

| Campo                      | Contenido                                                                                                                                                         |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción** | El sistema genera una cantidad recomendada por producto y sucursal a partir de la demanda esperada y el inventario disponible. Acepta una solicitud para un producto o para todos los productos de una sucursal. |
| **Origen**                 | Visión del producto, 18 de agosto de 2026.                                                                                                                        |
| **Prioridad**              | Imprescindible                                                                                                                                                    |
| **Criterio de aceptación** | Con entradas válidas, el sistema devuelve el producto, la sucursal y la cantidad recomendada. Con los mismos datos y configuración, el cálculo individual y el resultado de ese producto dentro de la generación por sucursal coinciden. La generación por sucursal devuelve las recomendaciones obtenidas y las incidencias por producto. La falta de datos de un producto permite continuar el cálculo de los demás. |
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
| **Origen**                 | Regla de negocio de la Visión del producto, 18 de agosto de 2026. Explorado en ejercicio de elicitación, 22/09/2026, como posible necesidad real ligada a productos perecederos.       |
| **Prioridad**              | Importante                                                                                                                                                                            |
| **Criterio de aceptación** | Al generar una recomendación para un producto identificado como perecedero y con información de vida útil o caducidad disponible, el sistema utiliza dicha información en el cálculo. |
| **Relacionado con**        | RF-003, RF-007, RF-012                                                                                                                                                                |

#### RF-010 · Aplicación de condiciones de compra

| Campo                      | Contenido                                                                                                                                               |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción** | El sistema ajusta la cantidad recomendada a las condiciones de compra configuradas para la relación entre producto y proveedor, como unidad, presentación, múltiplo y pedido mínimo. |
| **Origen**                 | Regla de negocio de la Visión del producto, 18 de agosto de 2026. Explorado en ejercicio de elicitación, 22/09/2026, como posible necesidad real ligada a condiciones de proveedores. |
| **Prioridad**              | Imprescindible                                                                                                                                          |
| **Criterio de aceptación** | Si la necesidad es positiva, el sistema ajusta la cantidad a las condiciones registradas en unidades compatibles. Si el ajuste supera la necesidad calculada, identifica el posible exceso. Si la necesidad es cero, la cantidad recomendada es cero. |
| **Relacionado con**        | RF-003, RF-004, RF-007                                                                                                                                  |

#### RF-011 · Visualización de justificación

| Campo                      | Contenido                                                                                                                                                     |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema muestra los datos utilizados para justificar una recomendación de compra.                                                                          |
| **Origen**                 | Visión del producto, 18 de agosto de 2026. Explorado en ejercicio de elicitación, 22/09/2026, como posible necesidad real.                                    |
| **Prioridad**              | Imprescindible                                                                                                                                                |
| **Criterio de aceptación** | Al consultar una recomendación, el sistema muestra como mínimo la demanda estimada, la cantidad disponible utilizada en el cálculo y la cantidad recomendada. |
| **Relacionado con**        | RF-002, RF-006, RF-007                                                                                                                                        |

#### RF-012 · Visualización de riesgo

| Campo                      | Contenido                                                                                                                                                 |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema muestra el nivel de riesgo de faltante o exceso asociado a una recomendación de compra.                                                        |
| **Origen**                 | Visión del producto, 18 de agosto de 2026. Explorado en ejercicio de elicitación, 22/09/2026, como posible necesidad real.                                |
| **Prioridad**              | Importante                                                                                                                                                |
| **Criterio de aceptación** | Al consultar una recomendación, el sistema muestra un nivel de riesgo de faltante o exceso cuando los datos necesarios para calcularlo están disponibles. |
| **Relacionado con**        | RF-007, RF-009, RF-011                                                                                                                                    |

#### RF-013 · Generación de listas de compra por proveedor

| Campo                      | Contenido                                                                                                                                                                                             |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Descripción**            | El sistema genera una lista de compra agrupada por proveedor a partir de las recomendaciones seleccionadas.                                                                                           |
| **Origen**                 | Visión del producto, 18 de agosto de 2026.                                                                                                                                                            |
| **Prioridad**              | Imprescindible                                                                                                                                                                                        |
| **Criterio de aceptación** | Al seleccionar recomendaciones de uno o más proveedores, el sistema genera una lista por proveedor que identifica los productos, las cantidades recomendadas y sus sucursales de origen. Se entrega al usuario para su revisión. |
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

| ID          | Atributo       | Nombre                                   | Prioridad      | Origen                                       |
| ----------- | -------------- | ----------------------------------------- | -------------- | ---------------------------------------------- |
| RNF-REN-001 | Rendimiento    | Tiempo de generación de recomendaciones  | Imprescindible | Derivado del tipo de sistema                   |
| RNF-REN-002 | Rendimiento    | Tiempo de consulta de recomendaciones    | Importante     | Derivado del tipo de sistema                   |
| RNF-SEG-001 | Seguridad      | Protección del acceso                    | Imprescindible | Supuesto propio derivado del tipo de sistema   |
| RNF-SEG-002 | Seguridad      | Separación de información entre empresas | Imprescindible | Supuesto propio derivado del modelo SaaS       |
| RNF-USA-001 | Usabilidad     | Identificación de recomendaciones        | Imprescindible | Derivado del objetivo del sistema              |
| RNF-USA-002 | Usabilidad     | Comprensión de la justificación          | Imprescindible | Visión del producto, 18 de agosto de 2026      |
| RNF-CON-001 | Confiabilidad  | Consistencia de las recomendaciones      | Imprescindible | Derivado del tipo de sistema                   |
| RNF-MAN-001 | Mantenibilidad | Modificación de reglas de cálculo        | Importante     | Supuesto propio derivado del tipo de sistema   |
| RNF-ESC-001 | Escalabilidad  | Soporte de múltiples sucursales          | Importante     | Visión del producto, 18 de agosto de 2026      |

### 4.2 Fichas

#### RNF-REN-001 · Tiempo de generación de recomendaciones

| Campo                   | Contenido                                                                                                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Atributo de calidad** | Rendimiento                                                                                                                                                                      |
| **Descripción**         | El sistema genera las recomendaciones de compra de una sucursal en menos de 10 segundos cuando procesa hasta 10 000 registros de ventas históricos.                              |
| **Métrica** | Tiempo entre la solicitud de generación para toda la sucursal y la disponibilidad del conjunto de resultados e incidencias, con hasta 10 000 registros históricos de ventas para dicha sucursal. |
| **Origen**              | Derivado del tipo de sistema. El límite es un supuesto propio sujeto a validación.                                                                                              |
| **Prioridad**           | Imprescindible                                                                                                                                                                   |
| **Por qué importa**     | Una generación demasiado lenta dificulta consultar las recomendaciones durante el proceso de planeación de compras.                                                              |
| **Afecta a**            | RF-006, RF-007, RF-012                                                                                                                                                           |

#### RNF-REN-002 · Tiempo de consulta de recomendaciones

| Campo                   | Contenido                                                                                                                             |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Atributo de calidad** | Rendimiento                                                                                                                           |
| **Descripción**         | El sistema muestra las recomendaciones de una sucursal en menos de 3 segundos después de solicitar la consulta.                       |
| **Métrica**             | Tiempo entre la solicitud de consulta y el despliegue de las recomendaciones, considerando una consulta de hasta 500 recomendaciones. |
| **Origen**              | Derivado del tipo de sistema. El límite de 3 segundos es un supuesto propio sujeto a validación.                                      |
| **Prioridad**           | Importante                                                                                                                            |
| **Por qué importa**     | El usuario necesita consultar las recomendaciones sin esperar periodos prolongados durante la planeación de compras.                  |
| **Afecta a**            | RF-007, RF-011, RF-014                                                                                                                |

#### RNF-SEG-001 · Protección del acceso

| Campo                   | Contenido                                                                                                                                  |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Atributo de calidad** | Seguridad                                                                                                                                  |
| **Descripción**         | El sistema impide consultar información de la empresa sin una sesión de usuario autenticada.                                               |
| **Métrica**             | En el 100 % de las pruebas realizadas sin sesión autenticada, las consultas de ventas, inventario y recomendaciones deben ser rechazadas.  |
| **Origen**              | Supuesto propio derivado del tipo de sistema SaaS y de que maneja información comercial de empresas.                                       |
| **Prioridad**           | Imprescindible                                                                                                                             |
| **Por qué importa**     | La información de ventas, inventario y compras puede ser información comercial que no debe quedar disponible para personas no autorizadas. |
| **Afecta a**            | RF-001, RF-002, RF-007, RF-011, RF-013, RF-015                                                                                             |

#### RNF-SEG-002 · Separación de información entre empresas

| Campo                   | Contenido                                                                                                                                                                              |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Atributo de calidad** | Seguridad                                                                                                                                                                              |
| **Descripción**         | El sistema impide que un usuario consulte información perteneciente a una empresa distinta de aquella a la que está asociado.                                                          |
| **Métrica**             | En el 100 % de las pruebas realizadas con dos empresas independientes, un usuario de una empresa no debe visualizar ventas, inventario, recomendaciones ni listas de compra de la otra empresa. |
| **Origen** | Supuesto propio derivado del modelo SaaS y del soporte para diferentes empresas. |
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
| **Origen**              | Supuesto propio derivado de que las reglas de negocio de OrderFlow pueden cambiar después de la validación con los usuarios.                                                        |
| **Prioridad**           | Importante                                                                                                                                                                          |
| **Por qué importa**     | Las reglas de compra pueden cambiar cuando el cliente valide el funcionamiento del sistema. Una alta cantidad de componentes afectados aumentaría el costo y riesgo de los cambios. |
| **Afecta a**            | RF-006, RF-007, RF-008, RF-009, RF-010                                                                                                                                              |

#### RNF-ESC-001 · Soporte de múltiples sucursales

| Campo                   | Contenido                                                                                                                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Atributo de calidad** | Escalabilidad                                                                                                                                                                                    |
| **Descripción**         | El sistema procesa información de al menos 15 sucursales pertenecientes a una misma empresa sin impedir la generación de recomendaciones.                                                        |
| **Métrica**             | Una ejecución que incluya datos de 15 sucursales debe completar la generación de recomendaciones sin errores funcionales y producir resultados para todas las sucursales con información válida. |
| **Origen**              | Visión del producto, 18 de agosto de 2026. Cifra ajustada de 50 a 15 sucursales tras un ejercicio de elicitación, 22/09/2026, para mantener un margen de crecimiento razonable; sigue siendo un supuesto propio sujeto a validación. |
| **Prioridad**           | Importante                                                                                                                                                                                       |
| **Por qué importa**     | OrderFlow debe ser útil para cadenas con múltiples puntos de venta y no limitarse a una sola sucursal.                                                                                           |
| **Afecta a**            | RF-005, RF-007, RF-013, RF-014, RF-015                                                                                                                                                           |

---

## 5. Casos de uso

### 5.1 Actores

Identificados a partir del ejercicio de elicitación del 22/09/2026. Las descripciones representan tareas habituales de cada actor.

| Actor | Descripción |
|---|---|
| **Responsable de sucursal** | Dueño de una tienda de abarrotes independiente o encargado de una sucursal dentro de una cadena. Consulta ventas, inventario y recomendaciones por sucursal. |
| **Responsable de compras de la cadena** | Dueño o encargado que consolida varias sucursales. Consulta recomendaciones de múltiples sucursales y prepara listas de compra agrupadas por proveedor. |

### 5.2 Lista de casos de uso

Confirmados tras el ejercicio de elicitación del 22/09/2026:

| ID    | Caso de uso                                      | Requisitos relacionados                |
| ----- | ------------------------------------------------ | -------------------------------------- |
| CU-01 | Consultar información de ventas e inventario     | RF-001, RF-002, RF-005                 |
| CU-02 | Consultar información de productos y proveedores | RF-003, RF-004                         |
| CU-03 | Generar recomendaciones de compra | RF-001, RF-002, RF-003, RF-004, RF-005, RF-006, RF-007, RF-008, RF-009, RF-010, RF-011, RF-012 |
| CU-04 | Consultar y analizar una recomendación           | RF-011, RF-012, RF-014                 |
| CU-05 | Generar listas de compra por proveedor          | RF-013, RF-015                         |
| CU-06 | Consultar información consolidada de sucursales  | RF-005, RF-014, RF-015                 |

No se identificaron requisitos sin caso de uso asociado ni casos de uso sin requisitos detrás.

### 5.3 Caso de uso detallado · CU-03 Generar recomendaciones de compra

| Campo | Contenido |
|---|---|
| **Actores** | Responsable de sucursal y responsable de compras de la cadena. |
| **Objetivo** | Obtener la cantidad de compra recomendada para un producto de una sucursal o para todos los productos de esa sucursal. |
| **Precondición** | La sucursal está registrada. Para una solicitud individual, el producto pertenece al catálogo de la sucursal. |
| **Escenario principal** | 1. El usuario selecciona la sucursal y pide el cálculo individual o el cálculo para todos sus productos. 2. El sistema determina los productos que va a procesar. 3. Por cada producto, obtiene ventas históricas, inventario, características y condiciones de compra aplicables. 4. Si las entradas permiten calcular, estima la demanda y obtiene la necesidad a partir del inventario, el plazo de entrega y la caducidad cuando aplique. 5. Si la necesidad es positiva, aplica las condiciones de compra a la cantidad. 6. Guarda las recomendaciones con su justificación y devuelve los resultados e incidencias por producto. |
| **Flujos alternos** | 2a. La sucursal no tiene productos registrados. El sistema devuelve un conjunto vacío e informa el motivo.<br>3a. Si falta el dato de inventario o una condición indispensable para calcular, informa la incidencia y omite ese producto. Un inventario registrado con valor cero es válido.<br>4a. Si el historial utilizable es insuficiente para una estimación confiable, marca la recomendación como de baja confianza. Si el historial no permite estimar, registra una incidencia y omite ese producto.<br>5a. Si la necesidad es cero, devuelve una cantidad recomendada de cero.<br>5b. Si el ajuste comercial supera la necesidad calculada, indica el posible exceso. Cada incidencia afecta solo al producto correspondiente. |
| **Postcondición** | Las recomendaciones generadas quedan disponibles para consulta posterior en CU-04. El usuario puede seleccionarlas para preparar listas por proveedor en CU-05. |
| **Requisitos relacionados** | RF-001 a RF-012, RNF-REN-001 y RNF-CON-001. |

Véase el [diagrama de casos de uso](diagramas/caso-de-uso.md#diagrama-de-casos-de-uso).

---

## 6. Trazabilidad

| Requisito   | Origen                            | Caso de uso                                            | Elemento del prototipo              |
| ----------- | --------------------------------- | -------------------------------------------------------- | ------------------------------------ |
| RF-001      | Visión del producto               | CU-01 Consultar información de ventas e inventario / CU-03     | Vista de ventas                     |
| RF-002      | Visión del producto               | CU-01 Consultar información de ventas e inventario / CU-03     | Vista de inventario                 |
| RF-003      | Visión del producto               | CU-02 Consultar información de productos y proveedores / CU-03 | Vista de producto                   |
| RF-004      | Visión del producto               | CU-02 Consultar información de productos y proveedores / CU-03 | Vista de proveedor                  |
| RF-005      | Visión del producto               | CU-01 / CU-06 / CU-03                                          | Selector de sucursal                |
| RF-006      | Visión del producto               | CU-03 Generar recomendaciones de compra                | Motor de cálculo                    |
| RF-007      | Visión del producto               | CU-03 Generar recomendaciones de compra                | Vista de recomendaciones            |
| RF-008      | Regla de negocio                  | CU-03 Generar recomendaciones de compra                | Parámetros del cálculo              |
| RF-009      | Regla de negocio                  | CU-03 Generar recomendaciones de compra                | Parámetros de productos perecederos |
| RF-010      | Regla de negocio                  | CU-03 Generar recomendaciones de compra                | Condiciones de compra               |
| RF-011      | Visión del producto               | CU-04 Consultar y analizar una recomendación / CU-03           | Detalle de recomendación            |
| RF-012      | Visión del producto               | CU-04 Consultar y analizar una recomendación / CU-03           | Indicador de riesgo                 |
| RF-013      | Visión del producto               | CU-05 Generar listas de compra por proveedor          | Lista de compra                    |
| RF-014      | Visión del producto               | CU-04 Consultar y analizar una recomendación           | Filtro por sucursal                 |
| RF-015      | Visión del producto               | CU-05 / CU-06                                          | Vista consolidada                   |
| RNF-REN-001 | Derivado del tipo de sistema      | CU-03                                                  | Generación de recomendaciones       |
| RNF-REN-002 | Derivado del tipo de sistema      | CU-04                                                  | Consulta de recomendaciones         |
| RNF-SEG-001 | Supuesto propio                   | Todos                                                  | Autenticación                       |
| RNF-SEG-002 | Supuesto propio                   | CU-01 a CU-06                                          | Separación de información           |
| RNF-USA-001 | Derivado del objetivo del sistema | CU-04                                                  | Vista de recomendación              |
| RNF-USA-002 | Visión del producto               | CU-04                                                  | Detalle de recomendación            |
| RNF-CON-001 | Derivado del tipo de sistema      | CU-03                                                  | Motor de cálculo                    |
| RNF-MAN-001 | Supuesto propio                   | CU-03                                                  | Motor de cálculo                    |
| RNF-ESC-001 | Visión del producto               | CU-05 / CU-06                                          | Vista consolidada                   |

---

## 7. Registro de cambios

| Fecha      | Requisito | Qué cambió                                                        | Por qué                                                                  |
| ---------- | --------- | ------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| 17/09/2026 | Todos     | Creación de la primera versión de la especificación de requisitos | Elaboración inicial de los requisitos a partir de la Visión del producto |
| 22/09/2026 | RF-006, RF-009, RF-010, RF-011, RF-012 | Se anotó que estos requisitos fueron explorados en un ejercicio de elicitación | Resultado del ejercicio de elicitación (roleplay), 22/09/2026 |
| 22/09/2026 | RNF-ESC-001 | Se corrigió la métrica de 50 a 15 sucursales | El ejercicio de elicitación sugirió que la cifra original no tenía base real |
| 22/09/2026 | RNF-SEG-002 | Se agregó una nota sobre una pregunta abierta: si un encargado de sucursal debería ver información de otras sucursales de la misma empresa | Surgió en el ejercicio de elicitación, no estaba contemplado en el supuesto original |
| 22/09/2026 | Sección 5 | Se agregaron actores, se confirmaron los seis casos de uso y se redactó completo CU-03 con escenario principal y flujos alternos | Resultado del ejercicio de casos de uso posterior a la elicitación |
| 17/11/2026 | Alcance, requisitos y casos de uso | Se distinguió la generación individual de la generación completa por sucursal y se precisaron las condiciones de compra y el resultado de CU-05 | Cierre del alcance de OrderFlow |

---
