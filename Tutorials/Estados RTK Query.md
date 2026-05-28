Para crear un estado, es parecido a un `reducer`, donde comienza con un estado inicial

```js
export const initialState = {
  balance: 0,
  loan: 0,
  loanPurpose: "",
  isLoading: false,
};
```

Seguimos con el pedazo de estado que queremos usar, en este caso un estado que tenga que ver con cuentas, para ello usamos `createSlice()`

```js
const accountSlice = createSlice({
  name: "account",
  initialState,
  reducers: {
    deposit(state, action) {
      state.balance += action.payload;
      state.isLoading = false;
    },
    requestLoan: {
      prepare(amount, purpose) {
        return { payload: { amount, purpose } };
      },

      reducer(state, action) {
        if (state.loan > 0) return;

        state.loan = action.payload.amount;
        state.loanPurpose = action.payload.purpose;
        state.balance += action.payload.amount;
      },
    },
  }
})

export const { withdraw, requestLoan, payLoan } = accountSlice.actions;
//* AQUI VA EL MIDDLEWARE
export default accountSlice.reducer;
```

Donde a cada slice le asignamos un `name`, le asociamos un estado inicial, y los `reducers` en este caso serian las action functions, donde cada funcion alterla el estado inicial

Y el  `action` es el argumento que le enviamos al ejecutar esa funcion, y exportamos esas funciones para usarlas en nuestros componentes

> [!NOTE] Optional

Luego, el prepare dentro del action function, es cuando tenemos dos argumentos que queremos unir en un mismo `payload`, por eso retornamos el objeto con una prop `payload`, para terminar con el `reducer` final.

## Action functions

Son funciones automatizadas para actualizar el estado, en lugar de poner `{ type: ACCOUNT_DEPOSIT, payload: amount }` directamente, ponemos poner `dispatch(deposit({payload:400}))`

Así no se nos olvida que hace cada función
### Middleware

Es un lugar que esta en la mitad, entre las action functions y la modificación del estado, sirve como opción para traer valores de APIs

```js
export function deposit(amount, currency) {
  if (currency === "USD") return { type: ACCOUNT_DEPOSIT, payload: amount };

  return async function (dispatch, getState) {
    dispatch({ type: "account/convertingCurrency" });
    //API call

    const res = await fetch(`https://api.frankfurter.dev/v1/latest?base=${currency}&amount=${amount}&symbols=USD`);
    const data = await res.json();
    const converted = data.rates.USD;

    dispatch({ type: ACCOUNT_DEPOSIT, payload: converted });
  };
}

//* aqui va el eport default slice.reducer
```

En la action function si se pueden enviar varios argumentos, podemos retornar otro objeto haciendo referencia a una `action`

El `middleware` se diferencia al ser una función que retorna una función, y estas se pueden usar mediante librerías, o nosotros podemos hacer las nuestras, como en este caso

### Store global

Este slice state lo pasamos a los reducers generales, usando `configureStore` para crear el store

```js
import accountReducer from "./features/accounts/accountSlice";

const store = configureStore({
  reducer: {
    account: accountReducer,
  },
});

export default store;
```

Y al final, como padre del App, usamos el `Provider` pasandole el `store` o sea el estado general

```js
import store from "./store";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));

root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);

```

