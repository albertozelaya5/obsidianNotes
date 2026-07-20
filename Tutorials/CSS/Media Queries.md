## How Media Queries Work


- Cuando hacemos uso de desktop first => `max-width`

- Cuando hacemos uso de mobile first => `min-width`


Por ejemplo, si queremos aplicar un estilo que este desde los 0px hasta los 600px


```css

@media (max-width: 600px);

```

Lo que esto chequea es `Is width <= 600px` ?

Es como decir "Mientras sea menor o igual a este ancho, haz esto"

Sino, los efectos no se aplican

- Se le llama asi porque este sera el `max-width` en el que todavía funcionara

- Aquí sobre escribimos propiedades que están en el resto del código, pero el otro código seguirá igual

Se le puede pensar como herramienta que sobre escriben partes del css hasta un cierto ancho del viewport

---

Ahora, si tenemos dos media queries, uno max-width 600px y otro 1200px

Y tenemos un teléfono de 400px, cual se aplicara?

> La respuesta es: ambos

Porque ambas condiciones son correctas, sin embargo si hay conflictos, la ultima en ser declarada se aplicara

  

## How to Select Breakpoints

Hay diferentes técnicas para seleccionar breakpoints

### Que son los breakpoints?

Son la anchura del navegador, en la cual queremos que cambie nuestro diseño, los px values que queremos poner en los media queries

  

---

### ❌ BAD Strategy

- Antes, usábamos el ancho de los apple devices y lo dábamos como hecho => ❌ Ya no se hace, ya que al ponerlo para dispositivos específicos, no crea la mejor experiencia para los demás

- También, no es bueno para mantenimiento en el futuro, ya que si se lanzan nuevos dispositivos Apple, se tendrá que volver a redimensionar

### ✅ GOOD Strategy

- Se basa en rangos de ancho de pantalla

- Miramos los anchos mas usados, de las diferentes categorías de dispositivos, y luego tratamos de agruparlos de manera lógica, para hacer nuestros puntos a partir de eso

  

- Podemos asumir que la mayoría de teléfonos, se encuentran entre 300px - 500px

- Tablets 600px - 900px , podemos ponerlos en algún lugar de esos 600px

- Tablets grandes, landscape tablets 900px - 1100px

- Laptops > 1200px

  

Entonces, los breakpoints estarían al final de la anchura promedio de cada uno de los dispositivos => 600px, 900px, 1200px


### ✅✅ PERFECT Strategy

Poner los breakpoints cuando el design breaks down, en esta estrategia podemos ignorar todas las mobile categories devices


- Comenzamos con un size, ya sea desktop o mobile, y comenzamos a incrementarlo o decrementarlo

- Luego, tan pronto como el diseño se rompe, o sea que ya no luce aceptable, creamos un nuevo breakpoint

Y tratar de hacerlo sin pensar en dispositivos, ahora, es complicado hacerlo sin pensar en dispositivos, como esto sera para phone, tablets, etc

Asi que, vamos a utilizar esta estrategia, en conjunto con la good strategy
