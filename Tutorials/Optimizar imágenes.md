## Comandos sharp-cli (via bunx)

| #   | Comando                                                                                                             | Qué hace                                                                           |
| --- | ------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| 1   | `bunx sharp-cli --help`                                                                                             | Muestra todas las opciones disponibles (verifica flags según la versión instalada) |
| 2   | `bunx sharp-cli -i src/assets/tractor-used.jpg -o src/assets/optimized/tractor-used.webp -f webp -q 80`             | Convierte un solo archivo `.jpg` a `.webp` con calidad 80                          |
| 3   | `bunx sharp-cli -i src/assets/tractor-used.jpg -o src/assets/optimized/tractor-used.webp -f webp -q 80 resize 1200` | Convierte y redimensiona a 1200px de ancho                                         |
| 4   | `bunx sharp-cli -i "src/assets/*.jpg" -o src/assets/optimized -f webp -q 80`                                        | Convierte en lote todos los `.jpg` de `src/assets/` a `src/assets/optimized/`      |
| 5   | `bunx sharp-cli -i "src/assets/*.jpg" -o src/assets/optimized -f webp -q 80 resize 1200`                            | Igual que el anterior, pero además redimensiona todas las imágenes                 |
**Regla práctica para este proyecto:** `1600px` de lado más largo + `-q 80` es el balance que ya usan las imágenes existentes en `src/assets/optimized/` — para fotos de tarjetas/cards como las que vas a usar en Providers, incluso `1000-1200px` de ancho sería suficiente y bajaría más el peso, ya que no se muestran a pantalla completa.

295.25*410.78px