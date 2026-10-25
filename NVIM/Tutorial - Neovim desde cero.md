
## La idea clave: los modos

Neovim no funciona como un editor normal. Tiene **modos**, y cada tecla hace algo distinto según el modo en que estés:

|Modo|Para qué sirve|Cómo entrar|
|---|---|---|
|**Normal**|Moverte y dar órdenes (modo por defecto)|`Esc`|
|**Insertar**|Escribir texto como en cualquier editor|`i`|
|**Visual**|Seleccionar texto|`v`|
|**Comando**|Guardar, salir, buscar y reemplazar|`:`|

La regla de oro: **si te pierdes, presiona `Esc`** y vuelves al modo Normal.

## 1. Abrir, guardar y salir

En la terminal escribe:

```
nvim archivo.txt
```

Dentro de Neovim (en modo Normal):

- `:w` y luego Enter guarda
- `:q` sale
- `:wq` guarda y sale
- `:q!` sale **sin guardar**, útil si hiciste un desastre

## 2. Escribir texto

1. Presiona `i` para entrar al modo Insertar.
2. Escribe normalmente.
3. Presiona `Esc` para volver al modo Normal.

> [!TIP]
Otras formas útiles de entrar a Insertar son `a` (escribe después del cursor), `o` (crea una línea nueva abajo) y `O` (crea una línea nueva arriba).

## 3. Moverte (en modo Normal)

Las flechas funcionan, pero lo típico es usar estas teclas:

```
        k (arriba)
h (izq)        l (der)
        j (abajo)
```

Otros movimientos rápidos:

- `w` salta a la siguiente palabra y `b` a la anterior
- `0` va al inicio de la línea y `$` al final
- `gg` va al inicio del archivo y `G` al final

## 4. Editar (en modo Normal)

- `x` borra un carácter
- `dd` borra (corta) la línea entera
- `yy` copia la línea
- `p` pega
- `u` deshace
- `Ctrl + r` rehace

**Truco:** puedes poner un número antes de una orden. Por ejemplo, `3dd` borra 3 líneas y `5j` baja 5 líneas.

## 5. Seleccionar

1. Presiona `v` y muévete para seleccionar.
2. Luego presiona `y` para copiar, `d` para borrar/cortar o `p` para pegar encima.

## 6. Buscar y reemplazar

- `/palabra` y Enter busca la palabra. Con `n` vas a la siguiente coincidencia y con `N` a la anterior.
- `:%s/viejo/nuevo/g` reemplaza todas las veces que aparece "viejo" por "nuevo".

## Tu primera práctica (5 minutos)

1. Abre un archivo con `nvim prueba.txt`.
2. Presiona `i`, escribe tres líneas y presiona `Esc`.
3. Muévete con `j` y `k`.
4. Borra una línea con `dd` y recupérala con `u`.
5. Copia una línea con `yy` y pégala con `p`.
6. Guarda y sal con `:wq`.

## Siguiente paso recomendado

Neovim trae un tutorial interactivo buenísimo. Escribe esto en la terminal:

```
nvim +Tutor
```

Te toma unos 30 minutos y te enseña con práctica real.

Al principio se siente lento, pero en una o dos semanas vas a moverte más rápido que con el mouse.

