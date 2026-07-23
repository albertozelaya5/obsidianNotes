
## 1. Instalar dependencias (solo desarrollo)

```bash
npm i -D eslint @eslint/js typescript-eslint eslint-plugin-react-hooks eslint-plugin-react-refresh globals
```

- Omitir `typescript-eslint` si el proyecto es JavaScript puro (usar solo `@eslint/js`).
- `eslint-plugin-react-hooks` / `eslint-plugin-react-refresh` son las reglas específicas
  de React (antes las traía `eslint-config-react-app`).

## 2. Crear `eslint.config.js/ts` en la raíz del proyecto

```js
import js from "@eslint/js";
import globals from "globals";
import reactHooks from "eslint-plugin-react-hooks";
import reactRefresh from "eslint-plugin-react-refresh";
import tseslint from "typescript-eslint";

export default tseslint.config(
  { ignores: ["dist"] },
  {
    extends: [js.configs.recommended, ...tseslint.configs.recommended],
    files: ["**/*.{ts,tsx}"], //* {js,jsx} SI ES JS
    languageOptions: {
      ecmaVersion: 2020,
      globals: globals.browser,
    },
    plugins: {
      "react-hooks": reactHooks,
      "react-refresh": reactRefresh,
    },
    rules: {
      ...reactHooks.configs.recommended.rules,
      "react-refresh/only-export-components": ["warn", { allowConstantExport: true }],
    },
  },
);
```

## 3. Agregar el script de lint en `package.json`

```json
{
  "scripts": {
    "lint": "eslint ."
  }
}
```

```bash
npm run lint
```

## 4. NO usar `vite-plugin-eslint`

ESLint **no** se mete como plugin de Vite. Corre aparte por CLI (`npm run lint`).
Para ver los errores mientras escribes (en vivo, en el editor), instalar la
extensión **ESLint** del editor (VS Code: `dbaeumer.vscode-eslint`) — lee
`eslint.config.js` automáticamente, sin tocar `vite.config.js`.

## Opcional: integrar con Prettier

Si también usas Prettier para formato, evitar conflictos de reglas con:

```bash
npm i -D prettier eslint-config-prettier eslint-plugin-prettier
```

Y en `eslint.config.js`, agregar al final del array:

```js
import eslintPluginPrettier from "eslint-plugin-prettier/recommended";

export default tseslint.config(
  // ...configs anteriores
  eslintPluginPrettier,
);
```

## Resumen: qué cambió vs. la config vieja (CRA)

| Antes (CRA / legacy)                          | Ahora (flat config)                                     |
| ---------------------------------------------- | --------------------------------------------------------- |
| `.eslintrc.json` con `"extends": "react-app"` | `eslint.config.js` con `tseslint.config(...)`             |
| `eslint-config-react-app`                     | `@eslint/js` + `typescript-eslint` + plugins específicos |
| `vite-plugin-eslint` en `vite.config.js`      | Nada — se corre por CLI (`npm run lint`) + extensión de editor |