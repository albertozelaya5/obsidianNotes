```markdown
--- 01 TYPOGRAPHY SYSTEM

- Font sizes (px)
10 / 12 / 14 / 16 / 18 / 20 / 24 / 30 / 36 / 44 / 52 / 62 / 74 / 86 / 98

--- 02 SPACING SYSTEM

- Spacing system (px)
2 / 4 / 8 / 12 / 16 / 24 / 32 / 48 / 64 / 80 / 96 / 128
```

## Line Height and Letter spacing

Ambos se toman en pixeles

```markdown
line-height: 1.1; <!-- Entre 1, 1.5 and 2 normal, big text go menos de 1.5 -->
letter-spacing: -1px; <!-- Entre 1px y -1px de lo normal -->
```

## CSS Lines to Begin a Project

```css
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

html {
  /* font-size: 10px; */
  /* 10px / 16px = 0.625 = 62.5% */
  /* Percentage of user's browser font-size setting */
  font-size: 62.5%;
  overflow-x: hidden;
  /* Does NOT work on Safari */
  /* scroll-behavior: smooth; */
}

body {
  font-family: "Rubik", sans-serif;
  line-height: 1;
  font-weight: 400;
  color: #555;
  /* Only works if there is nothing absolutely positioned in relation to body */
  overflow-x: hidden;
}
```
### Ingredients of Web Design

  
- Topography que consiste en formatear y diseñar el texto
- Colors
- Images/Illustrations
- Icons
- Shadows
- Border-radius la redondez de los elementos en una pagina
- Whitespace
- Visual Hierarchy
- User Experience
- Components/layouts

