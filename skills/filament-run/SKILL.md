---
name: filament-run
description: Notes for Filament work. Use it on its own when you write or change Filament code, to keep the project's own Filament ways. It covers the RelationManager writing rule (use the $relatedResource way and do not write any body content in the RelationManager).
---

# Notes for Filament Work

## Component Use Rules

1. **Do not use `Placeholder`** — use `TextEntry` instead. When you need to show read-only info, use `TextEntry`, not `Placeholder`.

## Resource Placement Rules

2. **Do not make nested Resources folders** — every class that extends `Filament\Resources\Resource` can only sit under `App\Admin\Resources` or `App\UserCenter\Resources`. You may not make another `Resources/` folder inside a sub-folder. For example, a nested shape like `App\Admin\Resources\Quotations\Resources\QuotationItems\Resources\` is not allowed.

## Action Hook Rules

3. **Do not put hooks on an Action** — buttons like `EditAction`, `DeleteAction`, `CreateAction` may not have `before()`, `after()`, `successRedirectUrl()` or other hooks. If you need hook logic, write it in the matching Page class (like `EditPage`, `CreatePage`), using the Page methods such as `afterSave()`, `afterCreate()`, `afterDelete()`.

## Activity Log Rules

4. **Activity Log uses the pxlrbt package** — when you need an activity log, use the `\pxlrbt\FilamentActivityLog\Pages\ListActivities` class. Do not write your own activity log page.

## Filament v5 Namespace Rules

5. **Actions must use the `Filament\Actions` namespace** — in Filament v5 all Action classes (`EditAction`, `DeleteAction`, `CreateAction`, `ViewAction`, `BulkAction`, and so on) sit under `Filament\Actions\`. **Do not use** the old namespaces like `Filament\Tables\Actions\` or `Filament\Forms\Actions\`; they no longer exist in v5.

## Select Component Rules

6. **A `Select` component should have a default value** — when you use `Select`, set a default value with `default()` to avoid a blank, not-picked state. If a default value does not fit the case (for example, the user must pick on their own), then add `selectablePlaceholder()` to clearly give a placeholder option to pick.

---

# Filament RelationManager Writing Rule

## Core Rule

**A RelationManager may only hold property declarations. It must never have any method or logic. All content (form, table, Actions, filters, and so on) must be set in the Resource that `$relatedResource` points to, and under its Schemas/, Tables/ folders.**

## Required Properties

Every RelationManager must set these properties:

```php
protected static ?string $relatedResource = SomeResource::class; // required: points to the matching Resource
protected static string $relationship = 'relationshipName';       // required: the Eloquent relationship name
protected static ?string $title = 'Display Name';                 // optional: the UI title
```

## The Only Allowed Form

A RelationManager may only take the form below. It may not have any extra method:

```php
<?php

namespace App\Admin\Resources\SomeParent\RelationManagers;

use App\Admin\Resources\SomeChild\SomeChildResource;
use Filament\Resources\RelationManagers\RelationManager;

class SomeChildrenRelationManager extends RelationManager
{
    protected static ?string $relatedResource = SomeChildResource::class;

    protected static string $relationship = 'someChildren';

    protected static ?string $title = 'Child Items';
}
```

## Strict Do-Not List

Inside a RelationManager, these are **never allowed**:

1. **Do not set a `table()` method** — table setup (columns, Actions, filters) must be set under the `Tables/` folder of the matching `$relatedResource`.
2. **Do not set a `form()` method** — the form must be set under the `Schemas/` folder of the matching `$relatedResource`.
3. **Do not set any Actions** — headerActions, recordActions must be set in the Table setup of the Resource.
4. **Do not write business logic** — all business logic should sit in a Service, a Repository, or the mutate method of a Form Schema.
5. **Do not set a query change (modifyQueryUsing)** — query changes should be handled in the Table setup of the Resource.
6. **Do not leave out `$relatedResource`** — every RelationManager must have a matching Resource.
7. **Do not set any method** — a RelationManager may only have property declarations.

## Where Logic Should Go

| Logic Type | Right Place |
|---------|---------|
| Form fields | `Schemas/SomeForm.php` of the `$relatedResource` |
| Table columns | `Tables/SomeTable.php` of the `$relatedResource` |
| headerActions / recordActions | `Tables/SomeTable.php` of the `$relatedResource` |
| Filters | the `Filters/` folder of the `$relatedResource` |
| Data change | the static `mutateFormDataBeforeCreate()` method of the Form Schema |
| Business logic | a Service or Repository class |

## Checklist

Before you write a RelationManager, make sure:
- [ ] The matching Resource class already exists (if not, make it first).
- [ ] The Form Schema of the Resource is set in the `Schemas/` folder.
- [ ] The Table of the Resource is set in the `Tables/` folder (with the Actions it needs).
- [ ] The RelationManager has **only** property declarations, no method at all.
