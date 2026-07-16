Una landing page de Banhcafe llamada "Agritech", dedicada a los agricultores

> [!IMPORTANT]
> - El proyecto esta hecho con tanstack start, shadcn, react ts, zustand y tailwind, en un futuro se va a trasladar a Astro
> - Toda la nomenclatura del nombre de los archivos y carpetas es en ingles, para los archivos es `PascalCase.tsx`, para los ts que sean tipo `camelCase.ts` 
> -  

Sus estilos visuales son Startup/Upbeat, Calm/Peaceful, y un poco de `Plain/Neutral`

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

### `Calm/Peaceful`: Healthcare, all products with focus on consumer well-being

  - For products and services that care, transmitted by calming pastel colors, soft serif headings, and matching images/illustrations

  > Ingredients:

  - Typography - Soft serif typefaces frequently used for headings, but sans-serif headings might be used too(ex, for software products)
  - Colors - Pastel/washed-out colors: light oranges, yellows, browns, greens, blues
  - Images - Images and illustrations are usual(lot ot people in there), matching with calm color palette in their photos
  - Icons - Icons are quite frequent
  - Shadows - Usually no shadows, but might be used sparingly(escasamente)
  - Border-radius - ✅ Some border-radius is usual
  - Layout - All kinds of layouts, no particular tendencies

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

Y este es el que tiene el sitio de agritech actualmente

```css
@import "tailwindcss" source(none);

@source "../src";

@import "tw-animate-css";

  

@custom-variant dark (&:is(.dark *));

  

/* =========================================================

AGRITECH BANHCAFE Design System

Personality: Corporate Modern + Calm/Peaceful accents

Primary (oficial): #A41F35 · Hover/Deep (oficial): #BA0C2F

========================================================= */

  

/*

NOTA

TODO - FALTA CAMBIAR ESCALA TIPOGRAFICA A REM

TODO - FALTA CORROBORAR SI LOS COLORES DEL MODO OSCURO ESTAN BIEN

*/

  

@theme inline {

/* Radius (soft 4–12px system) */

--radius: 0.5rem;

--radius-sm: 0.125rem; /* 2px */

--radius-md: 0.25rem; /* 4px */

--radius-lg: 0.5rem; /* 8px */

--radius-xl: 0.75rem; /* 12px */

--radius-2xl: 1rem; /* 16px */

--radius-3xl: 1.5rem; /* 24px */

  

/* Spacing scale (exact px) */

--spacing-1: 2px;

--spacing-2: 4px;

--spacing-3: 8px;

--spacing-4: 12px;

--spacing-5: 16px;

--spacing-6: 24px;

--spacing-7: 32px;

--spacing-8: 48px;

--spacing-9: 64px;

--spacing-10: 80px;

--spacing-11: 96px;

--spacing-12: 128px;

  

/* Typography */

--font-sans: "Inter", ui-sans-serif, system-ui, sans-serif;

--font-serif: "Instrument Serif", ui-serif, Georgia, serif;

  

/* Semantic color tokens */

--color-background: var(--background);

--color-foreground: var(--foreground);

--color-card: var(--card);

--color-card-foreground: var(--card-foreground);

--color-popover: var(--popover);

--color-popover-foreground: var(--popover-foreground);

--color-primary: var(--primary);

--color-primary-foreground: var(--primary-foreground);

--color-primary-hover: var(--primary-hover);

--color-primary-deep: var(--primary-deep);

--color-secondary: var(--secondary);

--color-secondary-foreground: var(--secondary-foreground);

--color-tertiary: var(--tertiary);

--color-muted: var(--muted);

--color-muted-foreground: var(--muted-foreground);

--color-accent: var(--accent);

--color-accent-foreground: var(--accent-foreground);

--color-destructive: var(--destructive);

--color-destructive-foreground: var(--destructive-foreground);

--color-border: var(--border);

--color-input: var(--input);

--color-ring: var(--ring);

--color-surface: var(--surface);

--color-surface-low: var(--surface-low);

--color-surface-high: var(--surface-high);

--color-nav: var(--nav);

--color-nav-foreground: var(--nav-foreground);

  

/* Official palette — extra tokens */

--color-gray-background: var(--gray-background);

--color-body: var(--body);

--color-light-gray: var(--light-gray);

--color-button: var(--button);

--color-accents-blue: var(--accents-blue);

--color-notification-container: var(--notification-container);

--color-overlay-light: var(--overlay-light);

--color-overlay-extra-light: var(--overlay-extra-light);

--color-submenu: var(--submenu);

--color-icon-light: var(--icon-light);

--color-icon-background: var(--icon-background);

--color-icon-background-hover: var(--icon-background-hover);

--color-icon-border: var(--icon-border);

  

/* Status colors */

--color-error-light: var(--error-light);

--color-error-main: var(--error-main);

--color-success-light: var(--success-light);

--color-success-main: var(--success-main);

--color-warning-light: var(--warning-light);

--color-warning-main: var(--warning-main);

--color-info-light: var(--info-light);

--color-info-main: var(--info-main);

  

/* Calm-layer accents */

--color-calm-sage: var(--calm-sage);

--color-calm-sage-soft: var(--calm-sage-soft);

--color-calm-sand: var(--calm-sand);

--color-calm-sand-soft: var(--calm-sand-soft);

--color-calm-sky: var(--calm-sky);

--color-calm-sky-soft: var(--calm-sky-soft);

}

  

:root {

/* Core brand (oficial) */

--primary: #a41f35;

--primary-hover: #ba0c2f;

--primary-deep: var(--primary-hover); /* alias, compatibilidad con usos previos */

--primary-foreground: #ffffff;

  

/* Surfaces (Tonal Layering) */

--background: #fbfbfb;

--foreground: #181c20;

--surface: #f3f5fa; /* = "modal" oficial, ya coincidía */

--surface-low: #f5f5f5; /* = "gray-background" oficial */

--surface-high: #eeeeee; /* = "light-gray" oficial */

  

--card: #ffffff;

--card-foreground: #181c20;

--popover: #ffffff;

--popover-foreground: #181c20;

  

--secondary: #f8f8f8;

--secondary-foreground: #181c20;

--tertiary: #343642;

--muted: #ecedf1; /* = "body" / "gray-body" oficial */

--muted-foreground: #343642; /* = "text-subtitle" oficial */

--accent: #f3f5fa;

--accent-foreground: #181c20;

  

--destructive: #ba0c2f; /* = "error-main" oficial */

--destructive-foreground: #ffffff;

  

--border: #eeeeee; /* = "light-gray" oficial */

--input: #f0f5fa; /* = "input" oficial */

--ring: #a41f35; /* = nuevo primary oficial */

  

--nav: #7a232e;

--nav-foreground: #ffffff;

  

/* Official palette — extra tokens */

--gray-background: #f5f5f5;

--body: #ecedf1;

--light-gray: #eeeeee;

--button: rgba(41, 45, 50, 0.7);

--accents-blue: #418fd8;

--notification-container: #d9ecfe;

--overlay-light: rgba(0, 0, 0, 0.5);

--overlay-extra-light: rgba(0, 0, 0, 0.03);

--submenu: rgba(243, 245, 250, 0.9);

--icon-light: #b1b9d8;

--icon-background: rgba(233, 200, 200, 0.18);

--icon-background-hover: rgba(233, 200, 200, 0.3);

--icon-border: rgba(186, 12, 47, 0.3);

  

/* Status colors (oficial) */

--error-light: #fff2f2;

--error-main: #ba0c2f;

--success-light: #deede3;

--success-main: #26b769;

--warning-light: #ffeee1;

--warning-main: #d67c3b;

--info-light: #d9ecfe;

--info-main: #418fd8;

  

/* Calm/Peaceful accents (used sparingly on human-focused sections) */

--calm-sage: #7d9b76;

--calm-sage-soft: #dce5d4;

--calm-sand: #c9a984;

--calm-sand-soft: #f0ebe0;

--calm-sky: #6ba3c8;

--calm-sky-soft: #dbeaf4;

  

/* Shadows (soft ambient) */

--shadow-soft: 0 4px 12px rgba(0, 0, 0, 0.05);

--shadow-lift: 0 8px 24px rgba(0, 0, 0, 0.08);

}

  

.dark {

--background: #0f1216;

--foreground: #eff1f6;

--surface: #171b20;

--surface-low: #1a1e23;

--surface-high: #22272e;

  

--card: #1a1e23;

--card-foreground: #eff1f6;

--popover: #1a1e23;

--popover-foreground: #eff1f6;

  

--primary: #ff5a75;

--primary-hover: #ff8095; /* inferido: hover más claro para fondo oscuro */

--primary-deep: var(--primary-hover);

--primary-foreground: #0f1216;

  

--secondary: #22272e;

--secondary-foreground: #eff1f6;

--tertiary: #c7cad6; /* inferido */

--muted: #22272e;

--muted-foreground: #c7cad6; /* inferido, alineado con tertiary */

--accent: #22272e;

--accent-foreground: #eff1f6;

  

--destructive: #ff6b6b;

--destructive-foreground: #0f1216;

  

--border: rgba(255, 255, 255, 0.1);

--input: rgba(255, 255, 255, 0.15);

--ring: #ff5a75;

  

--nav: #0f1216;

--nav-foreground: #eff1f6;

  

/* Official palette — extra tokens (inferidos para modo oscuro) */

--gray-background: #1a1e23;

--body: #1a1e23;

--light-gray: #22272e;

--button: rgba(255, 255, 255, 0.08);

--accents-blue: #6fb3e6;

--notification-container: #1e2932;

--overlay-light: rgba(0, 0, 0, 0.6);

--overlay-extra-light: rgba(255, 255, 255, 0.03);

--submenu: rgba(26, 30, 35, 0.9);

--icon-light: #4a5568;

--icon-background: rgba(255, 90, 117, 0.12);

--icon-background-hover: rgba(255, 90, 117, 0.2);

--icon-border: rgba(255, 90, 117, 0.3);

  

/* Status colors (inferidos para modo oscuro) */

--error-light: #3a1414;

--error-main: #ff5a75;

--success-light: #16311f;

--success-main: #4ade80;

--warning-light: #3a2413;

--warning-main: #f0a35f;

--info-light: #1e2932;

--info-main: #6fb3e6;

  

--calm-sage: #a3c19b;

--calm-sage-soft: #2a3428;

--calm-sand: #d9bd97;

--calm-sand-soft: #2f2a22;

--calm-sky: #8cbfe0;

--calm-sky-soft: #1e2932;

  

--shadow-soft: 0 4px 12px rgba(0, 0, 0, 0.4);

--shadow-lift: 0 8px 24px rgba(0, 0, 0, 0.5);

}

  

/* =========================================================

Typography utilities — exact px scale

10 / 12 / 14 / 16 / 18 / 20 / 24 / 30 / 36 / 44 / 52 / 62 / 74 / 86 / 98

========================================================= */

@utility text-10 {

font-size: 10px;

line-height: 1.5;

}

@utility text-12 {

font-size: 12px;

line-height: 16px;

}

@utility text-14 {

font-size: 14px;

line-height: 20px;

}

@utility text-16 {

font-size: 16px;

line-height: 24px;

}

@utility text-18 {

font-size: 18px;

line-height: 28px;

}

@utility text-20 {

font-size: 20px;

line-height: 28px;

}

@utility text-24 {

font-size: 24px;

line-height: 32px;

}

@utility text-30 {

font-size: 30px;

line-height: 38px;

letter-spacing: -0.02em;

}

@utility text-36 {

font-size: 36px;

line-height: 44px;

letter-spacing: -0.02em;

}

@utility text-44 {

font-size: 44px;

line-height: 52px;

letter-spacing: -0.02em;

}

@utility text-52 {

font-size: 52px;

line-height: 60px;

letter-spacing: -0.02em;

}

@utility text-62 {

font-size: 62px;

line-height: 68px;

letter-spacing: -0.02em;

}

@utility text-74 {

font-size: 74px;

line-height: 78px;

letter-spacing: -0.02em;

}

@utility text-86 {

font-size: 86px;

line-height: 90px;

letter-spacing: -0.02em;

}

@utility text-98 {

font-size: 98px;

line-height: 100px;

letter-spacing: -0.02em;

}

  

/* Eyebrow label */

@utility eyebrow {

font-size: 12px;

font-weight: 600;

letter-spacing: 0.16em;

text-transform: uppercase;

line-height: 16px;

}

  

/* Calm serif — used only in human/impact-focused sections */

@utility font-serif-calm {

font-family: var(--font-serif);

font-weight: 400;

letter-spacing: -0.01em;

}

  

@utility shadow-soft {

box-shadow: var(--shadow-soft);

}

@utility shadow-lift {

box-shadow: var(--shadow-lift);

}

  

@layer base {

* {

border-color: var(--color-border);

}

html {

font-family: var(--font-sans);

scroll-behavior: smooth;

}

body {

background-color: var(--color-background);

color: var(--color-foreground);

font-family: var(--font-sans);

font-size: 16px;

line-height: 1.5;

-webkit-font-smoothing: antialiased;

-moz-osx-font-smoothing: grayscale;

}

h1,

h2,

h3,

h4,

h5,

h6 {

letter-spacing: -0.02em;

font-weight: 700;

}

}
```