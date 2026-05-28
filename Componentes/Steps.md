```tsx
export type TypeStepsCont = {
  pageValue: number;
  onSetPageValue: (arg1: number) => void;
  stepsArr: number[];
};

export const PageContainer = styled.span`
  display: flex;
  gap: 0.5rem;
`;

export const DefaultButtonPagination = styled.div<{ isActive?: boolean }>`
  width: 1rem;
  height: 1rem;
  border-radius: 50%;
  font-size: 1.3rem;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: ${({ isActive, theme }) => (isActive ? theme.bgcHoverPagination : "")};
  color: ${(props) => props.theme.textPagination};
  padding: 1.5rem;
  color: ${(props) => props.theme.text};
  cursor: pointer;
`;

export const StepsContainer = ({ pageValue, onSetPageValue, stepsArr }: TypeStepsCont) => {
  return (
    <PageContainer>
      {stepsArr.map((step) => {
        return (
          <DefaultButtonPagination key={step} onClick={() => onSetPageValue(step)} isActive={pageValue === step}>
            {step}
          </DefaultButtonPagination>
        );
      })}
    </PageContainer>
  );
};
```