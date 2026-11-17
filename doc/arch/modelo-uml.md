# Modelo UML de OrderFlow

El flujo principal desarrolla [CU-03](../../docs/especificacion-requisitos.md#53-caso-de-uso-detallado--cu-03-generar-recomendaciones-de-compra). CU-04 consulta las recomendaciones guardadas y CU-05 agrupa una selección en listas por proveedor.

## Paquetes de la aplicación

Cada paquete corresponde al componente del [nivel 3 de C4](diagramas.md#c4-nivel-3--componentes-de-la-aplicación-web). El diagrama usa la notación de flujo de Mermaid para representar dependencias entre paquetes.

```mermaid
flowchart LR
    P["«package» Presentación"]
    I["«package» Integración de datos"]
    R["«package» Recomendaciones"]
    C["«package» Compras"]
    D["«package» Persistencia"]

    P -.-> I
    P -.-> R
    P -.-> C
    P -.-> D
    I -.-> D
    R -.-> D
    C -.-> D
```

## Clases

El modelo de clases tiene una vista de dominio y otra de las operaciones de CU-03. Las cantidades usan valores decimales para admitir unidades como kilogramos. Cada recomendación conserva el producto, la sucursal, la condición de compra aplicada y la justificación.

### Entidades de dominio

```mermaid
classDiagram
    direction TB

    class Cliente {
        -String id
        -String nombre
    }

    class Sucursal {
        -String id
        -String nombre
    }

    class Producto {
        -String id
        -String sku
        -String nombre
        -String unidad
        -Integer vidaUtilDiasOpcional
    }

    class VentaHistorica {
        -Date fecha
        -Decimal cantidad
    }

    class InventarioSucursal {
        -Decimal existenciaActual
        -Date fechaCorte
    }

    class Proveedor {
        -String id
        -String nombre
    }

    class CondicionProveedor {
        -Decimal tamanoPaquete
        -Decimal cantidadMinima
        -Decimal multiploCompra
        -Integer plazoEntregaDias
        +ajustarCantidad(necesidad) Decimal
    }

    class PronosticoDemanda {
        -String periodo
        -Decimal demandaEstimada
        -Boolean bajaConfianza
    }

    class RecomendacionCompra {
        -Decimal cantidadSugerida
        -Decimal inventarioUtilizado
        -String justificacion
        -Boolean bajaConfianza
        -Boolean posibleExceso
    }

    Cliente "1" --> "0..*" Sucursal : tiene
    Cliente "1" --> "0..*" Producto : cataloga
    Cliente "1" --> "0..*" Proveedor : registra
    Sucursal "1" --> "0..*" InventarioSucursal : contiene
    Producto "1" --> "0..*" InventarioSucursal : existencias
    Sucursal "1" --> "0..*" VentaHistorica : registra
    Producto "1" --> "0..*" VentaHistorica : ventas
    Proveedor "1" --> "0..*" CondicionProveedor : ofrece
    Producto "1" --> "0..*" CondicionProveedor : condiciones
    PronosticoDemanda "1" --> "0..*" RecomendacionCompra : fundamenta
    CondicionProveedor "1" --> "0..*" RecomendacionCompra : condiciona
    Sucursal "1" --> "0..*" RecomendacionCompra : recibe
    Producto "1" --> "0..*" RecomendacionCompra : corresponde
```

### Clases que colaboran en CU-03

Las flechas punteadas representan dependencias de uso. CondicionProveedor también aparece en la vista de dominio.

```mermaid
classDiagram
    direction TB

    class VistaRecomendaciones {
        +solicitarGeneracion(sucursalId, productoIdOpcional)
    }

    class ControladorRecomendaciones {
        +generar(sucursalId, productoIdOpcional) List~ResultadoProducto~
    }

    class MotorRecomendaciones {
        +listarProductos(sucursalId) List~Producto~
        +generarProducto(sucursalId, productoId) ResultadoProducto
    }

    class RepositorioOrderFlow {
        +listarProductos(sucursalId) List~Producto~
        +obtenerDatosProducto(sucursalId, productoId) DatosProducto
        +guardarRecomendacion(recomendacion)
    }

    class ModeloPredictivo {
        -String version
        +predecir(historial, periodo) PronosticoDemanda
    }

    class ReglasCompra {
        +calcularRecomendacion(datos, pronostico) RecomendacionCompra
    }

    class CondicionProveedor {
        -Decimal tamanoPaquete
        -Decimal cantidadMinima
        -Decimal multiploCompra
        -Integer plazoEntregaDias
        +ajustarCantidad(necesidad) Decimal
    }

    VistaRecomendaciones ..> ControladorRecomendaciones
    ControladorRecomendaciones ..> MotorRecomendaciones
    MotorRecomendaciones ..> RepositorioOrderFlow
    MotorRecomendaciones ..> ModeloPredictivo
    MotorRecomendaciones ..> ReglasCompra
    ReglasCompra ..> CondicionProveedor
```

El repositorio entrega DatosProducto, que reúne el historial de ventas, el inventario, las características del producto y la condición de compra aplicable. ResultadoProducto identifica el producto y contiene una recomendación o el motivo por el cual no pudo calcularse.

## Secuencia · CU-03 Generar recomendaciones

La secuencia usa llamadas con los mismos nombres que las operaciones de clases. Por ejemplo, el mensaje «generarProducto(sucursalId, productoId)» llega a **MotorRecomendaciones**, donde se declara esa operación. El modo sucursal aporta varios productos al mismo ciclo.

```mermaid
sequenceDiagram
    actor Usuario
    participant Vista as VistaRecomendaciones
    participant Controlador as ControladorRecomendaciones
    participant Motor as MotorRecomendaciones
    participant Repo as RepositorioOrderFlow
    participant Modelo as ModeloPredictivo
    participant Reglas as ReglasCompra
    participant Condicion as CondicionProveedor

    Usuario->>Vista: solicitarGeneracion(sucursalId, productoIdOpcional)
    Vista->>Controlador: generar(sucursalId, productoIdOpcional)
    opt Solicitud para toda la sucursal
        Controlador->>Motor: listarProductos(sucursalId)
        Motor->>Repo: listarProductos(sucursalId)
        Repo-->>Motor: Productos
        Motor-->>Controlador: Productos
    end

    loop Para cada producto solicitado
        Controlador->>Motor: generarProducto(sucursalId, productoId)
        Motor->>Repo: obtenerDatosProducto(sucursalId, productoId)
        Repo-->>Motor: Historial, inventario y condiciones
        alt Faltan entradas indispensables o el historial no permite estimar
            Motor-->>Controlador: Incidencia del producto
        else Las entradas permiten estimar
            Motor->>Modelo: predecir(historial, periodo)
            Modelo-->>Motor: Demanda estimada y baja confianza, si aplica
            Motor->>Reglas: calcularRecomendacion(datos, pronostico)
            opt La necesidad calculada es positiva
                Reglas->>Condicion: ajustarCantidad(necesidad)
                Condicion-->>Reglas: Cantidad ajustada
            end
            Reglas-->>Motor: Recomendación y posible exceso, si aplica
            Motor->>Repo: guardarRecomendacion(recomendacion)
            Motor-->>Controlador: Resultado del producto
        end
    end

    Controlador-->>Vista: Resultados e incidencias
    Vista-->>Usuario: Muestra los resultados de la solicitud
```

## Actividad · CU-03 Generar recomendaciones

Las calles muestran las acciones del usuario y de OrderFlow. Cuando el usuario solicita todos los productos, una incidencia llega a la salida del producto y el flujo avanza al siguiente. El cálculo de necesidad considera inventario, entrega y caducidad cuando aplique. Una solicitud de sucursal con catálogo vacío devuelve el conjunto vacío e informa el motivo.

```mermaid
flowchart TD
    subgraph usuario["Usuario"]
        Inicio((Inicio))
        Solicitar("Seleccionar sucursal y modo de generación")
        Seleccionar("Elegir producto")
        Revisar("Revisar resultados e incidencias")
        Fin(((Fin)))
    end
    subgraph sistema["OrderFlow"]
        Modo{"¿Qué se solicitó?"}
        Catalogo("Cargar el catálogo de la sucursal")
        Pendientes{"¿Quedan productos por procesar?"}
        Datos("Leer ventas, inventario y condiciones del producto")
        Entradas{"¿Hay datos suficientes para calcular?"}
        Incidencia("Registrar incidencia del producto")
        Pronostico("Estimar demanda y su confianza")
        Necesidad("Calcular necesidad de compra")
        Positiva{"¿La necesidad es positiva?"}
        Cero("Recomendar cantidad cero")
        Ajuste("Aplicar presentación, múltiplo y mínimo")
        Exceso{"¿El ajuste supera la necesidad?"}
        Advertir("Indicar posible exceso")
        Guardar("Guardar recomendación con su justificación")
        Entregar("Devolver resultados e incidencias")
    end

    Inicio --> Solicitar --> Modo
    Modo -->|"[Un producto]"| Seleccionar --> Pendientes
    Modo -->|"[Toda la sucursal]"| Catalogo --> Pendientes
    Pendientes -->|"[Sí]"| Datos --> Entradas
    Pendientes -->|"[No]"| Entregar --> Revisar --> Fin
    Entradas -->|"[No]"| Incidencia --> Pendientes
    Entradas -->|"[Sí]"| Pronostico --> Necesidad --> Positiva
    Positiva -->|"[No]"| Cero --> Guardar
    Positiva -->|"[Sí]"| Ajuste --> Exceso
    Exceso -->|"[Sí]"| Advertir --> Guardar
    Exceso -->|"[No]"| Guardar
    Guardar --> Pendientes
```
