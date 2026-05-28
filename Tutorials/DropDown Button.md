```tsx
  const onClickButton = function (data) {
    dispatcher({
      type: "SET_VISIBLE_ID",
      payload: visibleId.id ? initialState : { id: data.rowIndex + 1, isVisible: true },
    });
  };
  
  return (
   <Column
          caption="Acciones"
          width="100"
          showInColumnChooser={false}
          cellRender={(data) => {
            const index = data?.rowIndex + 1;
            return (
		      <ButtonDropDown
		        isVisible={visibleId.isVisible && index === visibleId.id}
		        stylesContainer={{ gap: "0.8rem", flexDirection: "column" }}
		        onClickButton={() => onClickButton(data)}>		
				  <ButtonActionsEdit
					className="warning"
					title="Ver reportes">				
					<AiOutlineFileText />
					<p>Ver detalles</p>
				  </ButtonActionsEdit>
				  
				  {children}
		      <ButtonDropDown/>
		      );
	  />
  )

   
```