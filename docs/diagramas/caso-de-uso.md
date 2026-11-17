# Casos de uso de OrderFlow

## Diagrama de casos de uso

```mermaid
flowchart LR
    S["Responsable de sucursal"]
    C["Responsable de compras de la cadena"]

    subgraph OF["OrderFlow"]
        U1(["CU-01 · Consultar ventas e inventario"])
        U2(["CU-02 · Consultar productos y proveedores"])
        U3(["CU-03 · Generar recomendaciones<br/>Un producto o todos los de una sucursal"])
        U4(["CU-04 · Consultar y analizar una recomendación"])
        U5(["CU-05 · Generar listas de compra por proveedor"])
        U6(["CU-06 · Consultar información consolidada de sucursales"])
    end

    S --- U1
    S --- U3
    S --- U4
    C --- U1
    C --- U2
    C --- U3
    C --- U4
    C --- U5
    C --- U6
```

## Caso principal

La [ficha de CU-03](../especificacion-requisitos.md#53-caso-de-uso-detallado--cu-03-generar-recomendaciones-de-compra) documenta las entradas, el cálculo por producto y los flujos alternos. Las asociaciones del diagrama representan la participación habitual de cada actor.
