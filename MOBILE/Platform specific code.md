Cómo hacer que algo se vea o funcione distinto en **iOS** y **Android**.

Docs: https://reactnative.dev/docs/platform-specific-code#platform-module

Hay dos formas:
- **Módulo `Platform`**: para diferencias pequeñas (un estilo, un valor).
- **Archivos por plataforma** (`.ios.tsx` / `.android.tsx`): para diferencias grandes (un componente entero distinto).

## Módulo `Platform`

```tsx
import { Platform, StyleSheet } from 'react-native';
```

### `Platform.OS`: saber en qué plataforma estoy

Devuelve `'ios'` o `'android'` (y `'web'` si corre en web con Expo).

```tsx
const styles = StyleSheet.create({
  header: {
    height: Platform.OS === 'ios' ? 200 : 100,
  },
});
```

### `Platform.select()`: elegir un valor por plataforma

Recibe un objeto con claves `ios`, `android`, `native` o `default` y devuelve el valor de la plataforma actual.

```tsx
const styles = StyleSheet.create({
  container: {
    flex: 1,
    ...Platform.select({
      ios: { backgroundColor: 'red' },
      android: { backgroundColor: 'green' },
      default: { backgroundColor: 'blue' }, // web u otras
    }),
  },
});
```

| Clave     | Aplica a                                  |
| --------- | ----------------------------------------- |
| `ios`     | Solo iOS                                  |
| `android` | Solo Android                              |
| `native`  | iOS y Android (no web)                    |
| `default` | Lo que no coincida con ninguna de arriba  |

> [!TIP] También sirve para elegir **componentes**, no solo estilos:
> ```tsx
> const Component = Platform.select({
>   ios: () => require('ComponentIOS'),
>   android: () => require('ComponentAndroid'),
> })();
> ```

### `Platform.Version`: versión del sistema

| Plataforma | Qué devuelve                         | Ejemplo  |
| ---------- | ------------------------------------ | -------- |
| Android    | **Número** del nivel de API          | `33`     |
| iOS        | **String** con la versión del sistema | `'17.2'` |

```tsx
if (Platform.OS === 'android' && Platform.Version >= 33) {
  // Android 13 o más nuevo
}

const majorIOS = parseInt(Platform.Version as string, 10); // en iOS hay que convertirlo
```

## Archivos por plataforma

Si el código cambia mucho, se separa en dos archivos con extensión de plataforma:

```
BigButton.ios.tsx
BigButton.android.tsx
```

Y se importa **sin** la extensión:

```tsx
import BigButton from './BigButton'; // React Native elige el archivo correcto
```

> [!NOTE] Con `.native.tsx` se separa **móvil vs web**: `Container.native.tsx` (iOS + Android) y `Container.tsx` (web).

## Resumen

| Necesito...                               | Uso                          |
| ----------------------------------------- | ---------------------------- |
| Un `if` rápido según plataforma           | `Platform.OS`                |
| Un valor/estilo distinto por plataforma   | `Platform.select()`          |
| Saber la versión de Android / iOS         | `Platform.Version`           |
| Un componente completamente distinto      | `.ios.tsx` / `.android.tsx`  |

Relacionado: [[Core components]] · [[Ruta React Native]]
