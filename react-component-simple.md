19. `<Suspense>`

**使用時機：**

用來包裹需要懶加載（lazy loading）或異步渲染的元件，提供加載過程中的 fallback UI，有助於提升用戶體驗與初始載入效能。

**使用方式：**

```jsx
<Suspense fallback={fallbackUI}>
  <LazyComponent />
</Suspense>
```

**參數：**

- `fallback`: 用於顯示在子元件尚未準備好（例如尚未載入完成）時的佔位內容
- `children`: 需要懶加載的子元件

**返回值：**

`<Suspense>` 本身不返回任何值，但會根據包裹的內容自動處理加載狀態

重點：

- 通常搭配 `React.lazy()`、`use()` 或 Server Component 使用
- `fallback` 是必填，建議放置 `loading spinner` 或簡單文字
- 可巢狀使用：外層處理整頁、內層處理細節元件
- 與 `<SuspenseList>` 搭配可定義多個 Suspense 區塊的加載順序（React 18+）

**範例：**

```jsx
const LazyProfile = lazy(() => import('./Profile'));

function App() {
  return (
    <Suspense
      fallback={
        <div>載入中...</div>
      }
    >
      <LazyProfile />
    </Suspense>
  );
}
```