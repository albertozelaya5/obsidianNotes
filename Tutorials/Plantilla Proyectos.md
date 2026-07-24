Una pagina (AQUI PONER DE QUE TRATA)

> [!IMPORTANT]
> - El proyecto esta hecho con shadcn, react ts, zustand y tailwind
> - Toda la nomenclatura del nombre de los archivos y carpetas es en ingles, para los archivos es `PascalCase.tsx`, para los ts que sean tipo `camelCase.ts` 

Puedes usar este sistema de espaciado y estas recomendaciones

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


Sus estilos visuales son Startup/Upbeat, y un poco de `Plain/Neutral`

estos son sus definiciones

### `Startup/Upbeat`: Software startups, and other modern-looking companies

  - Widely used in startups, featuring medium-sized sans-serif typeface,s light-grey text and backgrounds, and rounded elements

  Usa shadows y border radius

  > Ingredients:

  - Typography - Medium-sized headings(not too large), usually one sans-serif typeface in whole design. Tendency for lighter text colors
  - Colors - Blues, greens and purples are widely used. Lots of light backgrounds(mainly gray), gradients are also common
  - Images - Images or illustrations are always used. 3D illustrations are modern. Sometimes patterns and shapes add visual details
  - Icons - ✅ Icons are very frequent
  - Shadows - ✅ Subtle shadows are frequent. Glows are becoming modern
  - Border-radius - ✅ Some border-radius is very common
  - Layout - Rows of cards, rows of product features and Z-patterns are usual, as well as animations

### `Plain/Neutral`: Well-established corporations, companies that don't want to make an impact through design

  - Design that gets out of the way by using neutral ans small typefaces, and very structured layout. Common in big corporations

  > Ingredients:

  - Typography - Boxy/squared sans-serif typefaces, small body font sizes - Si se usara un accent podría ser un font diferente(serif)
  - Colors - Neutral-looking sans-serif typefaces are used, and text is usually small and doesn't have visual impact
  - Images - Images are frequently used, but usually in a small format, tal vez solo en el header una big image
  - Icons - Usually no icons, but small simple black icons may be used
  - Shadows - Usually no shadows ❌
  - Border-radius - Usually no border-radius ❌
  - Layout - Structured and condensed layout, with lots of boxes and rows

Ahora, este programa tiene su propio esquema de colores basado en la paleta de banhcafe, que es esta:

```css
@import "tailwindcss";
@import "tailwind-animations";
@plugin "@tailwindcss/typography";

/* ── Tema ── */
@theme {
  --font-sans: var(--font-open-sans), system-ui, sans-serif;

  /* Spacing — reemplaza los defaults (igual que theme.spacing sin extend) */
  --spacing-*: initial;
  --spacing-0: 0;
  --spacing-1: 0.4rem;
  --spacing-2: 0.8rem;
  --spacing-3: 1.2rem;
  --spacing-4: 1.6rem;
  --spacing-5: 2rem;
  --spacing-6: 2.4rem;
  --spacing-7: 2.8rem;
  --spacing-8: 3.2rem;
  --spacing-9: 3.6rem;
  --spacing-10: 4rem;
  --spacing-11: 4.4rem;
  --spacing-12: 4.8rem;
  --spacing-13: 5.2rem;
  --spacing-14: 5.6rem;
  --spacing-15: 6rem;
  --spacing-16: 6.4rem;
  --spacing-header: var(--header-height);

  /* Font size — reemplaza defaults */
  --text-*: initial;
  --text-xs: 1rem;
  --text-sm: 1.2rem;
  --text-md: 1.4rem;
  --text-lg: 1.6rem;
  --text-xl: 2rem;
  --text-xxl: 2.4rem;
  --text-xxl--line-height: 1.2;
  --text-h4: 2.8rem;
  --text-h4--line-height: 1.2;
  --text-h3: 3.6rem;
  --text-h3--line-height: 1.2;
  --text-h2: 4.2rem;
  --text-h2--line-height: 1.3;
  --text-5xl: 5.2rem;
  --text-5xl--line-height: 1.2;
  --text-h1: 6.2rem;
  --text-h1--line-height: 1.12;

  /* Font weight — reemplaza defaults */
  --font-weight-*: initial;
  --font-weight-thin: 350;
  --font-weight-normal: 450;
  --font-weight-semibold: 550;
  --font-weight-bold: 620;
  --font-weight-black: 690;

  /* Colores — extiende defaults (extend.colors en v3) */
  --color-primary: #a41f35;
  --color-primary-hover: rgba(186, 12, 47, 1);
  --color-secondary: #f8f8f8;
  --color-tertiary: rgba(52, 54, 66, 1);
  --color-background: #fbfbfb;
  --color-gray-background: #f5f5f5;
  --color-button: rgba(41, 45, 50, 0.7);
  --color-notification-container: rgba(217, 236, 254, 1);
  --color-accents-blue: #418fd8;

  --color-error-light: #fff2f2;
  --color-error-main: rgba(186, 12, 47, 1);

  --color-success-light: rgba(222, 237, 227, 1);
  --color-success-main: rgba(38, 183, 105, 1);

  --color-warning-light: rgba(255, 238, 225, 1);
  --color-warning-main: rgba(214, 124, 59, 1);

  --color-info-light: rgba(217, 236, 254, 1);
  --color-info-main: rgba(65, 143, 216, 1);

  --color-body: #ecedf1;
  --color-modal: #f3f5fa;
  --color-light-gray: #eeeeee;
  --color-light: rgba(0, 0, 0, 0.5);
  --color-extra-light: rgba(0, 0, 0, 0.03);
  --color-submenu: rgba(243, 245, 250, 0.9);
  --color-icon-light: rgba(177, 185, 216, 1);
  --color-input: rgba(240, 245, 250, 1);
  --color-gray-body: rgba(236, 237, 241, 1);
  --color-gray-light: rgba(238, 238, 238, 1);
  --color-text-subtitle: rgba(52, 54, 66, 1);
  --color-icon-background: rgba(233, 200, 200, 0.18);
  --color-icon-background-hover: rgba(233, 200, 200, 0.3);
  --color-icon-border: rgba(186, 12, 47, 0.3);

  /* Background images */
  --background-image-promotion-background-top: url("/src/assets/images/promotion-background-top.webp");
  --background-image-promotion-background-bottom: url("/src/assets/images/promotion-background-bottom.png");
  --background-image-about-us-background: url("/src/assets/images/about-us.webp");
  --background-image-container:
    linear-gradient(0deg, #ffffff, #ffffff),
    radial-gradient(116.16% 288.31% at 93.33% 50%, #c3002f 0%, #ffffff 62.5%);
  --background-image-input-box: linear-gradient(0deg, #f0f5fa, #f0f5fa);

  /* Shadows */
  --shadow-container:
    0px 12px 30px -50px rgba(53, 62, 74, 0.1),
    0px 4px 4px 0px rgba(53, 62, 74, 0.1);
  --shadow-card: 0px 80px 28px -60px rgba(84, 0, 14, 0.5);
  --shadow-submenu: 0px 4px 10px rgba(101, 111, 126, 0.2);
  --shadow-card-shadow: 0px 10px 16px -3px rgba(101, 111, 126, 0.2);
  --shadow-quick-link: 0 3px 0 0 rgba(0, 0, 0, 0.1);
  --shadow-search-box: 0 0 0 5px rgba(255, 255, 255, 0.2);
  --shadow-focus: 0px 0px 0px 4px rgba(164, 31, 53, 0.08);
  --shadow-error-shadow: 0px 0px 0px 4px rgba(244, 221, 221, 1);

  /* Animations */
  --animate-progress: progress-bar 20s linear;

  @keyframes progress-bar {
    from {
      width: 0%;
    }
    to {
      width: 100%;
    }
  }
}

/* ── Base ── */
@layer base {
  html {
    @apply bg-white text-zinc-800;
    font-size: 62.5%;
  }

  body {
    font-size: var(--text-md);
    font-family: var(--font-sans);
  }

  :root {
    --header-height: 6rem;
  }
}

/* ── Components ── */
@layer components {
  .glass {
    box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.15);
    border-color: rgba(255, 255, 255, 0.3);
    border-width: 1px;
    backdrop-filter: brightness(100%) blur(7px);
  }

  .embed-html ol,
  .embed-html ul {
    padding-left: 2rem;
    padding-block: 1rem;
  }
  .embed-html ol {
    list-style-type: decimal;
  }
  .embed-html ul {
    list-style-type: disc;
  }

  .background-texture {
    @apply bg-white/70 bg-[url('/src/assets/images/background.webp')] bg-cover bg-center bg-no-repeat bg-blend-overlay;
  }

  .background-texture-primary {
    @apply bg-primary/50 bg-[url('/src/assets/images/background.webp')] bg-cover bg-center bg-no-repeat bg-blend-multiply;
  }

  .no-scroll {
    overflow: hidden;
    @apply pr-4;
  }

  .container {
    @apply bg-body/70 shadow-container backdrop-blur;
  }

  .slider {
    @apply h-2 w-full rounded-full bg-neutral-100;
  }
  .slider-thumb {
    @apply bg-accents-blue h-4 w-4 -translate-y-1/4 cursor-pointer rounded-lg outline-none;
  }
  .slider-thumb.slider-thumb-1 {
    @apply border-accents-blue border-2 bg-white;
  }
  .slider-track {
    @apply h-2;
  }
  .slider-track.slider-track-1 {
    @apply bg-accents-blue;
  }

  @keyframes slide {
    0% {
      opacity: 0;
    }
    8% {
      opacity: 1;
    }
    20%,
    30% {
      opacity: 1;
    }
    50% {
      opacity: 0;
    }
  }

  @keyframes slide-info {
    0% {
      opacity: 0;
    }
    8% {
      opacity: 1;
    }
    20%,
    30% {
      opacity: 1;
    }
    35% {
      opacity: 0;
    }
  }

  @keyframes loading-bar {
    from {
      width: 0;
    }
    to {
      width: 100%;
    }
  }
}
 
```

