---
name: filament-run
description: Filament 開發注意事項。當撰寫或修改 Filament 相關程式碼時自動套用，確保遵循專案特定的 Filament 慣例；包含 RelationManager 撰寫規範（使用 $relatedResource 模式且禁止在 RelationManager 中撰寫任何內容）
---

# Filament 開發注意事項

## 元件使用規範

1. **禁止使用 `Placeholder`** — 應使用 `TextEntry` 取代。當需要顯示唯讀資訊時，使用 `TextEntry` 而非 `Placeholder`。

## Resource 放置規範

2. **禁止建立巢狀 Resources 資料夾** — 所有繼承 `Filament\Resources\Resource` 的類別只能放在 `App\Admin\Resources` 或 `App\UserCenter\Resources` 目錄下，不可在子資料夾中再建立 `Resources/` 目錄。例如 `App\Admin\Resources\Quotations\Resources\QuotationItems\Resources\` 這樣的巢狀結構是禁止的。

## Action 鉤子規範

3. **禁止在 Action 上掛載鉤子** — `EditAction`、`DeleteAction`、`CreateAction` 等按鈕上不可撰寫 `before()`、`after()`、`successRedirectUrl()` 等鉤子。如果需要鉤子邏輯，應寫在對應的 Page 類別中（如 `EditPage`、`CreatePage`），使用 Page 提供的 `afterSave()`、`afterCreate()`、`afterDelete()` 等方法。

## Activity Log 規範

4. **Activity Log 使用 pxlrbt 套件** — 需要實作 activity log 時，應使用 `\pxlrbt\FilamentActivityLog\Pages\ListActivities` 類別，不可自行實作 activity log 頁面。

## Filament v5 Namespace 規範

5. **Actions 必須使用 `Filament\Actions` namespace** — Filament v5 中所有 Action 類別（`EditAction`、`DeleteAction`、`CreateAction`、`ViewAction`、`BulkAction` 等）統一位於 `Filament\Actions\` 下。**禁止使用** `Filament\Tables\Actions\`、`Filament\Forms\Actions\` 等舊版 namespace，這些在 v5 中已不存在。

## Select 元件規範

6. **`Select` 元件應給予預設值** — 使用 `Select` 時應以 `default()` 設定預設值，避免出現空白未選取狀態。若情境不適合給預設值（例如必須由使用者主動選擇），則應改加上 `selectablePlaceholder()`，明確提供可選的 placeholder 選項。

---

# Filament RelationManager 撰寫規範

## 核心原則

**RelationManager 只能包含屬性宣告，絕對禁止撰寫任何方法或邏輯。所有內容（表單、表格、Actions、篩選器等）都必須定義在 `$relatedResource` 指向的 Resource 及其 Schemas/、Tables/ 目錄下。**

## 必要屬性

每個 RelationManager 必須定義以下屬性：

```php
protected static ?string $relatedResource = SomeResource::class; // 必要：指向對應 Resource
protected static string $relationship = 'relationshipName';       // 必要：Eloquent 關聯名稱
protected static ?string $title = '顯示名稱';                      // 選填：UI 顯示標題
```

## 唯一允許的寫法

RelationManager 只允許以下形式，不得有任何額外方法：

```php
<?php

namespace App\Admin\Resources\SomeParent\RelationManagers;

use App\Admin\Resources\SomeChild\SomeChildResource;
use Filament\Resources\RelationManagers\RelationManager;

class SomeChildrenRelationManager extends RelationManager
{
    protected static ?string $relatedResource = SomeChildResource::class;

    protected static string $relationship = 'someChildren';

    protected static ?string $title = '子項目';
}
```

## 嚴格禁止事項

在 RelationManager 中**絕對禁止**以下行為：

1. **禁止定義 `table()` 方法** — 表格設定（欄位、Actions、篩選器）必須在 `$relatedResource` 對應的 Tables/ 目錄下定義
2. **禁止定義 `form()` 方法** — 表單必須在 `$relatedResource` 對應的 Schemas/ 目錄下定義
3. **禁止定義任何 Actions** — headerActions、recordActions 必須在 Resource 的 Table 設定中定義
4. **禁止撰寫業務邏輯** — 所有業務邏輯應放在 Service、Repository 或 Form Schema 的 mutate 方法中
5. **禁止定義查詢修改（modifyQueryUsing）** — 查詢修改應在 Resource 的 Table 設定中處理
6. **禁止沒有 `$relatedResource`** — 每個 RelationManager 都必須有對應的 Resource
7. **禁止定義任何方法** — RelationManager 只能有屬性宣告

## 邏輯應放置的位置

| 邏輯類型 | 正確位置 |
|---------|---------|
| 表單欄位 | `$relatedResource` 的 `Schemas/SomeForm.php` |
| 表格欄位 | `$relatedResource` 的 `Tables/SomeTable.php` |
| headerActions / recordActions | `$relatedResource` 的 `Tables/SomeTable.php` |
| 篩選器 | `$relatedResource` 的 `Filters/` 目錄 |
| 資料變換 | Form Schema 的靜態 `mutateFormDataBeforeCreate()` 方法 |
| 業務邏輯 | Service 或 Repository 類別 |

## 檢查清單

撰寫 RelationManager 前，確認：
- [ ] 對應的 Resource 類別已存在（若不存在，先建立）
- [ ] Resource 的 Form Schema 已定義在 `Schemas/` 目錄
- [ ] Resource 的 Table 已定義在 `Tables/` 目錄（包含所需的 Actions）
- [ ] RelationManager 中**只有**屬性宣告，沒有任何方法
