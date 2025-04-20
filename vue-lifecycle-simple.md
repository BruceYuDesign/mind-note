
| 名稱 | 簡述說明 |
| --- | --- |
| onMounted() | 元件首次掛載到 DOM 後執行。常用於 DOM 操作或資料請求。 |
| onUpdated() | 每次 DOM 更新後執行。常用於追蹤變更後的狀態或動畫處理。 |
| onUnmounted() | 元件從 DOM 卸載時執行。常用於清除資源（如監聽器、計時器）。 |
| onBeforeMount() | 元件掛載前執行。此時尚未產生 DOM。 |
| onBeforeUpdate() | 更新前執行，通常用來對比變更前後的資料或狀態。 |
| onBeforeUnmount() | 元件被卸載前執行，可進行清理動作。 |
| onErrorCaptured() | 捕捉子元件錯誤，提供類似 try-catch 的容錯機制。 |
| onRenderTracked() | 用於追蹤 reactive 的依賴來源（開發除錯用途）。 |
| onRenderTriggered() | 當依賴變化導致重新 render 時觸發（開發除錯用途）。 |
| onActivated() | <KeepAlive> 中的元件被重新啟用時執行。 |
| onDeactivated() | <KeepAlive> 中的元件被暫停時執行。 |
| onServerPrefetch() | 僅用於 SSR，允許在伺服器端預取資料。 |