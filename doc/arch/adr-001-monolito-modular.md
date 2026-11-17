# ADR-001 · Aplicación web modular para OrderFlow

**Estado:** Propuesto

**Fecha:** 17/11/2026

## Contexto

El ejercicio de elicitación describe una cadena de seis sucursales que consolida manualmente la información de compras. OrderFlow necesita calcular cantidades con ventas e inventario por producto y sucursal. Puede recibir una solicitud para un producto o para todos los productos de la sucursal. También necesita conservar las recomendaciones para su consulta posterior.

El proceso actual depende de reportes manuales. Cada empresa puede tener datos de origen en formatos distintos. La meta de rendimiento de RNF-REN-001 es procesar hasta 10 000 registros históricos de una sucursal y dejar disponible el resultado completo en menos de 10 segundos.

## Alternativas consideradas

| Alternativa | Ventaja | Costo para OrderFlow |
|---|---|---|
| Aplicación web modular y PostgreSQL | El cálculo individual y el recorrido por sucursal comparten módulos y acceso a datos dentro de una aplicación. | La generación por sucursal comparte capacidad de cómputo con las consultas. |
| Aplicación web y servicio de predicción separado | Permite operar y escalar el cálculo de forma independiente. | Agrega comunicación entre procesos, manejo de fallas y coordinación de despliegues para el caso inicial de seis sucursales. |

## Decisión propuesta

Usar una aplicación web modular en Python con FastAPI y vistas HTML de Jinja2. PostgreSQL almacena los datos normalizados y las recomendaciones con su empresa y sucursal de origen. La aplicación se organiza en cinco módulos: Presentación, Integración de datos, Recomendaciones, Compras y Persistencia.

En Integración de datos, el patrón **Adapter** convierte los registros externos al formato que usa OrderFlow. La entrada inicial propuesta es una carga CSV. La aplicación valida y normaliza los datos antes de guardarlos. En Recomendaciones, el modo sucursal recorre su catálogo y llama a la misma operación de cálculo que el modo individual.

## Consecuencias

- **A favor.** Se opera una sola aplicación web y un almacén de datos. La fórmula y las condiciones de compra se mantienen en un sitio y se reutilizan en ambos modos.
- **A favor.** Incorporar otro formato de datos puede concentrarse en un adaptador de entrada. Pronóstico, reglas de compra y consultas comparten el origen de datos normalizado.
- **En contra.** Una generación para toda la sucursal puede competir por recursos con las consultas. Hay que medir el tiempo completo con hasta 10 000 registros históricos, incluidos los productos con incidencias.
- **En contra.** La carga por CSV depende de que la empresa entregue archivos actualizados y compatibles. El desfase entre la fecha de corte y el cálculo puede reducir la utilidad de la recomendación.

## Validación de la decisión

Medir RNF-REN-001 con un conjunto representativo y verificar que el mismo producto, con las mismas entradas y configuración, dé el mismo resultado en ambos modos. Si las consultas repetidas de datos dominan el tiempo, evaluar la precarga por sucursal. Si el cálculo sigue comprometiendo el objetivo, revisar la separación del proceso predictivo.

[C4 de OrderFlow](diagramas.md) · [Modelo UML](modelo-uml.md)
