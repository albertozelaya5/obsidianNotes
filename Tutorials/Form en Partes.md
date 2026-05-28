```tsx
 const formValues = useForm<ProcurePaymentFormValues>({
    defaultValues: formDefaultValues,
    resolver: yupResolver(procPayValidationScheme),
    mode: "onChange",
  });
  
const {
    control,
    setValue,
    handleSubmit,
    reset,
    formState: { errors },
  } = formValues;
  
  return (
  <FormProvider {...formValues}>
            <form autoComplete="off" style={modalFormStyle}>
              <AddEditContext.Provider
                value={{
                  readOnly,
                  readOnlyDist,
                  readOnlyTax,
                  defaultValues,
                  requested,
                  toAuthorize,
                  dataCatalog: dataSecondPage,
                  onAddTax: () => appendTax(taxDocValues),
                  onRemoveTax: () => removeTax(-1),
                  onAddDist: () => appendDist(distPaymentValues),
                  onRemoveDist: () => removeDist(-1),
                  fieldsTax,
                  fieldDist,
                  edit,
                  voucherSequenceNumber:
                    voucherData?.data?.voucherSequenceNumber ?? taxDocument?.[0]?.currentValueRange,
                }}
              >
                {activeTab === 1 && <ModalGeneralData />}
                {activeTab === 2 && <ModalTaxDocument />}
                {activeTab === 3 && <ModalDistPayment />}
                {activeTab === 5 && <ModalDocQuestions />}
                {activeTab === 6 && <ModalUpDocs />}
              </AddEditContext.Provider>
            </form>
          </FormProvider>
  )
```
