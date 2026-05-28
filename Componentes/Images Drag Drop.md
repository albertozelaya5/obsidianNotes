## CardImg.tsx

```tsx
import styled from "styled-components";

type CardImg = {
  task: any;
};

export default function CardImg({ task }: CardImg) {
  return (
    <ImgContainer>
      <ImgField style={{ backgroundImage: `url(${task.imgUrl})` }} />
    </ImgContainer>
  );
}

const ImgContainer = styled.figure`
  display: flex;
  justify-content: center;

  cursor: pointer;
`;

const ImgField = styled.div`
  width: 160px;
  height: 160px;
  background-color: white;
  background-size: cover;
`;

```

## DragDrop padre

```tsx
      <DragDrop
        //
        isLoading={isLoading}
        useCustomLoading
      >
        <TabledList>
          {testImgs.map((tasks, listIndex) => {
            return (
              <DragList
                //
                key={listIndex}
                description="taskDescription"
                title={statuses[listIndex]}
                statusColor={colors[listIndex]}
                statusText={statusesText[listIndex]}
                {...{ tasks, onTaskDrop, listIndex }}
              >
                {tasks?.map((task) => {
                  return <CardImg key={task.imgUrl} task={task} />;
                })}
              </DragList>
            );
          })}
        </TabledList>
      </DragDrop>
```

### Json test

```json
[
  [
    {
      "imgUrl": "https://upload.wikimedia.org/wikipedia/commons/9/95/2024_Tesla_Cybertruck_Foundation_Series%2C_front_left_%28Greenwich%29.jpg"
    }
  ],
  [
    {
      "imgUrl": "https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Tesla_Model_3_%282023%29_Autofr%C3%BChling_Ulm_IMG_9282.jpg/1200px-Tesla_Model_3_%282023%29_Autofr%C3%BChling_Ulm_IMG_9282.jpg"
    }
  ],
  [
    {
      "imgUrl": "https://hips.hearstapps.com/hmg-prod/images/2025-tesla-model-s-1-672d42e172407.jpg?crop=0.465xw:0.466xh;0.285xw,0.361xh&resize=1200:*"
    }
  ],
  [
    {
      "imgUrl": "https://cdn.autobild.es/sites/navi.axelspringer.es/public/bdc/dc/fotos/Tesla_Model_3_202403.jpg?tf=3840x"
    }
  ]
]
```

