Ahora vas a hacer la seccion de "productos", ahorita en la pantalla principal solamente vas a poner unicamente el boton de "agregar una nueva", y vamos a hacer primero esa pantalla, guiandote de esta foto

pce precio venta, coast, lo que me cuesta comprarlo, su es código de barra o de inventario, unis como se vende, controlStock ingresar un stinimo y actual si es true, isPublished que ya aparezca en el sitio cuando lo quieran crear, isFeatured es variantes

![[Pasted image 20260928102110.png]]

esta es la estructura del post

```json
{
  "name": "string",
  "price": 0,
  "cost": 0,
  "categoryId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "sku": "string",
  "description": "string",
  "promoPrice": 0,
  "unit": "string",
  "tag": "string",
  "imageUrl": "string",
  "controlStock": true,
  "minStock": 0,
  "isPublished": true,
  "isFeatured": true,
  "isActive": true
}
```

en categoryId como miras es un select, ahi tendrias que poner este endpoint "api/v1/categories", asi como unit

el de "vender por", tambien es un select, pero ese todavia no esta el endpoint, asi que haz uno de mentira y dime donde tendria que poner el de verdad

dime si tienes alguna duda

---
Qué hice
- /products: solo muestra el botón "Agregar producto", que lleva a /products/new.
- /products/new ("Registre sus productos"), armada según la foto:
  - Barra superior: "Destacar producto" (estrella → isFeatured) y el switch "Mostrar en el Catálogo Online" (→ isPublished).
  - Columna izquierda: vista previa de la tarjeta (etiqueta o imagen, nombre y precio). Debajo: nombre y precio (obligatorios); en "Opcionales" van precio de promoción (tiene que ser menor que el precio), categoría (select), nombre de la etiqueta (tag), descripción, URL de la imagen, código del producto (sku), costo y "Vender por" (unit, select que arranca en "Unidad").
  - Columna derecha: tarjeta de Stock. Si "Controlar stock del producto" está activo, se habilitan stock actual y stock mínimo, y el mínimo pasa a ser obligatorio.
  - Abajo: barra fija con "Descartar" (vuelve a /products) y "Guardar producto" (con spinner y toast al terminar).
  - isActive se envía siempre en true.
- Todo vive en src/features/products/ (api, hooks, mocks, schema de Zod y componentes). También toqué dos primitivos de shadcn: el Switch, que con la base de 10px salía diminuto, y FormField, para que acepte schemas que convierten el texto de los inputs a número.

Endpoints
- Categorías: src/features/products/api/categories.ts llama a /api/v1/categories. Con VITE_USE_MOCKS=true usa 4 categorías de prueba.
- "Vender por" (el que todavía no existe): hoy devuelve siempre las unidades de prueba de mocks/productsMock.ts, aunque VITE_USE_MOCKS=false. El endpoint real va en src/features/products/api/saleUnits.ts. Ahí dejé un comentario TODO(backend) con el código listo para cambiar el mock por la llamada real.
- POST del producto: src/features/products/api/products.ts. Como no me pasaste la ruta, supuse /api/v1/products.

Lo que dejé fuera de la foto porque no está en el POST: el color de la etiqueta, "Registro automático" con IA, "Historial de movimientos", "Producto con variación" y Duplicar/Eliminar (esos dos son de la pantalla de edición).

Dudas
1. isFeatured: me dijiste que "es variantes", pero en la foto "Destacar producto" es justo eso (featured = destacado). Lo conecté a la estrella. ¿Está bien, o de verdad tiene que ver con variantes?
2. Stock actual: el POST solo tiene minStock. El campo "Stock actual" aparece y se valida, pero no se envía. ¿Falta un campo en el backend (stock?) o se carga por otro endpoint?
3. Imagen: el POST pide imageUrl. ¿Hay un endpoint para subir la foto y que devuelva la URL? Mientras tanto dejé un campo de texto para pegar la URL.
4. Ruta del POST: ¿es /api/v1/products?
5. Formatos de respuesta: ¿categorías devuelve un arreglo de { id, name }? ¿Y qué valor espera unit? En el mock usé códigos como "unit", "kg", "lb" con etiquetas en español.
6. Campos opcionales vacíos (costo, promoción, categoría, etc.): hoy no se mandan en el body. ¿El backend los acepta así o prefiere 0 / null?

---
descargar el formato y solo se sube

