
Podemos establecer un fondo de varios colores con esta propiedad

Primero debemos llamarla asi

```css
.cta {
background-image: linear-gradient(90deg, rgb(230, 126, 34), rgb(235, 152, 78));
/* background-image: linear-gradient(to right, red, rgb(235, 152, 78)); */
}
```

Y se hace dentro de `background-image`, ya que se se le trata a esta función como una imagen

- Como primer argumento, tenemos el `degree` o la dirección de los colores, que usualmente va en sentido contrario

- Si ponemos `to right` el ultimo color establecido ira a la derecha, y asi sucesivamente

- O podemos usar esquinas (corners), por ejemplo `to right bottom`, siendo el ultimo color el que ira en al esquina inferior derecha

---
  

Otra cosa que podemos hacer es poner una imagen con una sombra, tipo una imagen polarizada


```css
.cta-img-box {

background-image:
linear-gradient(to right bottom, rgba(235, 151, 78, 0.35), rgba(230, 125, 34, 0.35)), url("../img/eating.jpg");

}
```

