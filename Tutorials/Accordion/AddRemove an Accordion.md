
## Array Dinámico

### Padre
```tsx
  const [taxesList, setTaxesList] = useState<Record<"id", string>[]>([]);
  
   useEffect(function () {
    setValue(`distributionOfPayments.${index}.taxes`, taxesList);
  }, []);
```

### Hijo
```tsx
  useEffect(
    function () {
      setTaxesList(fields);
    },
    [fields.length],
  );
```

## Array Estático
### Padre

```tsx
const ref = useRef(null);

 useEffect(
    function () {
       setValue("nombreDelArray", ref.current);
    },
    [],
  );
```
### Hijo

```tsx
  const {
    fields: fieldInstAgent,
  } = useFieldArray({
    name: "intermediaryAgents",
  });

  useEffect(function () {
    interAgentsRef.current = fieldInstAgent
  }, [fieldInstAgent]);
```

---

> [!warning] Revisión del método
> Funciona, pero es frágil. Problemas concretos:
>
> 1. **`useEffect` del padre con deps `[]`** (los dos casos). Sólo corre una vez
>    al montar → hace `setValue` / lee el ref con el array **vacío** y nunca
>    vuelve a sincronizar. En "Array Dinámico" debería ser `[taxesList]`; en el
>    ref, el `setValue` de mount lee `ref.current` cuando todavía está en `null`.
> 2. **`[fields.length]`** sólo detecta alta/baja. Si editás un item en su lugar
>    o reordenás (misma longitud), no sincroniza.
> 3. **Duplicar el estado.** `useFieldArray` ya escribe en el form state de RHF.
>    Espejarlo a `useState` + `setValue` sobre la misma ruta hace que peleen
>    entre sí.
>
> **Mejor:**
> - No espejar nada. Pasar el mismo `control` al hijo y que su `useFieldArray`
>   opere directo sobre la ruta anidada (`products.${i}.taxes`). Ver
>   `AccountBeneficiaries.tsx`.
> - Si hay que leer el array desde afuera, usar `watch("ruta")` / `getValues()`
>   en el submit, no un mirror en `useState`.
> - Si de verdad necesitás el `useEffect` de sync, poné la dependencia real en
>   el array de deps y sincronizá sobre `fields` (no `fields.length`).
