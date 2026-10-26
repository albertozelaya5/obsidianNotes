Componentes base de React Native. Cada uno se convierte en la vista **nativa** de cada plataforma (no es HTML dentro de un navegador).

Docs: https://reactnative.dev/docs/intro-react-native-components

| React Native   | Android        | iOS             | Equivalente web           | Para qué sirve                                                                 |
| -------------- | -------------- | --------------- | ------------------------- | ------------------------------------------------------------------------------ |
| `<View>`       | `<ViewGroup>`  | `<UIView>`      | `<div>` sin scroll        | Contenedor: layout con flexbox, estilos, algo de toques y accesibilidad        |
| `<Text>`       | `<TextView>`   | `<UITextView>`  | `<p>`                     | Mostrar, estilizar y anidar texto. También recibe toques (`onPress`)           |
| `<Image>`      | `<ImageView>`  | `<UIImageView>` | `<img>`                   | Mostrar imágenes                                                               |
| `<ScrollView>` | `<ScrollView>` | `<UIScrollView>`| `<div>` con scroll        | Contenedor con scroll que puede tener varios componentes                       |
| `<TextInput>`  | `<EditText>`   | `<UITextField>` | `<input type="text">`     | Que el usuario escriba texto                                                   |

> [!ALERT] Todo texto debe ir dentro de `<Text>`. Un string suelto dentro de un `<View>` da error.

## Eventos de `TextInput` (el "event listener")

En web se usa `onChange` o `addEventListener("input")` y se lee `e.target.value`. En React Native se pasan **props** al `TextInput`:

| Prop              | Cuándo se dispara                    | Qué recibe                          | Equivalente web            |
| ----------------- | ------------------------------------ | ----------------------------------- | -------------------------- |
| `onChangeText`    | Cada vez que cambia el texto         | El **string** directo               | `onChange` / `"input"`     |
| `onChange`        | Cada vez que cambia el texto         | Evento: `e.nativeEvent.text`        | `onChange`                 |
| `onSubmitEditing` | Al presionar Enter / botón del teclado | Evento: `e.nativeEvent.text`      | `onSubmit` / `"keydown"` Enter |
| `onFocus` / `onBlur` | Al entrar / salir del input       | Evento                              | `"focus"` / `"blur"`       |

> [!TIP] Casi siempre se usa `onChangeText`: da el string directo, sin `e.target.value`. Por eso se puede pasar `setQuery` tal cual.

## Caso de uso: buscador con lista corta

Un `TextInput` que filtra una lista **limitada** (6 pokémon) mostrada en un `ScrollView`.

```tsx
import { useState } from "react";
import { Image, ScrollView, StyleSheet, Text, TextInput, View } from "react-native";

//* Lista corta y fija: por eso ScrollView + .map y no FlatList
const POKEMONS = ["bulbasaur", "charmander", "squirtle", "pikachu", "eevee", "snorlax"];

export default function Buscador() {
  const [query, setQuery] = useState("");

  const filtered = POKEMONS.filter((name) => name.includes(query.toLowerCase()));

  return (
    <View style={styles.container}>
      <Image
        source={{ uri: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/25.png" }}
        style={styles.logo} //* Image remota necesita width y height
      />

      <TextInput
        value={query}
        onChangeText={setQuery} //* cada tecla: recibe el string directo
        onSubmitEditing={(e) => console.log("buscar:", e.nativeEvent.text)} //* al presionar Enter
        placeholder="Buscar pokémon"
        autoCapitalize="none" //* evita que el teclado ponga mayúscula al inicio
        returnKeyType="search" //* el botón Enter del teclado dice "Buscar"
        style={styles.input}
      />

      <ScrollView contentContainerStyle={styles.list}>
        {filtered.map((name) => (
          <Text key={name} style={styles.item}>
            {name}
          </Text>
        ))}
        {filtered.length === 0 && <Text>Sin resultados</Text>}
      </ScrollView>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 16, gap: 12 },
  logo: { width: 96, height: 96, alignSelf: "center" },
  input: { borderWidth: 1, borderColor: "#ccc", borderRadius: 8, padding: 12, fontSize: 16 },
  list: { gap: 8 }, //* gap y padding van en contentContainerStyle, no en style
  item: { fontSize: 18, textTransform: "capitalize" },
});
```

Qué usa cada core component:
- `View`: el contenedor de la pantalla (`flex: 1` para ocupar todo el alto).
- `Image`: la imagen de arriba, con `source={{ uri }}`.
- `TextInput`: controlado con `value` + `onChangeText`, igual que un input controlado en React web.
- `ScrollView`: muestra la lista filtrada con `.map`.
- `Text`: cada nombre y el mensaje "Sin resultados".

> [!ALERT] `ScrollView` dibuja **todos** sus hijos de una vez. Sirve para listas cortas como esta. Si la lista es larga o viene de una API, usar `FlatList` (ver [[React Native Notes]]).
