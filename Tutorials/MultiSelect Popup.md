```tsx
import { Properties } from "devextreme/ui/popup";

export const dropDownOptions: Properties = {
  height: "100%",
  position: {
    // collision: "flip",
    my: "top",
    at: "top",
    offset: "0 25",
    // offset: "2 0", // separa el popup del input
  },
};

return (
       <DropDownBoxGrid
            label="Involucrados"
            control={control}
            errors={errors}
            name="requirementResponsables"
            valueExpr="id"
            displayExpr="fullName"
            data={customer?.data || []}
            columns={columnResponsable}
            dropDownOptions={dropDownOptions}
            multiple={true}
          />
)
```
