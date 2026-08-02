---
name: filament-check
description: Filament rules review. Use when the user asks to check Filament code against the project's Filament ways, or when another skill needs a Filament review of the code it touched.
---

# Check Filament Rules

## What To Check
`$ARGUMENTS` — a Filament class (`App\Admin\Resources\Articles\ArticleResource`), a namespace or folder (`App\Admin\Resources\Articles`, `app/Admin/Resources/Articles/`), or a single file (`app/Admin/Resources/Articles/Schemas/ArticleForm.php`).

If `$ARGUMENTS` is empty, check the Filament files in the current git changes (`git status`).

If `$ARGUMENTS` is unclear (no matching file found, or a namespace with more than one meaning), take every candidate that matches into scope, and name in the report which ones you took.

## Run Flow

### 1. Load the rules
Read [`../filament-run/SKILL.md`](../filament-run/SKILL.md) in full, live, this run. It is the single source of truth: every rule in it is a check item, and a rule it adds, changes, or drops changes your check items with it.

Turn each rule into a check list entry — what it asks for, and the shape that breaks it.

### 2. Gather the scope
- Use Glob and Read to list and open every `.php` file in scope.
- Read each file in full.
- Name the role: Resource / Page / RelationManager / Schema (Form) / Table / Filter / Widget.
- Apply the check list entries from step 1 that the role reaches.

### 3. Match one by one
Take the check list from step 1 against the code you read, and mark each rule `pass`, `break`, or `exempt`.

- **`break`** — write down `file_path:line_number` and the rule it broke.
- **`exempt`** — the role never reaches the rule, or the shape is one Filament itself dictates (the Resource / Page / Schema set of three). An exemption is still a verdict: name the shape that earned it.

**Done when**: every file in scope carries an explicit verdict on every rule you loaded in step 1. A rule you did not mention is a rule you did not run.

### 4. Make a report (in English)
1. **Scope** — the files you checked, and the role of each.
2. **Verdict table** — one row per rule from step 1, each `pass` / `break` / `exempt`.
3. **One section per rule that broke** — each break point as `file_path:line_number`, and what the rule asks for instead, quoted from `filament-run`.
4. **Overall judgment** — pass / has breaks that need a fix.
5. **Fix advice** — each break point gets one clear, doable fix direction, described in prose.

### 5. Close the check
This skill ends at the report — changing code belongs to the `run` or `fix` skill. When everything passes, say the good points in short and stop there.

When there are breaks, who called for the check decides where the report goes.
- **The user asked for it** → call the `grilling` skill to put the fix directions to them one at a time and settle which way to go.
- **You or another skill asked for it** → the report is yours to reason from. Take it back to the work that called for the check and act on it there.
