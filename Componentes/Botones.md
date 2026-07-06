```tsx
import { ButtonAdd } from "@/styles/Buttons";
import { IconButton } from "@/components/Tables/IconButton";

export type TypeAddRemBtn = {
  onClickButton?: () => void;
  onDisableCon?: boolean;
  style?: React.CSSProperties;
  iconName: string;
};

export function AddRemoveButton({ onClickButton, onDisableCon, style, iconName }: TypeAddRemBtn) {
  return (
    <ButtonAdd
      //*
      marginRight="0"
      padding="8px"
      onClick={(e) => {
        e.preventDefault();
        onClickButton?.();
      }}
      disabled={onDisableCon}
      style={style}
    >
      <IconButton name={iconName} />
    </ButtonAdd>
  );
}
```

