En react 19 se puede pasar como una prop normal

```jsx
// Componente Hijo
function InputHijo({ label, ref }) { // <-- Se recibe directamente en las props
  return (
    <label>
      {label}
      <input ref={ref} type="text" />
    </label>
  );
}

// Componente Padre
function Padre() {
  const inputRef = useRef(null);

  const enfocarInput = () => {
    inputRef.current?.focus(); // Controlas el input del hijo desde aquí
  };

  return (
    <>
      <InputHijo label="Nombre:" ref={inputRef} />
      <button onClick={enfocarInput}>Enfocar entrada</button>
    </>
  );
}
```

En react 18 no, porque React trata a `ref` como una propiedad reservada (palabra clave)


```jsx
import { forwardRef } from 'react';

// El componente hijo se envuelve en forwardRef para capturar la referencia separada de las props
const InputHijo = forwardRef((props, ref) => {
  return (
    <label>
      {props.label}
      <input ref={ref} type="text" />
    </label>
  );
});
```

