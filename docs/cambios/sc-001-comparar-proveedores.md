# SC-001 · Comparar opciones de proveedores para un producto

## Estado

**Rechazada · 17/11/2026.** La comparación agrega trabajo de datos, interfaz y pruebas al alcance inicial. Los fundamentos y la estimación están en el [análisis de impacto](ai-001-comparar-proveedores.md).

**Registro del documento:** Solicitud recibida de la dupla de revisión. El original no indica la fecha de solicitud.

## Quién la pide

Iker Gabriel Fonseca Fernández · Dupla de revisión.

## Qué se pide

Permitir que el dueño compare dos proveedores de un mismo producto antes de decidir a quién comprarle. Para cada uno, el sistema mostraría el tamaño del paquete, el tiempo de entrega y cuántos paquetes se necesitan para cubrir la cantidad recomendada.

## Por qué

Un proveedor puede vender paquetes más grandes o tardar más en entregar. Ver las dos opciones juntas ayudaría al dueño a elegir una que alcance a llegar y cubra lo que necesita la sucursal.

## Qué pasa si no se hace

El sistema todavía puede ayudar a planear las compras, pero el dueño tendría que comparar las opciones y calcular los paquetes por separado. Podría tardar más o equivocarse al comparar.

## Tipo de mantenimiento

- [ ] Correctivo
- [ ] Adaptativo
- [x] Perfectivo
- [ ] Preventivo

Es perfectivo porque añade una forma de comparar proveedores para apoyar la decisión de compra.

## Criterio de aceptación

Para una necesidad recomendada de 45 kg de leche, registrar un proveedor A que vende paquetes de 20 kg y tarda 2 días, y un proveedor B que vende paquetes de 15 kg y tarda 4 días. El sistema muestra ambos: A requiere 3 paquetes para cubrir al menos 45 kg y B requiere 3 paquetes. También muestra el tiempo de entrega de cada uno y la cantidad total que se compraría: 60 kg con A y 45 kg con B.

El dueño toma la decisión final; el sistema no realiza el pedido automáticamente. Las cantidades y tiempos del ejemplo son datos de prueba.

## Prioridad que sugiere el solicitante

Media. Puede facilitar la compra cuando hay varios proveedores, aunque el pronóstico y la recomendación básica siguen siendo lo primero.
