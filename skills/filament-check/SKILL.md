---
name: filament-check
description: 檢查指定的 Filament 程式碼是否符合 filament-run 規範。當用戶說「檢查 filament」、「filament check」、「檢查 xxx 是否符合 filament 慣例」、「審查 Filament Resource/RelationManager」或需要驗證 Filament 程式碼合規性時使用
---

# 檢查 Filament 合規性

依據 `filament-run` skill 所列的專案 Filament 慣例，檢查指定範圍的程式碼是否合規。

## 規範來源（單一真實來源）

**本規範條目一律以 `filament-run` skill 的 `SKILL.md` 為準，每次執行時即時讀取，確保 `filament-run` 調整後本檢查自動跟進、不會遺漏或失準。

執行第一步必須先讀取：

```
/var/www/skills/skills/filament-run/SKILL.md
```

讀到的每一條規範（元件使用、Resource 放置、Action 鉤子、Activity Log、v5 Namespace、Select 預設值、RelationManager 撰寫規範……）即為本次檢查的檢查項。**以該檔當下的實際內容為準**，不要依賴本 skill 過去的記憶或下方範例；若該檔新增、刪改條目，檢查項同步增減。

## 檢查對象
`$ARGUMENTS`

`$ARGUMENTS` 可以是：
- 單一 Filament 類別的 FQCN（例：`App\Admin\Resources\Articles\ArticleResource`）
- PHP 命名空間或目錄（例：`App\Admin\Resources\Articles`、`app/Admin/Resources/Articles/`）
- 單一檔案路徑（例：`app/Admin/Resources/Articles/Schemas/ArticleForm.php`）

若 `$ARGUMENTS` 為空，預設檢查目前 git 變更中（`git status`）涉及的 Filament 檔案。

## 執行流程

### 1. 載入規範
- 讀取上方「規範來源」指定的 `filament-run/SKILL.md`
- 將其中每一條規範整理成一份**檢查清單**（逐條，含該條的正確做法與禁止事項）

### 2. 蒐集範圍
- 用 Glob/Read 工具列出 `$ARGUMENTS` 範圍下所有相關的 `.php` 檔案，不要用 bash 的 find/grep
- 完整讀取每個檔案，**不要只看摘要**
- 識別每個檔案的角色：Resource / Page / RelationManager / Schema（Form）/ Table / Filter / Widget
- 依檔案角色，套用步驟 1 清單中適用的規範條目

### 3. 逐條比對
- 拿步驟 1 的檢查清單，逐條對照步驟 2 讀到的程式碼
- 每條規範標記：符合 / 違反 / 不適用（該角色用不到此條）
- 違反者記錄 `file_path:line_number` 與違反的規範條目

### 4. 出報告
報告格式（中文）：
1. **範圍**：列出檢查了哪些檔案與其角色
2. **逐條結果**：依 `filament-run` 規範條目分節，每個違反點列出
   - `file_path:line_number` + 違反了哪一條規範
   - 正確做法（引用該條規範的要求）
3. **整體評估**：合格 / 有違反需修正
4. **修正建議**：每個違反點對應一個具體可行的修正方向（描述方向即可，不要直接貼整段程式碼）

### 5. 與使用者討論
- 若全數合格 → 簡短說明即可，不需找碴湊問題
- 若有違反 → **不要直接動手改**，先呈現報告與修正建議，等使用者決定
- 違反項較多時用 AskUserQuestion 讓使用者選擇要先處理哪些 / 用哪個方向

## 注意事項
- 永遠用中文回覆
- **規範一律即時讀 `filament-run/SKILL.md`**，不要在本 skill 內維護一份規範副本
- 只做檢查與討論，**禁止在這個 skill 內直接修改程式碼**；要修改請等使用者授權後切換到 `run` 或 `fix` skill
- 對 Filament 框架本身要求的固定結構（Resource/Page/Schema 三件套）不要誤判為違反
- 若 `$ARGUMENTS` 不清楚（找不到對應檔案、命名空間有多義），先用 AskUserQuestion 確認範圍再開始
