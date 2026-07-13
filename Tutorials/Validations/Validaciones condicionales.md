Podemos hacerlo desde el objeto `context`, las propiedades que queremos mandar, en este caso `requiresCurrencyExchange`

```tsx
const {
control,
handleSubmit,
setValue,
watch,
formState: { errors },
} = useForm<CommissionType>({
resolver: yupResolver(addCommDetailValidation, {
context: { option, requiresCurrencyExchange: info?.requiresCurrencyExchange },
}),
});
```

Y en `addCommDetailValidation`

```ts
export const addCommDetailValidation: yup.AnyObjectSchema = yup.object().shape({
  job: yup.number().when("$requiresCurrencyExchange", {
    is: true,
    then: (schema) => schema.required("El rubro es requerido"),
    otherwise: (schema) => schema.optional(),
  }),
});
```
