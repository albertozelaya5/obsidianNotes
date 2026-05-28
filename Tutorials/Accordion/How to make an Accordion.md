```jsx
  const inputsDataSource = useMemo(
    function () {
      return fieldDist?.map((input, index) => {
        return { ...input, index, title: `Pago ${index + 1}` };
      });
    },
    [fieldDist]
    
    return <Accordion dataSource={inputsDataSource ?? []} displayData={DistPayment} multiple={false} />
  );
```

- `displayData`=> el componente a mostrar