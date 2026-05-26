---
name: solid-check
description: 檢查指定命名空間或類別是否符合 SOLID 與 IoC 原則。當用戶說「檢查 SOLID」、「solid check」、「審查 xxx 是否符合 SOLID」、「檢查 xxx 命名空間」或需要評估特定類別/命名空間的設計品質時使用
---

# 檢查 SOLID / IoC 合規性

## 檢查對象
`$ARGUMENTS`

`$ARGUMENTS` 可以是：
- 單一類別的 FQCN（例：`App\AI\Tools\UpdateArticleTool\UpdateArticleTool`）
- PHP 命名空間（例：`App\AI\Tools\UpdateArticleTool`）
- JavaScript/TypeScript 模組或目錄（例：`resources/script/components/message/`、`useMessageScroll`）
- 檔案或資料夾路徑（例：`app/AI/Tools/UpdateArticleTool/`、`resources/script/stores/product-store.ts`）

支援語言：PHP（Laravel）、JavaScript / TypeScript（Vue Composition API、composables、stores、純函式模組）。

## 執行流程

### 1. 蒐集範圍
- 用 Glob/Read 工具列出該範圍下所有相關檔案（`.php` / `.ts` / `.tsx` / `.js` / `.vue`），不要用 bash 的 find/grep
- 完整讀取每個檔案，**不要只看摘要**
- 用 Grep 找出該類別/模組/函式在專案其他地方的使用點（呼叫者、注入處、繼承者、import 端）
- 先判斷語言型態（PHP class / Vue SFC / composable / store / 純 TS 模組），不同型態套用對應的檢查重點

### 2. 逐項檢查

#### 單一職責原則 (SRP)
- 類別 / 模組 / composable 是否只有一個變更理由？
- 是否同時處理 HTTP/驗證/業務邏輯/資料存取/格式化/UI 狀態？
- 公開方法或 export 是否語意一致，還是混雜不相關職責？
- 檔案長度與方法數量是否暗示職責過多（PHP class >300 行或 >8 個公開方法、Vue SFC `<script>` >250 行、composable export >6 個值 為警訊）
- Vue：template / script / style 是否塞太多不相關邏輯，該抽 child component 或 composable？

#### 開放封閉原則 (OCP)
- 新增需求時是否得修改現有類別/模組內部？
- 是否用 `if/switch` 對型別/字串分支，而不是多型或 strategy map？
- 擴充點是否透過介面、抽象類別、Strategy/Factory（PHP）或 callback、handler map、plugin 物件（JS/TS）暴露？

#### 里氏替換原則 (LSP)
- 子類別 / 子型別是否破壞父契約（拋出父類別未宣告的例外、回傳 null、收緊前置條件）？
- override 方法的型別宣告是否與父類別/介面一致？
- 是否有 `instanceof` / `typeof` / discriminator 字串後分支處理特定子型別（LSP 違反訊號）？
- TS：實作 interface 的物件是否用了 `as` 或 `any` 強制過關，把違反藏起來？

#### 介面隔離原則 (ISP)
- 介面 / TS type 是否被迫實作用不到的方法（空 method、`throw new BadMethodCallException`、回 `undefined`）？
- 「胖介面」或「胖 props」是否該拆成多個小介面？
- 客戶端 / 子元件是否只依賴自己需要的方法或 prop？
- Vue：component props 是否塞了一堆只有少數分支用到的欄位，該拆成獨立 component 或多個 prop group？

#### 依賴反轉原則 (DIP)
- 高層模組是否直接 `new` 低層具體類別 / 直接 `import` 具體實作？
- 建構式注入或函式參數的型別是抽象還是具體？
- PHP：是否有 facade、`app()`、`resolve()`、`static::` 的隱性相依？
- JS/TS：composable / 模組是否直接 import `axios`、`fetch`、`localStorage`、`window`、特定 store，而沒有透過參數或 inject 注入？
- 外部 I/O（HTTP、檔案、DB、瀏覽器 API）是否藏在抽象後面，方便測試替換？

#### IoC / 容器使用
- **PHP / Laravel**：
  - 服務是否透過 Service Provider 註冊？需要單例的有沒有 `singleton`？
  - 是否有 service location 反模式（執行期 `app(SomeService::class)` 取代建構式注入）？
  - 介面有沒有對應的綁定？綁定是否寫在合理的 Provider？
  - contextual binding、tagged services 是否善用？
- **Vue / TS**：
  - 跨層共用狀態是否透過 `provide` / `inject` 或 Pinia store，而不是 module-level singleton 偷渡？
  - composable 是否回傳 factory 而非 module-scope 共用實例（避免 SSR/多實例衝突）？
  - 元件之間是否用 props / emits / inject 明確宣告依賴，而非靠全域事件 bus、`window` 變數？
  - 注意 SSR-safe：module-scope 狀態是否會跨 request 互相污染？（參考 [[feedback_use_store_inject_fallback]] 模式）

#### Laravel / PHP 額外檢查
- 是否該用 PHP Attribute 或 Laravel Attribute 取代舊寫法
- Eloquent Model 是否塞了不屬於它的 query/服務邏輯
- 控制器是否變成「胖控制器」（應該抽到 Action / Service / Form Request）
- 是否有靜態方法持有狀態、global 變數、Singleton 反模式

#### Vue / TypeScript 額外檢查
- Vue SFC 是否該拆成 presentational + container 兩層？
- 大量 `ref` / `reactive` / `watch` 散落 `<script setup>`，該抽到 composable？
- composable 命名是否以 `use` 開頭，且回傳值結構穩定可解構？
- 是否誤用 `any`、`as unknown as`、`@ts-ignore` 規避型別系統？
- 副作用（fetch、subscribe、timer）是否配對 cleanup（`onUnmounted` / `onScopeDispose`）？
- store（Pinia / 自製）是否把純運算邏輯也綁進 store 狀態，而不是抽純函式？

### 3. 出報告
報告格式（中文）：
1. **範圍**：列出檢查了哪些檔案
2. **依原則分節**：每節列出
   - 違反點：`file_path:line_number` + 簡短說明
   - 風險：違反後會造成什麼問題（測試難寫？耦合？擴充痛？）
3. **整體評估**：合格 / 有改善空間 / 嚴重需重構
4. **重構建議**：每個違反點對應一個具體可行的重構手法（不要寫程式碼，描述方向即可）

### 4. 與使用者討論
- 若全數合格 → 簡短說明亮點即可，不需要找碴湊問題
- 若有違反 → **不要直接動手改**，先把報告與重構建議呈現給使用者，等使用者決定要不要重構、用哪個方向
- 使用 AskUserQuestion 列出 2-4 個方向（保持現狀 / 局部抽介面 / 整體拆分 / ……）讓使用者選

## 注意事項
- 永遠用中文回覆
- 只做檢查與討論，**禁止在這個 skill 內直接修改程式碼**；要修改請等使用者授權後切換到 `run` 或 `fix` skill
- 不要把「程式碼能跑」當成合格標準，重點是設計層面的可維護性
- 對小型 Value Object、純 DTO、單一用途的 Action 類別，不要強行套用所有原則，務實判斷
- 若 `$ARGUMENTS` 不清楚（找不到對應檔案、命名空間有多義），先用 AskUserQuestion 跟使用者確認範圍再開始
- 對 framework 規範的固定結構（Filament Resource、Nova Resource、Livewire Component、Vue SFC 三段式、Pinia store 三件套）不要視為 SRP 違反
- PHP 與 JS/TS 的「IoC」意義不同：PHP 看 Laravel 容器綁定，JS/TS 看 provide/inject、Pinia、依賴注入式 composable，不要混用判準
