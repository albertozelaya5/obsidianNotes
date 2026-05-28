
>[!IMPORTANT] DataSource

> - Fields: CAMPOS, POSIBLES FILAS O COLUMNAS
> - Por cada campo, este pertenece a una propiedad del array `store` => Region - "Asia"

```ts
const dataSource = new PivotGridDataSource({
  fields: [
    {
      caption: "Region",
      width: 120,
      dataField: "region",
      area: "row",
      sortBySummaryField: "Total",
    },
    {
      caption: "Total",
      dataField: "amount",
      dataType: "number",
      summaryType: "sum",
      format: "currency",
      area: "data",
    },
  ],
  store: sales,
});

 const sales: Sale[] = [
 {
    id: 10259,
    region: "Asia",
    country: "Japan",
    city: "Tokyo",
    amount: 8400,
    date: new Date("2013-01-05"),
  },
  {
    id: 10249,
    region: "North America",
    country: "United States of America",
    city: "Los Angeles",
    amount: 850,
    date: new Date("2013-01-13"),
  }],
```

> - En el `PivotGrid`, en su referencia, se vincula al grafico
> - `customizeTooltip` Es para customizar los valores de `amount` a la izquierda del grafico

```tsx
const customizeTooltip = (args: ChartTypes.CommonPointInfo) => {
  const valueText = currencyFormatter.format(args.originalValue as number);
  return {
    html: `${args.seriesName} | Total<div class="currency">${valueText}</div>`,
  };
};

const App = () => {
 return (
    <>
      <Chart ref={chartRef}>
        <Size height={200} />
        <Tooltip enabled={true} customizeTooltip={customizeTooltip} />
        <CommonSeriesSettings type="bar" />
        <AdaptiveLayout width={450} />
      </Chart> 
      
       <PivotGrid
        id="pivotgrid"
        dataSource={dataSource}
        allowSortingBySummary={true}
        allowFiltering={true}
        showBorders={true}
        showColumnTotals={false}
        showColumnGrandTotals={false}
        showRowTotals={false}
        showRowGrandTotals={false}
        ref={pivotGridRef}
      >
        <FieldChooser enabled={true} height={400} />
      </PivotGrid>
    </>
    )
}
```

Que se puede ver por año la cantidad de denuncias, y dentro de ese año que estén las fechas
> - Un campo year, y otro campo month
> - Podría hacer el componente `PivotGrid` solamente, y luego al quererlos relacionar poner el Chart aparte y extraer el ref como prop