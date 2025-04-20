| Composable | 說明 |
|------------|------|
| useRoute() | 取得目前的路由資訊（如 params, query, path）。 |
| useRouter() | 操作路由導覽（如 push, replace）。 |
| useState() | 建立跨元件共享的 reactive 狀態（Server-safe）。 |
| useAsyncData() | 在 SSR/CSR 中取得資料，可快取與自動串接 loading/error 狀態。 |
| useFetch() | 類似 useAsyncData，專為 HTTP 請求設計（底層封裝 $fetch）。 |
| useHead() | 設定頁面 <head> 資訊（title, meta, link 等）。 |
| useSeoMeta() | 進階 SEO meta 設定，支援 Open Graph 與 Twitter Cards。 |
| useCookie() | 存取與操作 cookies，支援 SSR/CSR。 |
| useRuntimeConfig() | 讀取 nuxt.config.ts 中的 runtimeConfig。 |
| useNuxtApp() | 存取 app-level 的上下文，例如 $pinia, $i18n 等。 |
| useRouteParams() | 快取與 reactive 地存取 route params。 |
| useRequestHeaders() | 讀取 SSR 時的 request headers。 |
| useRequestEvent() | 取得 SSR 請求的 event，通常用於中介層。 |
| useError() | 拋出自訂錯誤，用於錯誤頁或錯誤捕捉流程。 |
| useRedirect() | 在 server/client 端執行重導向。 |
| useLazyAsyncData() | 僅在需要時才延遲請求（支援 lazy loading）。 |