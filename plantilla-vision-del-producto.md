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

*Instrucción: nombre del sistema y qué hace, en un párrafo que cualquier persona entienda sin ser del área. Si necesitas usar una palabra técnica para explicarlo, todavía no está listo.*

**Nombre del sistema:** OrderFlow

**Descripción:** Es una herramienta digital para tiendas de embutidos y abarrotes que ayuda a saber exactamente cuánto producto pedir a sus proveedores para evitar que la mercancía se eche a perder en bodega o que las vitrinas se queden vacías. El sistema revisa lo que se vende día a día y genera automáticamente la lista de compras ideal para cada proveedor, indicando la cantidad justa de kilos o piezas que se deben encargar. El programa deberá considerar diferentes sucursales, encontrar patrones anuales, ubicación, y otros factores para optimizar la cantidad de producto que se pide utilizando un modelo matemático. 

---

## 2. Problema y usuarios

*Instrucción: qué problema resuelve, a quién le sirve y, muy importante, qué hace esa gente hoy para arreglárselas sin el sistema. Esa última parte es la que revela el problema real.*

**El problema:** La falta de control en el inventario y en las compras provoca pérdidas de dinero de dos formas: por un lado, se tiran a la basura productos perecederos (como jamones, quesos o embutidos) porque vencen antes de venderse; por otro lado, se pierden ventas cuando un cliente busca un producto popular y no hay existencia disponible.

**Cómo se resuelve hoy sin el sistema:** El encargado o dueño camina por la bodega y las vitrinas revisando "a ojo" qué se ve vacío, o anota en un cuaderno lo que cree que se venderá durante la semana únicamente con su intuición y memoria.

**Usuarios del sistema:**

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
|Tiendas de Abarrotes|Calcular cantidad exacta de encargos | Exceso o inventario insuficiente |
|Tiendas de Embutidos|Calcular cantidad exacta de encargos | Que se eche a perder el producto |
| | | |

*Instrucción: necesitas al menos dos tipos de usuario con necesidades distintas. Si los dos quieren exactamente lo mismo, probablemente sean el mismo usuario.*

**Un conflicto entre usuarios:** Las tiendas de abarrotes normalmente no tienen como preocupación la fecha de caducidad de los productos (o no de todos) entonces no es necesario considerar la caducidad del producto para estimar el encargo.

*Instrucción: describe algo que un usuario quiera y que a otro le estorbe. Ahí está tu primera decisión de diseño real.*

---
 
---
## 3. Alcance

*Instrucción: lo que escribes en "fuera del alcance" es lo que después evita que el proyecto crezca sin control. Sé específico: "reportes" no dice nada, "reportes de ventas mensuales exportables a PDF" sí.*

### Dentro del alcance

-
-
-
-

### Explícitamente fuera del alcance

-
-
-

**Por qué queda fuera:**

*Instrucción: para al menos una de las exclusiones, explica la razón. Puede ser tiempo, complejidad, o que no aporta al problema central.*

---

## 4. Tipo de sistema y restricciones

*Instrucción: identifica de qué tipo es tu sistema y qué te obliga a garantizar ese tipo. Un sistema de información y un sistema crítico no se diseñan igual.*

**Tipo de sistema:**

*(De información · Embebido · Crítico · Web y SaaS · De datos y análisis)*

**Por qué es de ese tipo:**

**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
| | | |
| | | |
| | | |

**Reglas de negocio que ya identifiqué:**

*Instrucción: reglas que no son obvias desde fuera y que alguien que conoce el dominio tendría que explicarte. Si no encuentras ninguna, tu caso puede ser demasiado simple.*

1.
2.
3.

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
