# Diagramas de arquitectura — OrderFlow

**Proyecto:** OrderFlow  
**Materia:** Ingeniería de Software I · SIS3407  
**Formato:** Mermaid renderizable en GitHub  
**Base documental:** `especificacion-requisitos.md`, versión 1.1 (22/09/2026), y descripción del funcionamiento actual proporcionada por el responsable del proyecto.

## Criterio de modelado

OrderFlow **no administra los movimientos operativos de inventario ni compra automáticamente a proveedores**. Obtiene información del sistema del cliente mediante un *adapter*, valida, limpia y normaliza los datos, calcula la demanda mediante un modelo predictivo y obtiene recomendaciones de compra **por producto y sucursal**. Después aplica restricciones del producto y del proveedor, y permite consultar recomendaciones y preparar listas de compra agrupadas por proveedor.

Los diagramas representan la **arquitectura lógica propuesta a partir del comportamiento descrito**, no una auditoría del código implementado. Como no se indicó un lenguaje de programación, framework, motor de base de datos o protocolo de integración, **no se atribuye ninguna tecnología específica**. La separación entre aplicación web, backend, procesamiento de datos y motor predictivo en el nivel 2 es una **propuesta conceptual** y no implica que hoy se desplieguen como servicios independientes.

## 1. C4 — Nivel 1: contexto del sistema

Muestra quién utiliza OrderFlow y con qué sistemas externos intercambia información, sin entrar en tecnologías de implementación.

```mermaid
flowchart TD
    sucursal(["Responsable de sucursal"])
    compras(["Responsable de compras de la cadena"])
    orderflow["OrderFlow<br/>Sistema de apoyo a la planeación de compras"]
    origen["Sistema del cliente<br/>Ventas, inventario, productos, sucursales y proveedores"]

    sucursal -->|"Consulta recomendaciones y su justificación"| orderflow
    compras -->|"Consulta varias sucursales y prepara listas por proveedor"| orderflow
    origen -->|"Proporciona datos por integración mediante adapter"| orderflow
    orderflow -->|"Presenta cantidades recomendadas por producto y sucursal"| sucursal
    orderflow -->|"Presenta listas de compra agrupadas"| compras

    style orderflow fill:#ff5d00,color:#fff,stroke:#cc4900
```

**Límite de alcance:** los proveedores son entidades de negocio consideradas por las reglas de compra, pero aquí no se dibujan como actores de integración externa porque **no está confirmado que OrderFlow les envíe pedidos automáticamente**. Tampoco se muestra una pasarela de pago.

## 2. C4 — Nivel 2: contenedores lógicos

Presenta las responsabilidades mayores dentro de OrderFlow. Las cajas son **contenedores lógicos**: su despliegue físico o tecnología queda pendiente de confirmar.

```mermaid
flowchart TD
    usuario(["Responsable de sucursal o compras"])
    cliente["Sistema del cliente<br/>Fuente de datos"]

    subgraph orderflow["OrderFlow — frontera del sistema"]
        interfaz["Interfaz de consulta<br/>Recomendaciones y listas de compra"]
        api["Backend de aplicación<br/>Solicitudes, acceso y consultas"]
        integracion["Integración de datos<br/>Adapters y normalización"]
        motor["Motor de recomendación<br/>Predicción y reglas de compra"]
        datos[("Almacenamiento lógico<br/>Datos normalizados y recomendaciones")]
    end

    usuario -->|"Consulta y solicita recomendaciones"| interfaz
    interfaz -->|"Solicitudes y resultados"| api
    cliente -->|"Datos de origen"| integracion
    integracion -->|"Registros limpios por producto y sucursal"| datos
    api -->|"Solicita generación"| motor
    motor -->|"Lee datos preparados"| datos
    motor -->|"Guarda recomendaciones ajustadas"| datos
    api -->|"Consulta resultados y listas por proveedor"| datos
    api -->|"Responde a la interfaz"| interfaz

    style motor fill:#ff5d00,color:#fff,stroke:#cc4900
```

**Nota:** el almacenamiento lógico representa la persistencia que permitiría consultar recomendaciones posteriormente (CU-03). No presupone un gestor de base de datos específico. El motor predictivo y las reglas podrían ejecutarse en un mismo proceso o en procesos separados.

## 3. C4 — Nivel 3: componentes del procesamiento y recomendación

Descompone el flujo interno desde la integración de datos hasta la cantidad final recomendada, considerando que el proveedor condiciona la cantidad comprable.

```mermaid
flowchart TD
    fuente["Datos del sistema del cliente"]
    ui["Interfaz de consulta"]

    subgraph procesamiento["Integración y preparación de datos"]
        adapter["Adapter de entrada<br/>Adapta el formato de cada cliente"]
        validacion["Validación y limpieza<br/>Detecta faltantes e inconsistencias"]
        normalizacion["Normalización<br/>Unifica productos, sucursales y unidades"]
    end

    subgraph recomendacion["Motor de recomendación"]
        preparacion["Preparación de variables<br/>Ventas históricas e inventario"]
        predictor["Modelo predictivo<br/>Estima demanda por producto y sucursal"]
        calculo["Cálculo de necesidad<br/>Demanda estimada menos inventario disponible"]
        reglas["Reglas de producto y proveedor<br/>Entrega, presentación, mínimos y caducidad"]
        resultados["Resultado por producto y sucursal<br/>Cantidad ajustada, explicación y riesgo"]
        agrupador["Agrupación por proveedor<br/>Listas de compra"]
    end

    catalogo[("Datos preparados<br/>Ventas, inventario y catálogos")]
    persistencia[("Recomendaciones generadas")]

    fuente -->|"Registros originales"| adapter
    adapter -->|"Datos adaptados"| validacion
    validacion -->|"Datos válidos o incidencias"| normalizacion
    normalizacion -->|"Datos consistentes"| catalogo
    catalogo -->|"Información por producto y sucursal"| preparacion
    preparacion -->|"Variables para predicción"| predictor
    predictor -->|"Demanda esperada"| calculo
    catalogo -->|"Existencias disponibles"| calculo
    calculo -->|"Necesidad preliminar"| reglas
    catalogo -->|"Condiciones del proveedor y producto"| reglas
    reglas -->|"Cantidad compatible con las condiciones"| resultados
    resultados -->|"Guardar recomendaciones"| persistencia
    persistencia -->|"Recomendaciones seleccionadas"| agrupador
    persistencia -->|"Consulta de resultados y justificación"| ui
    agrupador -->|"Lista agrupada, no pedido enviado"| ui

    style predictor fill:#ff5d00,color:#fff,stroke:#cc4900
    style reglas fill:#fff0e8,color:#222,stroke:#ff5d00
```

**Excepciones previstas por la especificación:** sin inventario disponible no se genera una recomendación para ese producto y sucursal; si faltan suficientes datos históricos, la recomendación puede marcarse como de baja confianza; si el mínimo o la presentación del proveedor provocan sobrecompra, debe indicarse el riesgo de exceso. No se afirma que estas reglas ya estén implementadas.

## 4. Diagrama de secuencia — generación de recomendaciones

Detalla el recorrido lógico de una solicitud. El procesamiento puede ejecutarse bajo demanda o por lote; el mecanismo exacto de disparo queda pendiente de confirmar.

```mermaid
sequenceDiagram
    actor Usuario as Responsable de compras o sucursal
    participant UI as Interfaz
    participant App as Backend
    participant Adapter as Adapter del cliente
    participant Datos as Preparación de datos
    participant Modelo as Modelo predictivo
    participant Reglas as Reglas de proveedor
    participant Repo as Almacenamiento

    Usuario->>UI: Solicita recomendaciones de una sucursal
    UI->>App: Solicitar generación
    App->>Adapter: Obtener datos del cliente
    Adapter-->>App: Ventas, inventario y catálogos adaptados
    App->>Datos: Validar, limpiar y normalizar
    Datos-->>App: Datos preparados e incidencias

    alt Falta inventario del producto y sucursal
        App-->>UI: Informar que no se puede recomendar ese registro
    else Existe inventario válido
        App->>Modelo: Predecir demanda por producto y sucursal
        Modelo-->>App: Demanda esperada y confianza disponible
        App->>Reglas: Ajustar necesidad con inventario y condiciones
        Note over Reglas: Considera tiempo de entrega, unidad de compra, mínimos y caducidad cuando aplica
        Reglas-->>App: Cantidad final, justificación y riesgo disponible
        App->>Repo: Guardar recomendaciones
        Repo-->>App: Resultados disponibles
        App-->>UI: Mostrar recomendaciones por producto y sucursal
        UI-->>Usuario: Consultar cantidades y explicación
        opt El usuario prepara listas de compra
            Usuario->>UI: Selecciona recomendaciones
            UI->>App: Agrupar selección por proveedor
            App-->>UI: Listas de compra separadas por proveedor
            UI-->>Usuario: Presenta listas para su revisión
        end
    end
```

## Relación con los requisitos

| Elemento arquitectónico | Requisitos principales |
| --- | --- |
| Adapter, limpieza y normalización | RF-001 a RF-005 como información de entrada; integración especificada adicionalmente por el responsable del proyecto |
| Modelo predictivo | RF-006 |
| Necesidad y recomendación por producto y sucursal | RF-007, RF-014 |
| Ajustes por tiempo de entrega, caducidad y compra mínima | RF-008, RF-009, RF-010 |
| Justificación y riesgo | RF-011, RF-012 |
| Listas por proveedor y consolidación de sucursales | RF-013, RF-015 |
| Protección de acceso y separación entre empresas | RNF-SEG-001, RNF-SEG-002; transversal a los componentes |
