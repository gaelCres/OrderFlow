# Guion de entrevista y notas post-entrevista · OrderFlow

**Unidad 2 · Requisitos de ingeniería de software · SIS3407**
**Fecha del ejercicio:** 22/09/2026
**Modalidad:** ejercicio de elicitación en vivo (roleplay por duplas)

---

## 1. Preguntas y respuestas, organizadas por tramo

### Contexto

**13. ¿Cuántas sucursales manejas hoy y esperas manejar en los próximos años?**
Hoy manejo 6 sucursales, y la idea es seguir creciendo, tal vez abrir una o dos más en los próximos años.

**15. ¿Quién más, aparte de ti, necesitaría ver estas recomendaciones?**
Creo que los encargados de cada sucursal también deberían poder verlas, para que sepan qué está pasando en las otras tiendas.

### Proceso actual

**1. Cuéntame cómo decides hoy cuánto pedirle a cada proveedor.**
Cada encargado de sucursal me manda por WhatsApp una lista de lo que cree que le va a faltar. Yo la reviso, la comparo mentalmente con lo que sé de cada proveedor, y armo el pedido consolidado. Hacemos esto los lunes y los jueves.

**2. ¿Qué tan distinta es la demanda entre tus sucursales?**
Varía bastante, cada sucursal tiene su propio ritmo.

**3. ¿Hay días o temporadas en que la venta cambia mucho?**
Sí, los fines de quincena la venta sube casi el doble.

**5. ¿Todos tus proveedores entregan con la misma frecuencia?**
No, no todos.

**6. ¿Hay proveedores con pedido mínimo o presentación fija?**
Sí, hay proveedores con pedido mínimo.

### Dolores

**4. ¿Cómo sabes hoy si un producto se va a agotar antes del próximo pedido?**
Pues básicamente por lo que me reporta el encargado de cada sucursal, es lo único con lo que cuento.

**8. ¿Qué tan seguido se te vence producto antes de venderlo?**
De vez en cuando pasa, no es todos los días pero sí ocurre.

**11. ¿Cómo te enteras si compraste de más en una sucursal?**
Normalmente hasta que el encargado me dice que le sobró, o cuando ya se ve que no se está moviendo.

### Excepciones

**7. ¿Qué pasa cuando un proveedor no puede surtir todo lo que pediste?**
Avisa un día antes con menos cantidad de la que pedí, y ahí tengo que decidir rápido a qué sucursal le mando lo poco que llegó.

**9. ¿Qué haces cuando a un producto le quedan pocos días de vida útil?**
Si le quedan menos de 3 días de vida útil ya no tengo margen, porque el proveedor no me lo cambia ni me lo regresa, así que trato de moverlo rápido o se pierde.

**12. ¿Alguna vez una sucursal tiene sobrante de algo que otra necesita?**
Sí, a veces pasa.

### Verificación de supuestos

**10. Cuando decides cuánto pedir, ¿qué te preocupa más, quedarte corto o comprar de más?**
Las dos cosas me preocupan, depende del producto y del momento.

**14. Si el sistema tardara unos segundos en darte una recomendación cada vez que la consultas, ¿te parecería aceptable o sería un problema?**
No creo que sea un problema, unos segundos los puedo esperar sin drama.

---

## 2. Notas después del ejercicio

### Supuestos que resultaron falsos

| Supuesto original | Qué salió en el ejercicio | Ajuste |
|---|---|---|
| RNF-ESC-001 asumía "al menos 50 sucursales" | Hoy maneja 6, planea abrir 1 o 2 más en los próximos años | Bajar la métrica a un rango más realista (10-15 sucursales) y anotar que el número anterior no tenía base real |

### Supuestos que se confirmaron

- La demanda varía por sucursal y sube en fines de quincena (respalda RF-006, estacionalidad/día).
- Hay proveedores con pedido mínimo (respalda RF-010).
- Los productos perecederos generan merma, aunque de forma ocasional, no diaria (respalda RF-009 como prioridad Importante, no Imprescindible).
- El faltante y el exceso preocupan por igual, dependiendo del producto y momento (respalda el conflicto de usuarios y RF-012).
- Unos segundos de espera al consultar son aceptables (respalda el orden de magnitud de RNF-REN-001/002; los valores exactos de 10s/3s siguen siendo supuesto propio).
- Hoy no hay visibilidad cruzada entre sucursales hasta que el encargado avisa (respalda que RF-011/RF-012 responden a una necesidad plausible, no inventada).

### Lo que apareció sin que se esperara

- La fuente de datos hoy es manual (WhatsApp de cada encargado), no un POS; el prototipo probablemente necesite aceptar carga manual además de la automática.
- El reparto de producto entre sucursales cuando un proveedor entrega menos de lo pedido es una decisión manual del dueño y queda fuera de alcance, pero refuerza el valor de mostrar el riesgo por sucursal (RF-012).
- Los encargados de sucursal quieren ver recomendaciones de otras sucursales, no solo la propia, lo que abre una pregunta pendiente de validar sobre permisos (RNF-SEG-002 no la cubre). También surgió que a veces sobra producto en una sucursal que otra necesita, un punto a futuro fuera del alcance actual.