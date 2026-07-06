### STATE MANAGEMENT **TOOL** OPTIONS

En apps grandes tratamos todo el estado remoto como estado global

|                 |  LOCAL STATE                                            | GLOBAL STATE                                                                                                        |
| --------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| UI STATE 🪄     | useState, useReducer, useRef                            | Context API + useState o useReducer - Redux, Zustand, Recoil, etc - React Router                                    |
| REMOTE STATE 🌐 | fetch + useEffect + useState/useReducer (in small apps) | (Context API + useState o useReducer - Redux, Zustand, Recoil, etc - menos adecuados) - React Query, SWR, RTK Query |


> [!IMPORTANT]

> Tools highly specialized in handling **remote** state
> - React Query, SWR, RTK Query


