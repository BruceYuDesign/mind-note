# Vue 3 API 筆記整理

---

## ref

**使用時機：**

當需建立具有 .value 的單一響應式變數，用於 primitive 或 DOM 元素綁定。

**使用方式：**

```js
const state = ref(value)
```

參數：

- `value`: 初始值（可為任意型別）

返回值：

- `count`: Ref 物件，需透過 `.value` 取得或修改值

**重點：**

- `.value` 必須用於取值與設定
- 可用於 DOM 綁定（`<div ref="el" />`）
- 與 `reactive` 協作時會自動展開

**範例：**

```js
const count = ref(0);

function increment() {
  count.value++;
}
```

---

## computed

**使用時機：**

建立具快取的派生狀態，僅當依賴變更時重新計算。

**使用方式：**

```js
const double = computed(getter)
```

**參數：**

- `getter`: 函式，回傳計算結果

**返回值：**

- `computedRef`: Ref 物件，內含計算後的值

**重點：**

- 僅在依賴變更時重新執行
- 支援 get / set（雙向）
- 同樣透過 .value 存取

**範例：**

```js
const count = ref(1)
const double = computed(() => count.value * 2)
```

---

## reactive

**使用時機：**

當需建立具有巢狀結構的響應式物件。

**使用方式：**

```js
const state = reactive(target)
```

**參數：**

- `target`: 任意物件

**返回值：**

- `proxy`: 被轉換為 `Proxy` 的響應式物件

**重點：**

- 僅限物件、陣列、`Map`、`Set` 等
- 無需使用 `.value`
- 無法直接脫離原物件比對

**範例：**

```js
const state = reactive({ count: 0 })
state.count++
```

---

## watchEffect

**使用時機：**

當需立即執行 `effect` 並自動追蹤其依賴。

**使用方式：**

```js
watchEffect(effect, onCleanup)
```

**參數：**

- `effect`: 函式，會自動執行並追蹤內部依賴
- `onCleanup`: 返回一個停止 `watcher` 的函式

**返回值：**

無返回值

**重點：**

- 執行時自動收集依賴
- 無需指定依賴來源
- 無法比對前後變化（需 watch()）

**範例：**

```js
const count = ref(0)
watchEffect(() => {
  console.log(`count is: ${count.value}`)
})
```

---

## watchPostEffect

**使用時機：**

用於 DOM 更新後立即執行的副作用，與 `watchEffect` 相同語法但延後時機。

**使用方式：**

```js
watchPostEffect(effect, onCleanup)
```

**參數：**

與 `watchEffect` 相同

**重點：**

- 適用於 DOM 操作相關副作用
- 依賴追蹤與 `watchEffect` 相同

**範例：**

```js
watchPostEffect(() => {
  console.log('DOM ready')
})
```

---

## watchSyncEffect

**使用時機：**

立即同步執行副作用，依賴追蹤機制與 `watchEffect` 一致。

**使用方式：**

```js
watchSyncEffect(effect, onCleanup)
```

**參數：**

與 `watchEffect` 相同

**重點：**

- 與 Vue 更新同步（blocking）
- 使用時需注意阻塞效能問題

**範例：**

```js
watchSyncEffect(() => {
  console.log("同步執行", count.value)
})
```

---

## watch

**使用時機：**

需對特定變數變化做出反應，並可取得前後值差異時使用。

**使用方式：**

```js
watch(source, callback, options)
```

**參數：**

- `source`: 被監聽的變數、getter 或陣列
- `callback`: 變化時執行的函式，提供新值與舊值
- `options`: 監聽選項（可選）
- `options.immediate`: 是否立即執行
- `options.deep`: 是否深層監聽
- `options.flush`: 更新時機（pre、post、sync），pre 為預設值，功能為在 DOM 更新前執行，post 為在 DOM 更新後執行，sync 為同步執行

**重點：**

- 非即時執行，預設在變更後才觸發
- 可加上 `immediate: true` 強制執行一次
- 支援深層監聽與 `flush` 時機設定

**範例：**

```js
watch(() => count.value, (newVal, oldVal) => {
  console.log(`from ${oldVal} to ${newVal}`)
}, {
  immediate: true,
  deep: true,
  flush: 'post'
})
```