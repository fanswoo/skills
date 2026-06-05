---
name: filament-check
description: Check that the given Filament code follows the filament-run rules. Use this when the user says "check filament", "filament check", "check if xxx follows the filament way", "review a Filament Resource/RelationManager", or needs to confirm that Filament code follows the rules.
---

# Check Filament Rules

Check that the code in the given range follows the project's Filament ways, as listed in the `filament-run` skill.

## Rule Source (Single Source Of Truth)

**These rules always come from the `filament-run` skill's `SKILL.md`. Read it live each run. This makes sure that when `filament-run` changes, this check follows on its own and stays right.

The first step must read this file:

```
/var/www/skills/skills/filament-run/SKILL.md
```

Each rule you read (component use, Resource placement, Action hooks, Activity Log, v5 Namespace, Select default value, RelationManager writing rules, and so on) is a check item for this run. **Use what is in that file right now.** Do not trust this skill's past memory or the examples below. If that file adds, changes, or drops items, your check items change with it.

## What To Check
`$ARGUMENTS`

`$ARGUMENTS` can be:
- The FQCN of one Filament class (like `App\Admin\Resources\Articles\ArticleResource`)
- A PHP namespace or folder (like `App\Admin\Resources\Articles`, `app/Admin/Resources/Articles/`)
- One file path (like `app/Admin/Resources/Articles/Schemas/ArticleForm.php`)

If `$ARGUMENTS` is empty, check the Filament files in the current git changes (`git status`) by default.

## Run Flow

### 1. Load The Rules
- Read the `filament-run/SKILL.md` named in "Rule Source" above
- Turn each rule in it into a **check list** (one by one, with the right way and the banned things for that rule)

### 2. Collect The Range
- Use the Glob/Read tools to list all the `.php` files in the `$ARGUMENTS` range. Do not use bash find/grep
- Read each file in full. **Do not read just a summary**
- Find the role of each file: Resource / Page / RelationManager / Schema (Form) / Table / Filter / Widget
- By the file role, apply the rule items from the step 1 list that fit

### 3. Match One By One
- Take the check list from step 1 and match it, one by one, against the code you read in step 2
- Mark each rule: pass / fail / not used (this role does not use this rule)
- For a fail, write down `file_path:line_number` and the rule it broke

### 4. Make A Report
Report form (in English):
1. **Range**: list which files you checked and their roles
2. **One By One Results**: split into parts by `filament-run` rule item. For each fail, list
   - `file_path:line_number` + which rule it broke
   - The right way (quote what that rule asks for)
3. **Overall Check**: pass / has fails that need a fix
4. **Fix Ideas**: give each fail one clear, doable fix direction (just describe the direction, do not paste a whole block of code)

### 5. Talk With The User
- If all pass → just say so in short. Do not look for small things to fill the report
- If there are fails → **do not change the code yourself**. First show the report and fix ideas, then wait for the user to choose
- When there are many fails, use AskUserQuestion to let the user pick which ones to do first / which direction to use

## Notes
- **Always read the rules live from `filament-run/SKILL.md`.** Do not keep a copy of the rules inside this skill
- Only check and talk. **Do not change any code inside this skill.** To change code, wait for the user to allow it, then switch to the `run` or `fix` skill
- Do not wrongly mark the fixed structure that the Filament framework itself needs (the Resource/Page/Schema set of three) as a fail
- If `$ARGUMENTS` is not clear (no file found, or the namespace has more than one meaning), use AskUserQuestion to confirm the range before you start
