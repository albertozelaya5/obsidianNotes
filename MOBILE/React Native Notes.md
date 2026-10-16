![[Pasted image 20261015094104.png]]

![[Pasted image 20261015094732.png]]

![[Pasted image 20261015094939.png]]

`Instalar una App` => `npx create-expo-app@latest` (sino ir a [Expo Docs](https://docs.expo.dev/))
`inicializar la App` => `npx expo start`
`Dejar solo lo escencial de la plantilla` => `npm run reset-project`

Se usa el archivo `_layout.tsx`, para definir un stack(pila) de pantallas
``
```tsx
export default function RootLayout() {
  return <Stack />;
}
```

De esta forma, `index.tsx` es el punto de entrada de la aplicacion

En los componentes como `index.tsx` se usan ciertos componentes para sustituir las `html tags`:

| `Text`       | Se usa para escribir texto                                                                                                                         |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `View`       | Para ver información (excepto arrays)                                                                                                              |
| `ScrollView` | Para ver información (Para arrays, como padre de `View`) (es el UNICO que no funciona con `style`; sino `contentContainerStyle` - que es lo mismo) |
| `Image`      | Ver imagenes, acepta `source={{ uri: pokemon.image }}` para agregar la url, ocupa `width` y `height` como otras props                              |
|              |                                                                                                                                                    |
Pressable component

Tambien existe styles como `StyleSheet.create` para crear estilos afuera del componente

```tsx
<Text style={styles.name}>{pokemon.name}</Text>

const styles = StyleSheet.create({
  name: {
    fontSize: 28,
    fontWeight: "bold",
    textAlign: "center",
  },
  type: {
    fontSize: 20,
    fontWeight: "bold",
    color: "grey",
    textAlign: "center",
  },
});
```

### `_Layout.tsx` para ver las pages
Al crear un componente dentro de la carpeta `app` este sera una ruta, por lo que debe ser en minúscula

```tsx
<Stack.Screen
name="details"
options={{
  title: "Details",
  headerBackButtonDisplayMode: "minimal", //* como de visualiza el boton de regresar
  presentation: "formSheet", //* como se visualiza la pantalla(modal, form, etc)
  sheetAllowedDetents: [0.3, 0.5, 0.7], //* cuando se arrastre, en que sizes es permitido
  sheetGrabberVisible: true, //* que se permita moldear su size
  headerShown: false, //* mostrar el titulo de la pagina
}}
/>
```

`Stack.Screen` es usada para editar información de la propia pagina, se puede usar dentro de `_layout.tsx` como en cualquier otra pagina

| `name`    | siempre debe ser el nombre de la ruta         |
| --------- | --------------------------------------------- |
| `options` | objecto con opciones de lo que se puede hacer |
> [!ALERT] El size es solo numérico, ya que aquí cada unidad mide lo mismo en todos los teléfonos

