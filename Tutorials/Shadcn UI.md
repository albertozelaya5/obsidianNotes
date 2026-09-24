shadcn/ui **no es una librería de componentes**: es un catálogo de componentes que copiás a tu repo con un CLI. Vos sos dueño del código —lo editás in-place—, está construido sobre **Radix** (accesibilidad + comportamiento) y **Tailwind** (estilos), y usa **CVA** para variantes.

Notas basadas en **BANHCAFE Collections**: estilo `new-york`, `baseColor` neutral, CSS variables, alias `@/`, iconos `lucide`. Ver también [[React Query]], [[React hook form]] y [[BANHCAFE Collections — Guía del proyecto]].

## Índice

1. Qué es (y qué NO es)
2. Cómo está configurado: `components.json`
3. La capa de tokens en `index.css`
4. Agregar un componente
5. Anatomía de un componente generado (`button.tsx`)
6. `cva`: leer y editar variantes
7. `cn()`: el merge de clases
8. `asChild` / `Slot`: composición
9. Editar in-place, no envolver
10. Formularios: `Form*` + React Hook Form + Zod
11. `Spinner` (componente propio)
12. Toasts: `sonner`
13. Dark mode
14. Charts (Recharts)
15. Iconos: `lucide-react`
16. Actualizar componentes (`diff`)
17. Errores comunes
18. Checklist

---

## 1. Qué es (y qué NO es)

| | |
| --- | --- |
| **NO es** | un paquete npm de componentes que importás (`import { Button } from "shadcn"`) |
| **SÍ es** | un CLI que **copia el `.tsx` del componente a tu carpeta** (`src/components/ui/`) |

Consecuencias:

- **Sos dueño del código.** No hay versión que actualizar en `package.json`; si querés un cambio, editás el archivo.
- **Cero abstracción oculta.** Lo que ves en `button.tsx` es todo lo que hay.
- **Radix por debajo.** Los componentes con comportamiento (dialog, checkbox, dropdown…) instalan su `@radix-ui/react-*` correspondiente. shadcn solo les pone estilos y estructura.
- **La única dependencia "de shadcn"** en `package.json` es el CLI (`shadcn`), más `class-variance-authority`, `clsx`, `tailwind-merge` y `lucide-react`.

---

## 2. Cómo está configurado: `components.json`

```json
{
  "style": "new-york",
  "rsc": false,
  "tsx": true,
  "tailwind": {
    "config": "",
    "css": "src/index.css",
    "baseColor": "neutral",
    "cssVariables": true,
    "prefix": ""
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/components/ui",
    "lib": "@/lib",
    "hooks": "@/hooks"
  },
  "iconLibrary": "lucide"
}
```

| Campo | Qué implica en este proyecto |
| --- | --- |
| `style: "new-york"` | variante visual más compacta (sombras `shadow-xs`, tamaños ajustados). La otra opción era `default`. |
| `rsc: false` | no es Next con Server Components → los archivos llevan `"use client"` inofensivo o nada. |
| `tailwind.config: ""` | Tailwind v4: la config vive en el CSS (`@theme`), no en `tailwind.config.js`. |
| `tailwind.css: "src/index.css"` | ahí van los tokens que el CLI escribe/espera. |
| `baseColor: "neutral"` | la paleta gris de base de los componentes. |
| `cssVariables: true` | los colores son `var(--primary)` etc., no clases hardcodeadas → el theming se hace cambiando variables. |
| `aliases` | por eso los imports generados usan `@/components/ui/...` y `@/lib/utils`. |
| `iconLibrary: "lucide"` | los componentes importan de `lucide-react`. |

---

## 3. La capa de tokens en `index.css`

Orden del archivo (importa entenderlo, porque hay **dos** sistemas de tokens conviviendo):

```css
@import "tailwindcss";
@import "tw-animate-css";
@import "shadcn/tailwind.css";

@custom-variant dark (&:is(.dark *));

/* 1) Capa shadcn: mapea --color-* de Tailwind a las variables --* */
@theme inline {
  --color-background: var(--background);
  --color-primary: var(--primary);
  --color-destructive: var(--destructive);
  --color-border: var(--border);
  --color-input: var(--input);
  --color-ring: var(--ring);
  /* ...chart-1..5, sidebar-*, radius-* ... */
}

/* 2) Valores de esas variables, en oklch */
:root {
  --radius: 0.625rem;
  --primary: oklch(0.205 0 0);
  --destructive: oklch(0.577 0.245 27.325);
  /* ... */
}
.dark {
  --primary: oklch(0.985 0 0);
  /* ... */
}

/* 3) Capa design system Banhcafe: @theme { ... } — va DESPUÉS,
      así gana en los tokens que redefine (p. ej. --color-primary: #a41f35) */
```

> [!IMPORTANT] Por qué el rojo de marca "gana"
> El `@theme` de Banhcafe se declara **después** de la capa shadcn, así que `--color-primary: #a41f35` sobre-escribe al `--primary` neutro de shadcn. `primary` = acción primaria en **ambos** sistemas, así que el override es a propósito.

> [!WARNING]
> **No** sobre-escribir `--color-input` en el `@theme` de Banhcafe: rompe el borde de los `<Input>` de shadcn.

Clases que vas a ver en los componentes y de dónde salen: `bg-primary`, `text-primary-foreground`, `bg-destructive`, `border-input`, `ring-ring`, `bg-accent`, `text-muted-foreground`. Todas resuelven a `var(--*)`.

---

## 4. Agregar un componente

```bash
pnpm dlx shadcn@latest add button
pnpm dlx shadcn@latest add dialog table chart   # varios de una
```

Qué pasa:

- El `.tsx` aterriza en `src/components/ui/` **en minúsculas** (`button.tsx`, `input-otp.tsx`).
- Instala solo las deps de Radix que ese componente necesita (`@radix-ui/react-dialog`, etc.).
- Si el componente depende de otro primitivo (p. ej. `form` usa `label`), lo agrega también.

> [!NOTE] La excepción de nomenclatura
> Todo el proyecto es `PascalCase.tsx` para componentes propios, **pero** los primitivos de `src/components/ui/` quedan en minúsculas. No renombrarlos: romperían el `add` / `diff` del CLI.

Instalados hoy en el proyecto: `button`, `checkbox`, `dialog`, `form`, `input`, `input-otp`, `label`, `sonner`, y `spinner` (este último es propio, ver sección 11).

---

## 5. Anatomía de un componente generado (`button.tsx`)

```tsx
import * as React from "react";
import { cva, type VariantProps } from "class-variance-authority";
import { Slot } from "radix-ui";
import { cn } from "@/lib/utils";

const buttonVariants = cva(
  "inline-flex shrink-0 items-center justify-center gap-2 rounded-md text-sm font-medium ...", // clases base, SIEMPRE presentes
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        destructive: "bg-destructive text-white hover:bg-destructive/90 ...",
        outline: "border bg-background shadow-xs hover:bg-accent ...",
        secondary: "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        ghost: "hover:bg-accent hover:text-accent-foreground ...",
        link: "text-primary underline-offset-4 hover:underline",
      },
      size: {
        default: "h-9 px-4 py-2 has-[>svg]:px-3",
        xs: "h-6 gap-1 rounded-md px-2 text-xs ...",
        sm: "h-8 gap-1.5 rounded-md px-3 ...",
        lg: "h-10 rounded-md px-6 ...",
        icon: "size-9",
        "icon-sm": "size-8",
        "icon-lg": "size-10",
      },
    },
    defaultVariants: { variant: "default", size: "default" },
  },
);

function Button({
  className,
  variant = "default",
  size = "default",
  asChild = false,
  ...props
}: React.ComponentProps<"button"> &
  VariantProps<typeof buttonVariants> & { asChild?: boolean }) {
  const Comp = asChild ? Slot.Root : "button";

  return (
    <Comp
      data-slot="button"
      data-variant={variant}
      data-size={size}
      className={cn(buttonVariants({ variant, size, className }))}
      {...props}
    />
  );
}

export { Button, buttonVariants };
```

Piezas del patrón (se repite en casi todos):

| Pieza | Para qué |
| --- | --- |
| `cva(base, { variants, defaultVariants })` | define las clases: base + una por combinación de props. |
| `VariantProps<typeof buttonVariants>` | deriva el **tipo** de las props `variant` / `size` desde el `cva`. |
| `React.ComponentProps<"button">` | hereda todas las props nativas del `<button>` (`onClick`, `disabled`, `type`…). |
| `data-slot="button"` | gancho estable para CSS/tests que no depende de clases. |
| `cn(buttonVariants({ variant, size, className }))` | resuelve variantes + deja que `className` del consumidor gane. |
| `asChild` + `Slot.Root` | render polimórfico (sección 8). |

Uso:

```tsx
<Button>Guardar</Button>
<Button variant="outline" size="sm">Cancelar</Button>
<Button variant="destructive" disabled={mutation.isPending}>
  {mutation.isPending ? <Spinner /> : "Eliminar"}
</Button>
<Button variant="ghost" size="icon" aria-label="Cerrar"><XIcon /></Button>
```

---

## 6. `cva`: leer y editar variantes

`cva(base, config)` devuelve una función. Al llamarla con `{ variant, size }` concatena: `base + variants.variant[x] + variants.size[y]`.

### Agregar una variante de marca (in-place)

El proyecto pide ajustar el `cva` **directamente**, no crear un wrapper. Ejemplo: un botón "success":

```tsx
variants: {
  variant: {
    default: "bg-primary text-primary-foreground hover:bg-primary/90",
    // ...
    success: "bg-success-main text-white hover:bg-success-main/90", // <- nuevo, usa tokens Banhcafe
  },
}
```

`VariantProps` recoge el cambio solo: `<Button variant="success">` ya tipa.

### Reglas

- Las clases **base** van siempre; las de `variants` se suman según props.
- Prettier + `prettier-plugin-tailwindcss` **reordena** las clases dentro del `cva()` al guardar (está declarado en `tailwindFunctions`). No pelear con ese orden.
- Si una variante necesita varias clases condicionales, se puede pasar un array; lo normal es un string.

---

## 7. `cn()`: el merge de clases

```ts
// src/lib/utils.ts
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

- **`clsx`**: junta strings/objetos/arrays y descarta lo `falsy` → `cn("p-2", cond && "hidden")`.
- **`twMerge`**: resuelve **conflictos** de Tailwind quedándose con la última. `twMerge("p-2 p-4")` → `"p-4"`.

Por eso `cn(buttonVariants({ ... , className }))` deja que el consumidor **gane**: si pasás `className="rounded-none"`, sobre-escribe el `rounded-md` de la base.

```tsx
<Button className="w-full rounded-none">Ancho completo, sin borde redondo</Button>
```

shadcn espera `cn` en `@/lib/utils` (por el alias de `components.json`). No moverlo.

---

## 8. `asChild` / `Slot`: composición

`asChild` hace que el componente **no renderice su propio tag** y en su lugar fusione props/clases sobre su hijo. Sirve para "quiero un `<Link>` que se vea como Button" sin anidar `<a><button>`.

```tsx
import { Link } from "react-router";

<Button asChild>
  <Link to="/collections">Ver cobranzas</Link>
</Button>;
// renderiza UN <a> con las clases del botón
```

Internamente: `const Comp = asChild ? Slot.Root : "button"`. `Slot.Root` (de Radix) toma el único hijo y le inyecta className + props.

> [!TIP]
> Regla: **un solo** hijo con `asChild`, y ese hijo debe reenviar `className` y `ref`. Componentes de router (`Link`) y la mayoría de shadcn ya lo hacen.

---

## 9. Editar in-place, no envolver

> [!IMPORTANT] Regla del proyecto
> Los archivos de `src/components/ui/` **se editan in-place**. No se crea un wrapper solo para cambiar clases o agregar una variante — eso va en el `cva()` del propio archivo.

Cuándo **sí** creás un componente nuevo:

- **Envoltura de negocio**: compone primitivos + datos de una feature (p. ej. `DebtorTable` sobre `<Table>`). Es `PascalCase`, vive en `features/<x>/components/`, puede tener carpeta con tipos/subcomponentes.
- **Nunca** en `components/ui/`: si un componente "sabe qué es un deudor", no es un primitivo.

```
src/components/ui/table.tsx          → primitivo shadcn, agnóstico
src/features/debtors/components/DebtorTable.tsx  → usa <Table> + useDebtors()
```

---

## 10. Formularios: `Form*` + React Hook Form + Zod

`pnpm dlx shadcn@latest add form` trae wrappers sobre RHF que cablean `id`, `aria-invalid`, `aria-describedby` y el mensaje de error automáticamente.

| Componente | Rol |
| --- | --- |
| `Form` | es `FormProvider` de RHF (renombrado). Le pasás `{...form}`. |
| `FormField` | es `Controller` de RHF + contexto con el `name`. Usa `render={({ field }) => ...}`. |
| `FormItem` | contenedor (`grid gap-2`) + genera un `id` con `useId()`. |
| `FormLabel` | `<Label>` con `htmlFor` correcto y color `destructive` si hay error. |
| `FormControl` | `Slot` que inyecta `id`, `aria-invalid`, `aria-describedby` en el input hijo. |
| `FormDescription` | texto de ayuda, enlazado por `aria-describedby`. |
| `FormMessage` | muestra `fieldState.error.message`; se auto-oculta si no hay error. |

Ejemplo completo con `zodResolver`:

```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";
import {
  Form, FormField, FormItem, FormLabel, FormControl, FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { Spinner } from "@/components/ui/spinner";

const schema = z.object({
  documentId: z.string().min(1, "Requerido"),
  amount: z.coerce.number().positive("Debe ser mayor a 0"),
});
type FormValues = z.infer<typeof schema>;

function NewCollectionForm() {
  const form = useForm<FormValues>({
    resolver: zodResolver(schema),
    defaultValues: { documentId: "", amount: 0 },
  });
  const createCollection = useCreateCollection(); // hook de React Query

  const onSubmit = (values: FormValues) => {
    createCollection.mutate(values, {
      onError: (e) => {
        if (e instanceof ApiError && e.status === 409) {
          form.setError("documentId", { message: "Ya existe" }); // error de campo → inline
        } else {
          toast.error("No se pudo crear"); // global → toast
        }
      },
    });
  };

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="grid gap-4">
        <FormField
          control={form.control}
          name="documentId"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Documento</FormLabel>
              <FormControl>
                <Input placeholder="0801-1990-12345" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        <FormField
          control={form.control}
          name="amount"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Monto</FormLabel>
              <FormControl>
                <Input type="number" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        <Button type="submit" disabled={createCollection.isPending}>
          {createCollection.isPending ? <Spinner /> : "Crear"}
        </Button>
      </form>
    </Form>
  );
}
```

> [!TIP]
> Campos no triviales (OTP, `Select`, `Checkbox`) van con el `render` de `<FormField>` / `<Controller>`, **no** con `register` + `setValue` manual. Ver [[React hook form]].

---

## 11. `Spinner` (componente propio)

No lo trae shadcn: lo agregó el proyecto en `spinner.tsx`.

```tsx
import { Loader2Icon } from "lucide-react";
import { cn } from "@/lib/utils";

function Spinner({ className, ...props }: React.ComponentProps<"svg">) {
  return (
    <Loader2Icon
      role="status"
      aria-label="Cargando"
      className={cn("size-4 animate-spin", className)}
      {...props}
    />
  );
}

export { Spinner };
```

Uso principal: dentro de botones en estado `isPending` de una mutación de React Query.

```tsx
<Button disabled={mutation.isPending}>
  {mutation.isPending ? <Spinner /> : "Guardar"}
</Button>
```

---

## 12. Toasts: `sonner`

`pnpm dlx shadcn@latest add sonner` trae el wrapper `<Toaster>` de la librería `sonner`.

```tsx
// src/app/providers.tsx
import { Toaster } from "@/components/ui/sonner";

<QueryClientProvider client={queryClient}>
  {children}
  <Toaster richColors position="top-center" />
</QueryClientProvider>;
```

Se dispara desde cualquier lado (no necesita hook ni contexto propio):

```tsx
import { toast } from "sonner";

toast.success("Cobranza registrada");
toast.error("No se pudo conectar con el servidor");
toast.promise(savePromise, { loading: "Guardando…", success: "Listo", error: "Falló" });
```

En el proyecto: errores **de campo** → `form.setError` (inline); errores **globales** → `toast.error`.

---

## 13. Dark mode

El proyecto define `@custom-variant dark (&:is(.dark *))`: el modo oscuro se activa poniendo la clase `.dark` en un ancestro (típicamente `<html>`), y `.dark { --primary: ...; }` redefine los tokens.

- Las clases `dark:` en los componentes (`dark:bg-input/30`) funcionan gracias a ese `@custom-variant`.
- Para togglearlo: un helper que hace `document.documentElement.classList.toggle("dark")`, y el estado (preferencia) vive en el **store global de UI** de zustand (`stores/slices/uiSlice.ts`), no en React Query.

---

## 14. Charts (Recharts)

`pnpm dlx shadcn@latest add chart` trae `ChartContainer` + helpers sobre **Recharts**.

```tsx
import { ChartContainer, ChartTooltip, ChartTooltipContent } from "@/components/ui/chart";
import { Bar, BarChart, XAxis } from "recharts";

const config = {
  paid: { label: "Pagado", color: "var(--chart-1)" },
  pending: { label: "Pendiente", color: "var(--chart-2)" },
};

<ChartContainer config={config} className="h-64 w-full">
  <BarChart data={data}>
    <XAxis dataKey="month" />
    <ChartTooltip content={<ChartTooltipContent />} />
    <Bar dataKey="paid" fill="var(--color-paid)" radius={4} />
    <Bar dataKey="pending" fill="var(--color-pending)" radius={4} />
  </BarChart>
</ChartContainer>;
```

Los colores de serie salen de `--chart-1..5` de `index.css`. `ChartContainer` expone cada `config.key.color` como `--color-<key>`.

---

## 15. Iconos: `lucide-react`

```tsx
import { Trash2Icon, PlusIcon, SearchIcon } from "lucide-react";

<Button variant="ghost" size="icon" aria-label="Eliminar">
  <Trash2Icon />
</Button>;
```

- El `cva` de `button` ya dimensiona los SVG hijos: `[&_svg:not([class*='size-'])]:size-4` (y `size-3` en `size="xs"`). No hace falta pasar `className="size-4"` salvo que quieras otro tamaño.
- Import nombrado (tree-shakeable). El sufijo `Icon` es la convención nueva de lucide (`TrashIcon`), los alias sin sufijo siguen existiendo.
- Iconos decorativos: dejalos sin label. Icon-only interactivo: **siempre** `aria-label`.

---

## 16. Actualizar componentes (`diff`)

Como el código es tuyo, "actualizar" = comparar con la versión upstream y aplicar a mano lo que quieras.

```bash
pnpm dlx shadcn@latest diff            # lista qué primitivos cambiaron upstream
pnpm dlx shadcn@latest diff button     # muestra el diff de ese archivo
```

Después editás `button.tsx` a mano, conservando tus variantes de marca. Por eso **no se renombran** los archivos de `ui/` ni se reordenan sus exports: el `diff` los busca por nombre.

---

## 17. Errores comunes

> [!DANGER]
> - **Tratar shadcn como paquete**: `import { Button } from "shadcn"` no existe. Se importa de `@/components/ui/button`, que es un archivo tuyo.
> - **Renombrar `button.tsx` → `Button.tsx`** para "cumplir PascalCase": rompe `add` / `diff`. Los primitivos de `ui/` son la excepción documentada.
> - **Crear un wrapper `<PrimaryButton>`** solo para fijar `variant="default"` + clases: no. Se ajusta el `cva` in-place o se pasa `className`.
> - **Ordenar clases a mano** dentro del `cva`: Prettier + plugin Tailwind las reordena al guardar. Declarar `cn`/`cva` en `tailwindFunctions` de `prettier.config.js`.
> - **Sobre-escribir `--color-input`** en el `@theme` de Banhcafe: rompe el borde de `<Input>`.
> - **Olvidar `<Form {...form}>`** alrededor de los `<FormField>`: `useFormField` lanza "should be used within <FormField>".
> - **`asChild` con más de un hijo**, o con un hijo que no reenvía `className`/`ref`: Radix `Slot` falla o pierde estilos.
> - **Meter la preferencia de tema en React Query**: es estado de cliente → zustand (`uiSlice`).
> - **Iconos sin `aria-label`** en botones icon-only: quedan sin nombre accesible.

---

## 18. Checklist

Al usar / agregar un primitivo:

- [ ] ¿Existe en `src/components/ui/`? Si no: `pnpm dlx shadcn@latest add <c>`.
- [ ] Nombre en **minúsculas**, no renombrar.
- [ ] Variantes de marca → editar el `cva()` del archivo, usando tokens (`bg-success-main`, no hex).
- [ ] Combinar clases con `cn()`; dejar que el `className` del consumidor gane.
- [ ] Composición polimórfica → `asChild` con un solo hijo que reenvíe props.
- [ ] Formularios → `Form` + `FormField` + `zodResolver`; `FormMessage` para errores de campo.
- [ ] Estados de carga en botones → `disabled={isPending}` + `<Spinner />`.
- [ ] Toasts → `toast.*` de `sonner` (el `<Toaster>` ya está en `providers.tsx`).
- [ ] Componente que "sabe de negocio" → NO en `ui/`; va en `features/<x>/components/` como `PascalCase`.
- [ ] `pnpm lint` y `pnpm build` pasan; código formateado con Prettier.

---

## Referencias

- Docs oficiales: https://ui.shadcn.com/docs
- CVA: https://cva.style/docs
- Radix Primitives: https://www.radix-ui.com/primitives
- sonner: https://sonner.emilkowal.ski
- Guía del proyecto: [[BANHCAFE Collections — Guía del proyecto]] → secciones *shadcn/ui* y *Componentes reutilizables*
- Relacionado: [[React Query]], [[React hook form]]
