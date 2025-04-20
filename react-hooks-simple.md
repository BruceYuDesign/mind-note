# React 19 Hook 筆記整理（續）

---

## useState

**使用時機：**

在函式元件中管理本地狀態，並觸發 UI 更新。

**使用方式：**

```jsx
const [state, setState] = useState(initialState)
```

**參數：**

- `initialState`: 初始狀態值

**返回值：**

- `state`: 當前狀態值
- `setState`: 更新狀態的函式。更新方式：`setState(newState)` 或 `setState(prevState => /* ... */)`

**重點：**

- 更新狀態會觸發重新渲染
- 非同步，狀態更新可能會合併，且不會即時反映
- 支援 lazy 初始化，即：`setState(prevState => /* ... */)`
- 可以傳入函式作為參數，這樣可以獲取到最新的狀態值。注意：不要加 `()`，因為這樣會立即執行函式，而不是傳遞函式本身

**範例：**

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

---

## useReducer

**使用時機：**

用於更複雜的狀態邏輯，或需要管理多個狀態變量的情況，通常替代 useState。

**使用方式：**

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init?);
```

**參數：**

- `reducer`：更新邏輯函數，接受當前狀態和動作，來回傳新的狀態
- `initialArg`：初始狀態值，可與 `init` 一起使用
- `init`：初始化函數，會使用 `init(initialArg)` 作為初始值。未填寫則直接使用 `initialArg`

**返回值：**

- `state`: 當前的狀態值
- `dispatch`: 用來觸發動作更新狀態的函式

**重點：**

- 用於複雜的狀態邏輯，比 `useState` 更適合多種狀態更新操作
- `dispatch` 通常與 `action` 一起使用來更新狀態
- `Reducer` 中不可修改 `state`，必須回傳新的狀態
- 初始化若要使用函數，可傳入 `init` 參數，這樣可以避免在每次渲染時都計算初始狀態

**範例：**

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

---

## useRef

**使用時機：**

存儲可變資料（如 DOM 節點或跨渲染的變量），不會觸發重新渲染。

**使用方式：**

```jsx
const ref = useRef(initialValue)
```

**參數：**

- `initialValue`: 初始值

**返回值：**

- `ref`: 包含 `current` 屬性的物件，`ref.current` 可以被直接修改

**重點：**

- `useRef` 用來存儲資料，不會觸發重新渲染
- 可以用於引用 DOM 節點
- 用來存儲跨渲染的資料或事件處理器

**範例：**

```jsx
function FocusInput() {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input
        ref={inputRef}
      />
      <button
        onClick={focusInput}
      >
        Focus Input
      </button>
    </div>
  );
}
```

---

## useEffect

**使用時機：**

在元件渲染後執行副作用（如資料抓取、訂閱等）。

**使用方式：**

```jsx
useEffect(effect, dependencies)
```

**參數：**

- `effect`: 副作用函式
- `dependencies`: 依賴陣列，當陣列中的值變化時會重新執行 `effect`

**返回值：**

- 無返回值，`effect` 本身的副作用會在渲染後執行

**重點：**

- `useEffect` 在渲染後執行，不會阻止瀏覽器更新畫面
- 當 `dependencies` `改變時，effect` 會被重新執行
- 如果 `dependencies` 傳入空陣列 []`，effect` 只會在第一次渲染後執行一次
- 如果未傳入 `dependencies`，`effect` 會在每次渲染後執行

**範例：**

```jsx
import { useState, useEffect } from 'react';

function FetchData() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('/api/data')
      .then(response => response.json())
      .then(setData);
  }, []);

  return (
    <p>
      {data ? JSON.stringify(data) : 'Loading...'}
    </p>
  );
}
```

---

## useMemo

**使用時機：**

用來記憶計算結果，避免不必要的計算，提升性能。

**使用方式：**

```jsx
const cachedValue = useMemo(calculateValue, dependencies);
```

**參數：**

- `calculateValue`: 計算函式，返回記憶化的值
- `dependencies`: 依賴陣列，當其中的任何值改變時重新執行計算

**返回值：**

- `cachedValue`: 計算後的記憶化值，只有當 dependencies 改變時才會重新計算

**重點：**

- `useMemo` 用來防止每次渲染都進行昂貴的計算
- 只有在依賴值變化時才會重新計算，否則會返回上次計算的結果
- 主要用於優化性能，避免不必要的計算

**範例：**

```jsx
function ExpensiveComponent({ a, b }) {
  const memoizedResult = useMemo(() => expensiveComputation(a, b), [a, b]);

  return <div>{memoizedResult}</div>;
}
```

---

## useCallback

**使用時機：**

用來記憶函式，防止函式在每次渲染時重建，適用於傳遞給子元件的函式。

**使用方式：**

```jsx
const cachedFn = useCallback(fn, dependencies);
```

**參數：**

- `fn`: 需要記憶的函式
- `dependencies`: 依賴陣列，當其中的值改變時，callback 函式會被重新創建

**返回值：**

- `cachedFn`: 記憶化後的函式，只有當 `dependencies` 改變時才會更新

**重點：**

- `useCallback` 用來記憶函式，防止函式每次渲染時都被重新創建
- 對於傳遞給子元件的回呼函式，`useCallback` 可以避免不必要的重渲染
- 可以用於優化性能

**範例：**

```jsx
function Button({ onClick }) {
  return (
    <button
      onClick={onClick}
    >
      Click Me
    </button>
  );
}

function Parent() {
  const memoizedClick = useCallback(() => {
    console.log('Button clicked');
  }, []);

  return (
    <Button
      onClick={memoizedClick}
    />
  );
}
```

---

## useContext

**使用時機：**

用來在組件樹中共享資料，避免層層傳遞 `props`。

**使用方式：**

```jsx
const value = useContext(SomeContext);
```

**參數：**

- `SomeContext`: 要使用的 `Context` 物件

**返回值：**

- `value`: 該 `Context` 中的當前值

**重點：**

- `useContext` 可以讓你在組件中使用上下文（`Context`）的值
- 可以避免層層傳遞 `props`，直接在需要的組件中訪問共享的狀態

**範例：**

```jsx
const ThemeContext = React.createContext('light');

function ThemedComponent() {
  const theme = useContext(ThemeContext);
  return (
    <div
      style={{
        background: theme === 'dark' ? 'black' : 'white',
      }}
    >
      Theme is {theme}
    </div>
  );
}
```

---

## useLayoutEffect

**使用時機：**

用來在瀏覽器繪製前執行副作用，通常用來測量 DOM 或觸發動畫。

**使用方式：**

```jsx
useLayoutEffect(effect, dependencies);
```

**參數：**

- `effect`: 副作用函式
- `dependencies`: 依賴陣列，當陣列中的值變化時會重新執行 `effect`

**返回值：**

- 無返回值

**重點：**

- `useLayoutEffect` 會在所有 DOM 更新後但在畫面更新之前執行，這樣可以確保 DOM 在畫面呈現之前被完全設置
- 用於處理需要準確測量 DOM 或進行動畫操作的場景
- 應盡量避免使用 `useLayoutEffect`，因為它會阻塞瀏覽器的繪製，影響性能
- 如果不需要在畫面更新之前執行副作用，建議使用 `useEffect`

**範例：**

```jsx
function ResizeComponent() {
  const [size, setSize] = useState({ width: 0, height: 0 });

  useLayoutEffect(() => {
    const updateSize = () => {
      setSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    };

    updateSize();
    window.addEventListener('resize', updateSize);
    return () => window.removeEventListener('resize', updateSize);
  }, []);

  return (
    <div>
      {`Width: ${size.width}, Height: ${size.height}`}
    </div>
  );
}
```

---

## useImperativeHandle

**使用時機：**

用於自定義暴露給父元件的 `ref` 屬性。

**使用方式：**

```jsx
useImperativeHandle(ref, createHandle, dependencies);
```

**參數：**

- `ref`: 父組件的 `ref`
- `createHandle`: 返回自定義的物件或方法，暴露給父元件
- `dependencies`: 當依賴改變時重新創建暴露的物件

**返回值：**

- 無返回值

**重點：**

- `useImperativeHandle` 讓你控制暴露給父元件的 `ref` 物件
- 可以用來定義自定義方法，而不暴露整個 DOM 節點

**範例：**

```jsx
function MyInput({ ref }) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => {
    // 自訂 ref 行為，父組件僅可調用以下方法
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);

  return <input ref={inputRef} />;
};
```

---

## useTransition

**使用時機：**

用來處理過渡性狀態，避免影響 UI 的響應性。

**使用方式：**

```jsx
const [isPending, startTransition] = useTransition();
```

**參數：**

無需傳入參數

**返回值：**

- `isPending`: 是否處於過渡狀態
- `startTransition`: 用於觸發過渡的函式

**重點：**

- `useTransition` 用來在執行昂貴操作時，保持 UI 的響應性
- `startTransition` 會讓 React 認為該操作是過渡性的，允許 React 延遲某些更新

**範例：**

```jsx
function Search() {
  const [query, setQuery] = useState('');
  const [isPending, startTransition] = useTransition();

  const handleChange = (event) => {
    startTransition(() => {
      setQuery(event.target.value);
    });
  };

  return (
    <div>
      <input
        value={query}
        onChange={handleChange}
      />
      {
        isPending
          ? 'Loading results...'
          : <Results query={query} />
      }
    </div>
  );
}
```

---

## useOptimistic

**使用時機：**

用來處理樂觀更新，當你預期某個操作會成功時，可以立即更新 UI，而不必等待服務器回應。

**使用方式：**

```jsx
const [optimisticState, setOptimisticState] = useOptimistic(initialValue, updateFunction);
```

**參數：**

- `initialValue`: 初始狀態值
- `updateFunction`: 當狀態變更時，根據優先權更新的函式

**返回值：**

- `optimisticState`: 當前的樂觀狀態
- `setOptimisticState`: 更新樂觀狀態的函式

**重點：**

- `useOptimistic` 使你能夠提前更新 UI，即使後端操作尚未完成
- 當操作成功時，狀態會最終與伺服器狀態同步
- 這對用戶體驗有幫助，尤其是在高延遲的網絡環境中

**範例：**

```jsx
function LikeButton({ postId }) {
  const [likes, setLikes] = useState(0);

  const handleLike = () => {
    setOptimisticState((prevState) => prevState + 1);
    // 假設這是與後端的互動
    fetch(`/like/${postId}`)
      .then(() => {
        // 成功後同步更新
        setLikes(likes + 1);
      })
      .catch(() => {
        // 處理錯誤情況
        setLikes(likes);
      });
  };

  return (
    <button
      onClick={handleLike}
    >
      Like {likes}
    </button>
  );
}
```

---

## useDeferredValue

**使用時機：**

用來處理需要延遲更新的情境，當 UI 較為繁重或數據處理時間較長時，可以延遲某些值的更新，避免阻塞主線程。

**使用方式：**

```jsx
const deferredValue = useDeferredValue(value);
```

**參數：**

- `value`: 需要延遲處理的值

**返回值：**

- `deferredValue`: 延遲更新的值，這個值會在主線程空閒時才更新

**重點：**

- `useDeferredValue` 允許你將某些不那麼緊急的更新延遲到主線程閒置時進行
- 常用於處理大量資料的輸入或長時間運算，提升 UI 響應性

**範例：**

```jsx
function Search() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);

  return (
    <div>
      <input
        value={query}
        onChange={event => setQuery(event.target.value)}
      />
      <div>Searching for: {deferredQuery}</div>
    </div>
  );
}
```