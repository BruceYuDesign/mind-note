## 基礎元件

- `<NuxtPage>`：用於渲染對應路由的頁面內容（在 layout 中使用）。
- `<NuxtLayout>`：定義與切換當前使用的 layout。
- `<NuxtLink>`：內部路由導航元件，具備 prefetch、自動 active class。
- `<NuxtErrorBoundary>`：包覆元件，捕捉錯誤並顯示 fallback UI。
- `<NuxtWelcome>`：預設歡迎畫面，專供開發初期使用。

## 執行環境控制

- `<ClientOnly>`：只在 client side 渲染內部內容，避免 SSR 錯誤。
- `<DevOnly>`：只在開發模式下渲染，用於 debug 元件。
- `<NuxtClientFallback>`：SSR 無資料時，在 client side fallback 渲染內容。
- `<Teleport>`：將內容傳送至指定 DOM 節點（常用於 modal、tooltip）。

## 媒體處理

- `<NuxtPicture>`：延伸 `<picture>`，支援多尺寸圖片載入、自動格式優化。
- `<NuxtImg>`：Nuxt Image 模組元件，具備 lazy load、尺寸裁切、CDN 支援。

## 互動 & 系統元件

- `<NuxtLoadingIndicator>`：頁面載入進度條元件，可自定顏色/位置。
- `<NuxtRouteAnnouncer>`：支援無障礙功能的路由切換提示（螢幕閱讀器用）。
- `<NuxtIsland>`：支援 partial hydration，提升性能（實驗性功能）。