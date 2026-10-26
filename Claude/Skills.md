
[Emil Kowalski's Design Skills](https://agenticskills.io/skills/emilkowalski-skills)

Skills de diseño de Emil Kowalski (creador de Sonner y Vaul) para que el agente construya UI con animaciones, transiciones y microinteracciones pulidas y con buen criterio de diseño.

```bash
npx skills add emilkowalski/skills
```

[Impeccable](https://agenticskills.io/skills/impeccable)

Conjunto de skills y comandos de diseño frontend que ayudan al agente a evitar la estética "genérica de IA": tipografía, color, espaciado, jerarquía y revisión/pulido de interfaces.

```
npx impeccable install
```

[Taste Skill](https://agenticskills.io/skills/taste-skill)

Le da al agente "buen gusto" de diseño frontend: guías para elegir layouts, tipografía y estilos modernos en vez de los defaults típicos.

```bash
npx skills add Leonxlnx/taste-skill@design-taste-frontend
```

[Claude Figma MCP](https://developers.figma.com/docs/figma-mcp-server/remote-server-installation/#claude-code)

Conecta Claude Code con Figma: puede leer frames/componentes, variables y tokens de un archivo para convertir diseños en código, y también crear o editar diseños en Figma.

```bash
claude plugin install figma@claude-plugins-official
```

Después de instalar: reiniciar Claude Code y autenticarse con `/mcp` → figma.

### Cómo pedirle a Claude un layout y pasarlo a código

**Opción A — Yo diseño en Figma, Claude lo pasa a código (la más confiable)**
1. En Figma, seleccionar el frame → clic derecho → **Copy link to selection**.
2. Pegarle el link a Claude, por ejemplo:
   > "Implementa este diseño como la pantalla `src/app/berries.tsx` en React Native: `https://www.figma.com/design/XXXX/...?node-id=12-345`. Usa los componentes y estilos que ya existen en el proyecto."

   Claude lee el frame (estructura, medidas, colores, tipografía, variables y assets) y escribe el código.

**Opción B — Claude crea el layout en Figma**
> "Crea en mi archivo de Figma `<link del archivo>` un layout mobile (390×844) para una pantalla de lista de berries: header con búsqueda, grid de 2 columnas con cards (imagen, nombre, sabor) y tab bar abajo."

Ajustarlo a mano en Figma y luego seguir con la Opción A:
> "Ahora replica este frame en código: `<link>`"

**Tips para que el código salga fiel**
- Usar **Auto Layout** en Figma: se traduce directo a flexbox.
- **Nombrar las capas** (`BerryCard`, `SearchBar`): se vuelven los nombres de los componentes.
- Usar **variables/estilos** para colores y tipografía, así se generan como theme en vez de valores hardcodeados.
- Pedir **un frame a la vez**, no el archivo completo.
- En React Native, recordarle que use `View`, `Text`, `StyleSheet`, `expo-image` (no HTML/CSS web).

[Playwright CLI Skills](https://playwright.dev/agent-cli/skills)

Enseña al agente a usar `playwright-cli` para controlar un navegador real: navegar, hacer clic, tomar snapshots, manejar sesiones, mockear red, guardar cookies/localStorage, grabar traces/videos y generar, ejecutar y depurar tests de Playwright.

```bash
# 1. Instalar el CLI
npm install -g @playwright/cli

# 2. Instalar la skill
playwright-cli install --skills            # en el proyecto (.claude/skills/playwright-cli)
playwright-cli install --skills=agents     # layout .agents/skills
playwright-cli install --skills -g         # global (home), disponible en todos los proyectos
```
