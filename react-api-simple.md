# React 19 API 筆記整理

---

## cache

**使用時機：**

用於管理具有快取需求的資料或計算結果。常見於長時間運行的操作或需要避免重複計算的情境。

**使用方式：**

```jsx
const cachedFn = cache(fn);
```

**參數：**

- `fn`: 用來創建一個快取物件

**返回值：**

- `cachedFn`: 一個函式，當被調用時會檢查快取是否存在，若存在則返回快取的結果，否則執行原始函式並快取結果。

**注意事項：**

- 快取的數據不會每次重新計算，能提高性能。
- 適用於那些不需要每次渲染都重新運算的情境。

**範例：**

```jsx
const getMetrics = cache(calculateMetrics);

function Chart({data}) {
  const report = getMetrics(data);
  // ...
}
```

---

## createContext

**使用時機：**

用於創建一個上下文對象，並在 React 元件樹中共享資料，避免繁瑣的 `props` 層層傳遞。

**使用方式：**

```jsx
const MyContext = createContext(defaultValue);
```

**參數**

- `defaultValue`: 預設值，用於當上下文未被 `<Provider/>` 包裹時的回退值

**返回值：**

- `MyContext`: 由 `createContext` 返回的上下文對象

**注意事項：**

- `Provider` 用來包裹需要共享資料的範圍，`Consumer` 用來讀取資料。
- 用於跨越多層傳遞資料，而不需要將資料逐層傳遞。

**範例：**

```jsx
const ThemeContext = createContext('light');

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Child />
    </ThemeContext.Provider>
  );
}

function Child() {
  const theme = useContext(ThemeContext);
  return <p>Current theme: {theme}</p>;
}
```

---

## lazy

**使用時機：**

用於動態加載元件，進行懶加載，以提高應用程式的加載效率。

**使用方式：**

```jsx
const SomeComponent = lazy(load)
```

**參數：**

- `lazy`: 用於動態加載元件的高階函式，接受一個函式返回 `import()` 的結果

**參數：**

- `SomeComponent`: 使用 `lazy` 返回的動態加載元件

**注意事項：**

- 必須與 `Suspense` 配合使用，來顯示加載狀態。
- 用於減少初次加載的 JavaScript 大小。

**範例：**

```jsx
const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}
```

--- 

## memo

**使用時機：**

用於避免不必要的重渲染，特別是當傳入的 `props` 沒有改變時，React `memo` 可以跳過渲染過程，提升性能。

**使用方式：**

```jsx
const MemoizedComponent = memo(SomeComponent, arePropsEqual?)
```

**參數：**

- `SomeComponent`: 需要被優化的元件
- `arePropsEqual`: 可選的函式，用來比較前後 `props` 是否相等，若相等則不重新渲染。需要返回布林值。

**返回值：**

`MemoizedComponent`: 優化後的元件，只有當 props 改變時才會重新渲染

**注意事項：**

- `memo` 只適用於函式元件。
- 若 `props` 是複雜資料結構，則應該自定義 `propsAreEqual` 函式來比較 `prop` 是否改變。

**範例：**

```jsx
const MemoizedCounter = React.memo(function Counter({ count }) {
  return <div>Count: {count}</div>;
});

function App() {
  const [count, setCount] = useState(0);
  return <MemoizedCounter count={count} />;
}
```

---

## startTransition

**使用時機：**

用於將某些狀態更新標記為非緊急，以避免阻塞主線程，提升應用的響應性。

**使用方式：**

```jsx
startTransition(action);
```

**參數：**

- `action`: 一個函式，包含需要被標記為非緊急的狀態更新

**返回值：**

無返回值

**注意事項：**

- `startTransition` 會告訴 React 這個狀態更新不是緊急的，React 可以在空閒時進行更新。
- 適用於需要進行大量計算或渲染的情況，避免阻塞 UI 更新。

**範例：**

```jsx
function TabContainer() {
  const [tab, setTab] = useState('about');

  function selectTab(nextTab) {
    startTransition(() => {
      setTab(nextTab);
    });
  }
  // ...
}
```

---

## use

**使用時機：**

React 18 引入了 `use`，它允許你在 Concurrent Mode 中使用函式式的渲染方法來獲取異步數據。

**使用方式：**

```jsx
const value = use(resource);
```

**參數：**

- `resource`: 需要在渲染過程中異步取得的資料

**返回值：**

- `value`: 當資料取得後，會傳回的數據

**注意事項：**

- `use` 必須在 Concurrent Mode 啟用的情況下使用。
- 用於在渲染時等待數據，不會阻塞 UI 更新。

**範例：**

```jsx
function DataComponent() {
  const data = use(fetchData());
  return <div>{data}</div>;
}
```