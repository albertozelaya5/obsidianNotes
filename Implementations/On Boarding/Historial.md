`validationApi.ts`

```ts
const EQUIFAX_SEARCH_URL = "/backOffice/equifax/search";

interface IBlackListParams {
  IdClient: string;
}

export interface IEquifaxSearchBody {
  documentType: number;
  documentNumber: string;
  personType: number;
  consent: boolean;
}

/*DENTRO DE EL CREADOR DE ENDPOINTS validationsApi*/

searchEquifax: builder.mutation<ApiResponse<unknown>, IEquifaxSearchBody>({
  query: (body) => {
	return {
	  url: EQUIFAX_SEARCH_URL,
	  method: "POST",
	  body,
	};
  },
  invalidatesTags: ["ClientCreation"],
}),
```

`Validations.tsx`

```tsx
const [triggerEquifax, equifax] = useSearchEquifaxMutation();

const runEquifax = useCallback(() => {

if (documentNumber) triggerEquifax({ documentType: 1, documentNumber, personType: 1, consent });

}, [documentNumber, consent, triggerEquifax]);

  // Se dispara al entrar al paso, salvo que ambas ya hayan pasado en una visita previa.
  useEffect(() => {
    if (alreadyPassed) return;
    runBlackList();
    runEquifax();
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);
  
const allPassed = blackListState === "success" && equifaxState === "success";
const anyError = blackListState === "error" || equifaxState === "error";
const anyLoading = blackListState === "loading" || equifaxState === "loading";

  // Cachea el OK para no re-disparar si el usuario regresa a este paso.
useEffect(() => {
if (allPassed && !alreadyPassed) {
  dispatch(setOnBoardingData({ validationsResult: { blackList: true, equifax: true } }));
}
// eslint-disable-next-line react-hooks/exhaustive-deps
}, [allPassed]);

const retry = () => {
if (blackListState === "error") runBlackList();
if (equifaxState === "error") runEquifax();
};

const CARDS: { label: string; state: CheckState }[] = [
{ label: "Identidad RNP", state: "success" },
{ label: "Lista Negra", state: blackListState },
{ label: "Equifax", state: equifaxState },
];
```

