Hay dos maneras, las usamos dependiendo si queremos ir a un elemento especifico, o al inicio o al final del documento

### window.scrollTo()

Este es un método, que acepta los siguientes parametros

```js
const section1 = document.querySelector('#section--1');
const s1coords = section1.getBoundingClientRect(); //*setBounding objeto que contiene las coordeadas

window.scrollTo({
left: s1coords.left + window.pageXOffset,
top: s1coords.top + window.pageYOffset, //*Se tiene que sumar la altura relativa del viewport, mas la altura absoluta del scroll
behavior: "smooth"
}); //*ocupamos que este del todo a la izquierda, para el viewport
```

- `window.pageYOffset` → cuánto bajaste desde el **top del documento** hasta el borde superior del viewport
- `s1coords.top` → distancia desde el **borde superior del viewport** hasta el elemento

El `left` SOLO es necesario si el usuario scrollea horizontalmente
## element.scrollIntoView()

Este es un método, que acepta los siguientes parámetros

```js
const section1 = document.querySelector('#section--1');
section1.scrollIntoView({ behavior: 'smooth' });
```
