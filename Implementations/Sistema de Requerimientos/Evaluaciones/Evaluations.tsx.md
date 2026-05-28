```tsx
  const onSubmit = (data: DataFormPre) => {
    const formFormatted = data?.structure.map(function (field, index) {
      const entries = Object.entries(field).filter(([_, value]) => {
        return value;
      });

      const entryToObject = entries.reduce((acc, [key, val]) => {
        acc[key] = val;
        return acc;
      }, {} as Record<string, any>);

      return {
        // ...entryToObject,
        ...data.structure[index],
        question: structures?.[index]?.question,
        ...(field?.dateEvaluated && { dateEvaluated: field?.dateEvaluated?.toISOString().split("T")[0] }),
      };
    });

    const includesSelected = JSON.stringify(structuresGet)?.includes("selected");

    console.log(formFormatted);

    const evaluationBody = {
      evaluationStructure: {
        requirementId: requirementID,
        structure: JSON.stringify(formFormatted),
        // structure: formFormatted,
      },
    };

    if (includesSelected) {
      updateEvaluation(evaluationBody);
    } else {
      createEvaluation(evaluationBody);
    }
  };
```