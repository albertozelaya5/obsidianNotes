
> [!IMPORTANT]
> Hay que modificarlo para que acepte titulo y descripcion
> Quitar CardBoolean y cambiarlo por children

```tsx
type TypeColModal = {
  title: string;
  children?: React.ReactNode;
};

export function ColumnModal({ title, children }: TypeColModal) {
  return (
    <Column>
      <LabelRequest>
        <p>{title}</p>
        <Value>{children}</Value>
      </LabelRequest>
    </Column>
  );
}
```

import { Column, LabelRequest, Value } from "@/styles/DataTables";