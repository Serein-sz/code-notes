# Hooks

## useState(initialState)

```react
import { useState } from 'react';

function MyComponent() {
  const [age, setAge] = useState(28);
  const [name, setName] = useState('Taylor');
  const [todos, setTodos] = useState(() => createTodos());
  // ...
```

### 参数

* `initialState`: state初始化的值，它可以是任何值，在初始渲染后，其将被忽略。
  * 如果`initialState`类型为参数，则它将被视为初始化函数。其应是纯函数，不应接受任何参数，且应该返回任意类型的值。当初始化组件时，React将调用初始化函数，并将其返回值作为初始值。

### 返回

`useState`返回一个由两个值组成的数组。

1. 当前state，首次渲染时，它与`initialState`匹配。
2. set函数，它可以更新state值并重新触发渲染。

### 注意事项

* `useState`是一个Hook，你只能在**组件的顶层**或自己的Hook中调用他。你不能在循环或条件语句中调用它。如果你需要这样做，请提取一个新组件，并将状态移入其中。
* 在严格模式中，React 将 **两次调用初始化函数**，以 帮你找到意外的不纯性。这只是开发时的行为，不影响生产。如果你的初始化函数是纯函数（本该是这样），就不应影响该行为。其中一个调用的结果将被忽略。

***



### `set`函数(`setSomething(nextState)`)



