## Objetivo

Aumentar el salario lo más rápido posible aprendiendo **React Native + Expo**.

- Es el mismo React y TypeScript que ya uso: no hay lenguaje nuevo.
- En el trabajo hay **un solo dev móvil**. Si puedo cubrir móvil, tengo argumentos para negociar un aumento ahí mismo.
- Backend (Node + PostgreSQL) queda para después: tiene más conceptos nuevos (SQL, auth, deploy), así que tarda más.

> [!TIP]
> Si ya pedí aumento y no hay presupuesto, lo más rápido es aplicar a otras empresas con **React + TS + React Native** en el CV.

## Lo nuevo frente a React web

| Web | React Native |
| --- | --- |
| `div`, `span`, `p` | `View`, `Text` |
| CSS / Tailwind | `StyleSheet` (o NativeWind) |
| React Router | Expo Router (rutas por archivos) |
| Deploy web | Publicar en App Store / Play Store (EAS) |

Lo que **no cambia**: hooks, TypeScript, zustand, TanStack Query, React Hook Form, Zod.

## ¿React Native o Expo primero?

**Directo a Expo.** La propia documentación de React Native recomienda empezar con un framework, y ese framework es Expo. La app de la empresa usa Expo, así que todo lo que aprenda se aplica directo ahí.

## Orden de estudio

Por ahora: **los videos que me pasó mi coworker, en su orden, + la documentación**. El curso de Udemy queda para después (ver abajo).

### 1. [ ] React Native for Dummies 2026 – Full Beginner Crash Course
https://www.youtube.com/watch?v=BUXnASp_WyQ

Primer contacto con todo. Mientras lo veo, tener abierta la sección **The Basics** de https://reactnative.dev/docs/getting-started para repasar cada tema:
- [ ] Core Components and Native Components (el equivalente de `div` y `span`)
- [ ] Style
- [ ] Layout with Flexbox (en móvil `flexDirection` es `column` por defecto, no `row`)
- [ ] Using a ScrollView
- [ ] Using List Views (`FlatList` en vez de `.map()` para listas largas)
- [ ] Platform Specific Code (diferencias entre iOS y Android)

### 2. [ ] Tutorial oficial de Expo (1–2 días)
https://docs.expo.dev/tutorial/introduction/
- Lo hago después del crash course para afianzar lo visto con la versión actual del SDK.
- Escribir el código yo mismo, sin copiar y pegar.
- Probarlo en el teléfono con **Expo Go** (escaneando el QR), no solo en el simulador.
- Cubre Expo Router, imágenes, modales, gestos y diferencias entre iOS y Android.

### 3. [ ] Correr y explorar la app de la empresa (1–2 días)
Clonar el repo y correrlo en el teléfono. Revisar:
- [ ] `app.json` / `app.config.ts`: versión del SDK y plugins
- [ ] Carpeta `app/`: cómo están organizadas las rutas (Expo Router)
- [ ] `package.json`: librerías de UI, estado, formularios y API
- [ ] `eas.json`: cómo hacen los builds

Anotar todo lo que no entienda. **Esa lista me dice en qué fijarme durante el bootcamp.**

### 4. [ ] FREE React Native Bootcamp: 13 Projects | Part 1
https://www.youtube.com/watch?v=sjnbszkMjeU
- Hacer los proyectos, no solo verlos.
- Ver a 1.25x–1.5x las partes que ya entiendo.
- Si algo no funciona por versión, **manda la documentación** de Expo.

### 5. [ ] FREE React Native Bootcamp: 13 Projects | Part 2
https://www.youtube.com/watch?v=yKjku_eKtvY
- Igual que la parte 1.
- Desde aquí, empezar a pedir **tareas reales** aunque no haya terminado.

### 6. [ ] Tareas reales en la app de la empresa
Tareas chicas: una pantalla nueva, un formulario o un bug de UI.

### 7. [ ] Entregar algo en producción
- [ ] Aprender el flujo de **EAS Build** y **EAS Update** que usa el equipo.
- [ ] Entregar algo que llegue a los usuarios.

### 8. [ ] Negociar
"Ahora cubro web y móvil. Si el otro dev falta, la app no se detiene."

## Plan de 4–6 semanas

| Semana | Qué hacer |
| --- | --- |
| 1 | Crash course + The Basics, tutorial de Expo, correr la app de la empresa. |
| 2–3 | Bootcamp parte 1 y parte 2. Empezar a pedir tareas reales. |
| 3–4 | Tareas reales en la app de la empresa. |
| 5–6 | EAS, entregar algo en producción y negociar. |

## Opcional: cursos de Udemy (después)

Solo si después de los videos me quedan huecos, o para profundizar:
1. [ ] **React Native Expo: Aplicaciones nativas para iOS y Android**: https://www.udemy.com/course/react-native-expo/
	- Hacerlo **después del bootcamp**, solo las secciones de lo que me falte (según la app de la empresa).
	- Saltar las secciones de fundamentos de React/TS.
2. [ ] **React Native - The Practical Guide** (solo de consulta, no hacerlo completo): https://www.udemy.com/course/react-native-the-practical-guide/

## Documentación de Expo para consultar cuando haga falta

- **Expo Router**: rutas, layouts, tabs y stacks. https://docs.expo.dev/router/introduction/
- **Development builds**: cuando Expo Go no alcanza porque la app usa módulos nativos. https://docs.expo.dev/develop/development-builds/introduction/
- **EAS Build / EAS Update**: builds y actualizaciones por OTA. https://docs.expo.dev/build/introduction/ · https://docs.expo.dev/eas-update/introduction/

## Videos o documentación

- **Los videos son la guía y la documentación es la referencia.**
- Expo saca un SDK nuevo varias veces al año y los videos se desactualizan rápido. Si algo no funciona, **manda la documentación**.
- Documentación: https://docs.expo.dev · https://reactnative.dev

## Resto de cursos

**Saltar por ahora** (ya lo sé o no me acerca al aumento):
- The Complete JavaScript Course
- The Ultimate React Course
- Build Responsive Real-World Websites with HTML and CSS
- Understanding TypeScript
- React & TypeScript - The Practical Guide
- Astro

**Después de móvil:**
- [ ] React Testing Library with Jest / Vitest (corto, sirve en web y móvil, suma en entrevistas)

**Más adelante (perfil senior):**
- [ ] Clean Code
- [ ] Software Architecture & Design of Modern Large Scale Systems
- [ ] Ultimate AWS Certified Solutions Architect Associate

## Siguiente paso a mediano plazo

- **Backend con Node + NestJS + PostgreSQL**, para tener un perfil full-stack.
- **Inglés**, para trabajo remoto con empresas de afuera, que suele ser la mayor subida de salario.
