```tsx
import { FormProvider, SubmitHandler, useFieldArray, useForm, useFormContext, UseFormReturn } from "react-hook-form";
import styled from "styled-components";

import { SelectBox, TextBox } from "@/components/dev-extreme";
import { LabelText } from "@/styles/FormModals";

import { ActionButtons } from "@/pages/technology/requirements/requirements-requests/components/sections/evaluation/Evaluation";
import {
  EvaluationAnswer,
  Structure,
} from "@/pages/technology/requirements/requirements-requests/components/sections/evaluation/interfaces/intEvaluations";
import {
  answerValidation,
  selectedValue,
  structureObj,
} from "@/pages/technology/requirements/requirements-requests/components/sections/evaluation/lib/Evalhelpers";
import { evaulationValidation } from "@/pages/technology/requirements/requirements-requests/components/sections/evaluation/validations";
import { DocumentationInterface } from "@/pages/technology/requirements/requirements-requests/interfaces/requirementRequest";
import { yupResolver } from "@hookform/resolvers/yup";

export type StructureArr = {
  question: number;
  perspective: string;
  optionAnswer: string;
  objectives: string;
  regulatoryBody: string;
  resolution: string;
  maishot: string;
  processImprovements: string;
  dateEvaluated: string;
};

type TypeEvaluationInputsList = {
  structures?: Structure[];
  structureFormArr?: StructureArr[];
  onSubmit: SubmitHandler<DocumentationInterface>;
};

export const EvaluationInputsList = function ({ structures, onSubmit }: TypeEvaluationInputsList) {
  const structureFormArr = structures?.map((structure) => {
    return { ...structureObj, question: structure?.id };
  });

  const formValues = useForm<DocumentationInterface>({
    defaultValues: {
      structure: structureFormArr,
    },
    resolver: yupResolver(evaulationValidation),
  });

  const {
    handleSubmit,
    control,
    watch,
    formState: { errors },
  } = formValues;

  const { fields, append, remove } = useFieldArray({
    control,
    name: "structure",
  });

  return (
    <>
      <ActionButtons />

      {fields.map((structure, index) => {
        return (
          <FormProvider {...formValues} key={structure.id}>
            <form onSubmit={handleSubmit(onSubmit)}>
              <EvaluationInputs
                key={structure.id}
                structure={structures[index]}
                index={index}
                structureFormArr={structureFormArr}
                onSubmit={onSubmit}
              />
            </form>
          </FormProvider>
        );
      })}
    </>
  );
};

type TypeEvaluationInputs = {
  structure?: Structure;
  index: number;
  onSubmit: SubmitHandler<DocumentationInterface>;
  formValues: UseFormReturn<DocumentationInterface, any, undefined>;
};

function EvaluationInputs({ structure, index }: TypeEvaluationInputs) {
  const {
    watch,
    control,
    formState: { errors },
  } = useFormContext();

  //* INPUTS
  const perspectiveInput = watch("structure")?.[index]?.perspectives;
  const optionAnswerInput = watch("structure")?.[index]?.optionAnswer;

  //* DATA
  const { options, evaluationPerspectives, evaluationAnswers } = structure || {};
  const objectives = evaluationPerspectives?.find((perspective) => perspective.id === perspectiveInput)?.objectives;
  const optionAnswerDescription = options?.find((option) => option?.id === optionAnswerInput)?.description;

  return (
    <>
      <LabelChildren>{structure?.typeLocal}</LabelChildren>
      <LabelChildren>{structure?.description}</LabelChildren>
      <SelectBox
        control={control}
        errors={errors}
        name={`structure.${index}.optionAnswer`}
        label="Respuesta"
        data={options}
        displayExpr="description"
        valueExpr="id"
        defaultValue={selectedValue(options)?.id}
        styleContainer={{ paddingBottom: "1.5rem" }}
      />
      {evaluationAnswers && !answerValidation(optionAnswerDescription) && (
        <EvaluationAnswers index={index} evaluationAnswers={evaluationAnswers} />
      )}
      {evaluationPerspectives && !answerValidation(optionAnswerDescription) && (
        <>
          <SelectBox
            control={control}
            errors={errors}
            name={`structure.${index}.perspective`}
            label="Perspectiva"
            data={evaluationPerspectives}
            defaultValue={selectedValue(evaluationPerspectives)}
            displayExpr="description"
            valueExpr="id"
            styleContainer={{ paddingBottom: "1.5rem" }}
          />
          <SelectBox
            control={control}
            errors={errors}
            name={`structure.${index}.objectives`}
            label="Objetivo"
            data={objectives}
            defaultValue={selectedValue(objectives)}
            displayExpr="description"
            valueExpr="id"
            styleContainer={{ marginInlineStart: "1.5rem" }}
          />
        </>
      )}
    </>
  );
}

type TypePerspectives = {
  index: number;
  evaluationAnswers?: EvaluationAnswer;
};
function EvaluationAnswers({ evaluationAnswers, index }: TypePerspectives) {
  const {
    control,
    formState: { errors },
  } = useFormContext();

  return (
    <EvalAnswersContainer>
      <AnsSingContainer>
        <TextBox
          //
          control={control}
          name={`structure.${index}.regulatoryBody`}
          label="Organismo regulador"
          errors={errors}
          defaultValue={evaluationAnswers?.regulatoryBody}
        />
      </AnsSingContainer>

      <AnsSingContainer>
        <TextBox
          //
          control={control}
          name={`structure.${index}.resolution`}
          label="Resolución"
          errors={errors}
          defaultValue={evaluationAnswers?.resolution}
        />
      </AnsSingContainer>

      <AnsSingContainer>
        <TextBox
          //
          control={control}
          name={`structure.${index}.maishot`}
          label="Circular"
          errors={errors}
          defaultValue={evaluationAnswers?.maishot}
        />
      </AnsSingContainer>

      <AnsSingContainer>
        <TextBox
          //
          control={control}
          name={`structure.${index}.processImprovements`}
          label="Mejoras de procesos"
          errors={errors}
          defaultValue={evaluationAnswers?.processImprovements}
        />
      </AnsSingContainer>
    </EvalAnswersContainer>
  );
}

const LabelChildren = styled(LabelText)<{ paddingStart?: string }>`
  padding-inline-start: ${(props) => props.paddingStart || "0"};
  font-weight: 600;
  opacity: 100%;
  color: ${(props) => props.theme.bgcBtnSecondaryHover};
`;

const AnsSingContainer = styled.div`
  flex: 0 0 calc(50% - 6px);
`;

const EvalAnswersContainer = styled.div`
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  padding-inline-start: 1.5rem;
`;

```