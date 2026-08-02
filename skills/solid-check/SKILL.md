---
name: solid-check
description: SOLID and IoC design review. Use when the user asks to check SOLID, judge the design quality of a class or namespace, or when another skill needs a design review of the code it touched.
---

# Check SOLID / IoC Rules

## What To Check
`$ARGUMENTS` — a class, namespace, file, or folder, in PHP (`App\AI\Tools\UpdateArticleTool`), JS/TS (`resources/script/stores/product-store.ts`, `useMessageScroll`), or Dart (`lib/features/order/`, `OrderRepository`).

If `$ARGUMENTS` is empty, check the files in the current git changes (`git status`).

If `$ARGUMENTS` is unclear (no matching file found, or a namespace with more than one meaning), take every candidate that matches into scope, and name in the report which ones you took.

## Run Flow

### 1. Gather the scope
- Use Glob and Read to list and open every file in scope (`.php` / `.ts` / `.tsx` / `.js` / `.vue` / `.dart`), and Grep to find its callers, inject points, child classes, and import sites.
- Read each file in full.
- Name the form: PHP class / Vue SFC / composable / store / plain TS module / Flutter widget / Bloc / Notifier / Dart repository.
- Read the yardstick for that form: PHP → [`PHP.md`](PHP.md), JS/TS → [`VUE-TS.md`](VUE-TS.md), Dart/Flutter → [`FLUTTER.md`](FLUTTER.md). A mixed scope reads each one that applies.

### 2. Run the six checks
Five SOLID principles plus IoC. Each carries language-specific check points in the yardstick you just read, where the language has them — apply both. Judge on how easy the design is to maintain, not on whether the code runs.

Before judging, take the exemptions. Two shapes earn `exempt` rather than a break:
- **By size** — a small Value Object, a plain DTO, or a single-use Action class is judged on whether it stays small and clear.
- **By framework** — a shape the framework dictates (Filament Resource, Nova Resource, Livewire Component, the three-part Vue SFC, the three-piece Pinia store, the `StatefulWidget` + `State` pair, a Bloc's event/state classes) is correct as it stands.

An exemption is still a verdict: mark the check `exempt` and name the shape that earned it.

#### Single Responsibility Principle (SRP)
- Does the class / module / composable have only one reason to change?
- Does it handle HTTP/validation/business logic/data access/formatting/UI state all at once?
- Are the public methods or exports about one idea, or do they mix unrelated jobs?
- Do the file length and the method count smell of too many jobs? (Thresholds live in the yardstick.)

#### Open-Closed Principle (OCP)
- When a new need comes, must you change the inside of an old class/module?
- Do you use `if/switch` to branch on a type/string, instead of polymorphism or a strategy map?
- Are the extension points shown, or does a caller have to reach inside to extend?

#### Liskov Substitution Principle (LSP)
- Does a child class / child type break the parent contract (throw an exception the parent did not declare, return null, tighten the pre-conditions)?
- Do the type declarations of an overridden method match the parent class/interface?
- Is there an `instanceof` / `typeof` / discriminator string and then branch handling for a given child type (an LSP-break smell)?

#### Interface Segregation Principle (ISP)
- Is a type forced to implement methods it does not use (empty method, a throw, a `undefined` return)?
- Should a "fat interface" or "fat props" be split into many small ones?
- Does the client / child component depend only on the methods or prop it needs?

#### Dependency Inversion Principle (DIP)
- Does a high-level module `new` a low-level concrete class, or `import` a concrete build directly?
- Is the type in the constructor inject or function parameter an abstract one or a concrete one?
- Is the outside I/O (HTTP, file, DB, browser API) hidden behind an abstract layer so a test can swap it out?

#### IoC / container use
Wholly language-specific — judge it from the yardstick for the form.

**Done when**: every file in scope carries an explicit verdict on all six checks — `pass`, `break`, or `exempt`. A check you did not mention is a check you did not run.

### 3. Make a report
1. **Scope** — the files you checked.
2. **Verdict table** — one row per check (SRP, OCP, LSP, ISP, DIP, IoC), each `pass` / `break` / `exempt`.
3. **One section per check that broke** — each break point as `file_path:line_number` + a short note, and the risk it carries (hard to test? coupling? painful to extend?).
4. **Overall judgment** — pass / room to improve / serious, needs refactor.
5. **Refactor advice** — each break point gets one clear, doable refactor direction, described in prose.

### 4. Close the check
This skill ends at the report — changing code belongs to the `run` or `fix` skill. When everything passes, say the good points in short and stop there.

When there are breaks, who called for the check decides where the report goes.
- **The user asked for it** → call the `grilling` skill to put the refactor directions to them one at a time and settle which way to go.
- **You or another skill asked for it** → the report is yours to reason from. Take it back to the work that called for the check and act on it there.
