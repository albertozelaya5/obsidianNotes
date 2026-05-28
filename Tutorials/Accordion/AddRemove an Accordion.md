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
