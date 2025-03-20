---
title: useState 與 useRef 的差異
description: React 19+
tags:
  - React
  - useState
  - useRef
  - hooks
groups:
  - React
---




React 兩大常用的 hooks，useState 與 useRef，兩者都可以用來存放數據，但是兩種適用場景不同，以下為說明：




## 使用方式




**useState**

```jsx
// 初始化
const [state, setState] = useState(0);

// 取值調用 state
// 注意：setState 是異步的，React 會合併多個 setState，並在下一次渲染時更新
console.log(state); // -> 0

// 更新值（異步）
setState(1);
console.log(state); // -> 0 （仍然是舊值，因為 UI 尚未重新渲染）
```




**useRef**

```jsx
// 初始化
const ref = useRef(0);

// 取值調用 ref.current
console.log(ref.current); // -> 0

// 更新值（同步）
ref.current = 1;
console.log(ref.current); // -> 1 （立即更新）
```




## 用途

**useState：存放狀態，並重新渲染 UI**

當值改變時，會觸發 UI 重新渲染：

```jsx
import { useState } from 'react';

export default function App() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);
  };

  return (
    <>
      <p>{count}</p>
      <button
        onClick={handleClick}
        type='button'
      >
        +1
      </button>
    </>
  );
}
```

**useRef：存放不影響 UI 的數據或 DOM**

適合存放計數、定時器、DOM 等，不影響 UI 渲染：

```jsx
import { useRef } from 'react';

export default function App() {
  const ref = useRef(0);

  const handleClick = () => {
    ref.current += 1;
    alert(ref.current);
  };

  return (
    <>
      <button
        onClick={handleClick}
        type='button'
      >
        +1
      </button>
    </>
  );
}
```

也可獲取 DOM：

```jsx
import { useRef, useEffect } from 'react';

export default function App() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return (
    <input
      ref={inputRef}
    />
  );
}
```




## 總結

|  | useState | useRef |
| :-- | :-- | :-- |
| 用途 | 存放狀態，並重新渲染 UI | 存放不影響 UI 的數據，或是存放 DOM |
| 是否觸發渲染 | 會（但 setState 為異步操作） | 不會 |
| 初始化 | `const [state, setState] = useState(initialState);` | `const ref = useRef(initialValue);` |
| 取值 | `state` | `ref.current` |
| 更新值 | `setState(newValue);`（異步） | `ref.current = newValue;`（同步） |