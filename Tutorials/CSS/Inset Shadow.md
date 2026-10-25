```css
.element{
	box-shadow: inset 0 0 0 3px #fff
}
```
## Web Design Rules #5: Shadows

Pueden ser an important part in helping users figure out the relationships between parts of our designs

Or using to add interesting visual details

### Concepts

Los shadows can be used on boxes and text

> Skeuomorphic design

- Lleno de detalles, efectos y muchos shadows(sombras)

> Flat design

- No more realistic details, gloss effects or shadows

- Reducido a lo esencial(minimalistic)

> Flat design 2.0

- Sin el realismo, pero trajo de vuelta esos shadows y depth(profundidades) to better usage

- Mientras el shadow(sombra) sea mas grande, es como si estuviera mas lejos el elemento de la pantalla


Los shadows emulan o crean una cierta profundidad en nuestras user interfaces, como una 3ra dimension

Mientras mas shadow le pongamos, mas profundidad tendrá

Simula la existencia de una luz externa


### Use Shadows Well

1. Realmente no son necesarios si no encajan con nuestra `web personality`

Serio/Elegante = Less shadows - Playful/fun = More shadows

Debemos tener una idea de que tan serios o elegantes queremos ser

2. Use Shadows in small doses(dosis): don't add shadows to every element!

Lo usamos para que un elemento destaque del resto, hacer algo mas prominente que el resto del contenido

3. Go light on shadows, don't make them too dark!

Lo natural es mas sutil, nunca vemos algo tan oscuro in the real world

Poner shadows en todos lados, y hacerlos muy oscuros, es una forma fácil de arruinar un diseño

### Use Shadows in the right Situation

4. Use small shadows for smaller elements that should stand out(to draw attention)

Como small forms, cards, buttons, mini boxes

El main color y el shadow llaman nuestra atención

Son importantes para la historia que cuentan, o mostrar el producto que esta siendo vendido

5. Use medium-sized shadows for larger areas that should stand out a bit more

Haciendo storytelling, or normal cards

6. Use large shadows for elements that should really float above the interface

Ejemplos serian navigation, or pop up windows como forms(también llamados call to action)

7. Experiment with changing shadows on mouse interaction(click and hover)

Por ejemplo que al inicio no tenga shadow, cuando en el hover medio, y al darle click solo un poco


El efecto con esto es que al usuario estar en hover arrastre el botón cerca de el, y al presionar se reduce haciendo un efecto de empujar ese botón

8. Experiment with glows(colored shadows)

Para botones, buttons dentro de forms

También se puede usar un text-shadow muy sutil, cuando se tiene texto on a top of an image
## Implementing Shadows

- Desplazamiento horizontal(offset) 1-2

- Desplazamiento vertical

- El blur(difuminado) del shadow (mientras mas size, mas blur)

- El penúltimo(opcional) Que tanto se esparce o hace mas grande la sombra, 10px,si no se especifica will be 0

- El ultimo es el color

Cuando queremos representar un color con cierta opacidad, usamos rgba

```css

text-shadow: 0 5px 5px rgba(0, 0, 0, 0.2)

```

  

```css
.chair {
	box-shadow: 0px 20px 0px 0px rgba(0, 0, 0, 0.8);
}
```

Para un shadow, usamos un valor de opacity bajo(como 0.1, 0.05, 0.07)

Si se deja el blur en 0, se hace como si fuera un box

El blur no debe hacer que se solapen unos con otros

20px de separación es un buen size para un tamaño medio

Examples for small, medium and big

> Small

- offset 5px, blur 10px, spread 0px, rgba (0.05)

> Medium

- offset 20px, blur 30px, spread 0px, rgba (0.07)

> Big

- offset 40px, blur 60px, spread 0px, rgba (0.1)