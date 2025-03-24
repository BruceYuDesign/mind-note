---
title: React Hooks 簡介
description: React 19+
tags:
  - React
  - hooks
groups:
  - React
---




## React Hooks 簡介

在 React 19+ 中一共有 16 個 Hooks，用來取代 Class Component 中的生命週期方法和狀態管理。主要會用到的 Hooks 有 `useState`、`useRef`、`useEffect`、`useContext`、`useReducer`、`useMemo`、`useCallback`，不過這邊我還是先把所有的 Hooks 列出來：

| 類別 | Hook | 用途 | 常用 |
| :-- | :-- | :-- | :-- |
| 狀態管理 | useState | 組件狀態 | V |
|  | useReducer | 複雜狀態管理 | V |
|  | useSyncExternalStore | 整合非 React 的存儲 |  |
| 副作用與生命週期 | useEffect | API、事件、監聽 | V |
|  | useLayoutEffect | DOM 繪製前執行 |  |
|  | useInsertionEffect | CSS-in-JS |  |
| 引用與 DOM 操作 | useRef | DOM、不影響 UI 的數據 | V |
|  | useImperativeHandle | 自訂 ref 行為 |  |
| 性能優化 | useMemo | 記憶計算結果 | V |
|  | useCallback | 記憶函式 | V |
| 非同步與 UI 流暢度 | useTransition | 讓 UI 更流暢（骨架載入） |  |
|  | useDeferredValue | 延遲更新數據 |  |
|  | useOptimistic | 預測性 UI 更新 |  |
|  | useActionState | 處理 Server Action |  |
| 其他 | useId | 生成唯一 ID |  |
|  | useContext | 讀取全域狀態 | V |
|  | useDebugValue | 在 DevTools 中顯示 Hook 資訊 |  |




## 狀態管理




### useState

用來存放狀態，並重新渲染 UI。

```jsx
const [state, setState] = useState(initialState);
```

參數：
- `initialState`：初始狀態值

返回值：
- `state`：狀態值
- `setState`：更新狀態的函數，可接受新的狀態值或函數




#### useState 基本用法

```jsx
import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(prevCount => prevCount + 1);
  };

  return (
    <>
      <p>{count}</p>
      <button
        onClick={handleClick}
        type='button'
      >
        Add
      </button>
    </>
  );
}
```




#### setState 的用法

`setState` 可以接受新的狀態值或函數：

```jsx
const [state, setState] = useState(0);

// 使用數值
setState(1);

// 使用函數
setState(prevState => prevState + 1);
```




#### setState 是異步的

React 會批次處理 setState，如果多次調用 setState，它們會被合併，因此直接使用當前值可能無法即時更新：

```jsx
const [alphaCount, setAlphaCount] = useState(0);
setAlphaCount(alphaCount + 1); // -> 1
setAlphaCount(alphaCount + 1); // -> 1

const [betaCount, setBetaCount] = useState(0);
setBetaCount(prevCount => prevCount + 1); // -> 1
setBetaCount(prevCount => prevCount + 1); // -> 2
```




#### setState 可接受函數作為初始值

可以接受函數作為初始值，可以避免不必要的計算：

```jsx
function initItems() {
  return Array.from({ length: 1000 }, (_, i) => ({ /* ... */ }));
}

export default function ItemsContainer() {
  // 正確的寫法
  const [items, setItems] = useState(initItems);

  // 錯誤的寫法，每次 render 都會重新計算
  // const [items, setItems] = useState(initItems());

  // ...
}
```




### useReducer

用來處理複雜的狀態管理，將更新邏輯集中到 Reducer 處理。

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init?);
```

參數：
- `reducer`：更新邏輯函數，接受當前狀態和動作，來回傳新的狀態
- `initialArg`：初始狀態值，可與 `init` 一起使用
- `init`：初始化函數，會使用 `init(initialArg)` 作為初始值。未填寫則直接使用 `initialArg`

返回值：

- `state`：狀態值
- `dispatch`：更新狀態的函數，接受物件作為參數




#### useReducer 基本用法

```jsx
import { useReducer } from 'react';

// Reducer 函數，可依 action 自訂更新邏輯
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return state + 1;
    case 'decrement':
      return state - 1;
    default:
      return state;
  }
}

export default function Counter() {
  const [state, dispatch] = useReducer(reducer, 0);

  const handleIncrement = () => {
    // 傳遞 action 給 Reducer
    dispatch({ type: 'increment' });
  };

  const handleDecrement = () => {
    // 傳遞 action 給 Reducer
    dispatch({ type: 'decrement' });
  };

  return (
    <>
      <p>{state}</p>
      <button
        onClick={handleIncrement}
        type='button'
      >
        Increment
      </button>
      <button
        onClick={handleDecrement}
        type='button'
      >
        Decrement
      </button>
    </>
  );
}
```




#### dispatch 用法

`dispatch(action)`

- `action`：要傳遞給 Reducer 的物件，Reducer 可依照物件自訂更新邏輯

```jsx
import { useReducer } from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return state + action.payload;
    case 'decrement':
      return state - action.payload;
    default:
      return state;
  }
}

export default function Counter() {
  const [state, dispatch] = useReducer(reducer, 0);

  // ...

  const handleDispatch = () => {
    dispatch({
      type: 'increment',
      payload: 5,
    });
  };

  // ...
}
```




#### Reducer 中不可修改 state

Reducer 中不可修改 state，必須回傳新的狀態：

```jsx
function reducer(state, action) {
  // ...

  // 正確的寫法
  return state + 1;

  // 錯誤的寫法
  // return state += 1;

  // ...
}
```

```jsx
function reducer(state, action) {
  // ...

  // 正確的寫法
  return {
    ...state,
    count: state.count + 1,
  };

  // 錯誤的寫法
  // state.count += 1;
  // return state;

  // ...
}
```




#### 初始化使用 init 函數

使用 init 函數可以避免不必要的計算：

```jsx
import { useReducer } from 'react';
import { fetchBlogs } from '../libs/requestHelper';

// ...

function init(initialArg) {
  return Array.from({ length: 1000 }, (_, i) => ({ /* ... */ }));
}

export default function App() {
  // 正確的寫法
  const [state, dispatch] = useReducer(reducer, initialArg, init);

  // 錯誤的寫法，每次 render 都會重新計算
  // const [state, dispatch] = useReducer(reducer, init(initialArg));

  // ...
}
```




### useSyncExternalStore

用來整合非 React 程式碼的存儲，例如 LocalStorage。

```jsx
const snapshot = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?);
```

參數：
- `subscribe`：訂閱外部存儲的事件
- `getSnapshot`：取得當前狀態的函數
- `getServerSnapshot`：取得伺服器狀態的函數

返回值：
- `snapshot`：當前狀態值

```jsx
import { useSyncExternalStore } from 'react';

function getSnapshot() {
  return navigator.onLine;
}

function subscribe(callback) {
  window.addEventListener('online', callback);
  window.addEventListener('offline', callback);
  return () => {
    window.removeEventListener('online', callback);
    window.removeEventListener('offline', callback);
  };
}

export default function ChatIndicator() {
  const isOnline = useSyncExternalStore(subscribe, getSnapshot);
  return <p>{isOnline ? 'Online' : 'Offline'}</p>;
}
```




#### subscribe 的用法

`subscribe(callback)`

- `callback`：當外部存儲有變動時的回調函數，即 `getSnapshot` 會重新執行

```jsx
import { useSyncExternalStore } from 'react';

// 於 subscribe 中調用 getSnapshot
function getSnapshot() {
  return {
    width: window.innerWidth,
    height: window.innerHeight,
  };
}

function subscribe(callback /* 調用 getSnapshot */) {
  window.addEventListener('resize', callback);
  return () => {
    window.removeEventListener('resize', callback);
  };
}

export default function WindowSize() {
  const windowSize = useSyncExternalStore(subscribe, getSnapshot);

  return (
    <p>{windowSize.width} x {windowSize.height}</p>
  );
}
```

`subscribe` 函數需宣告在元件外部，因為每次 render 都會重新執行。如果在組件內部宣告，會導致每次呼叫時都會回傳新物件，每次回傳值與皆上次不同，將導致無限迴圈：

```jsx
// 正確的寫法
function subscribe(callback) {
  // ...
}

export default function App() {
  // ...

  // 錯誤的寫法，每次 render 都會回傳新物件，將導致無限迴圈
  // function subscribe(callback) {
  //   ...
  // }

  const snapshot = useSyncExternalStore(subscribe, getSnapshot);

  // ...
}
```

如要在元件內部使用 `subscribe` 函數，可以使用 `useCallback` 來避免上述問題：

```jsx
export default function App({ someProp }) {
  // ...

  function subscribe = useCallback((callback) => {
    // ...
  }, [someProp]);

  const snapshot = useSyncExternalStore(subscribe, getSnapshot);

  // ...
}
```




## 副作用與生命週期




### useEffect

用來處理副作用，例如 API 請求、事件監聽、訂閱。如果未使用 `window`、定時器、非同步請求等，則可能不需要使用 `useEffect`。

```jsx
useEffect(setup, dependencies?);
```

參數：

- `setup`：副作用函數
- `dependencies`：依賴的變數，當變數有變動時才會重新執行 `setup`

```jsx
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [serverUrl, roomId]);

  // ...
}
```

#### dependencies 的用法

`dependencies` 是一個陣列，當陣列中的變數有變動時，會重新執行：

```jsx
// 當 dependency 有變動時重新執行
useEffect(() => {
  // ...
}, [dependency]);

// 初始化時執行一次
useEffect(() => {
  // ...
}, []);

// 每次 render 都會執行
useEffect(() => {
  // ...
});
```