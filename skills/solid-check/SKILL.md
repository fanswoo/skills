---
name: solid-check
description: Check if a given namespace or class follows the SOLID and IoC rules. Use it when the user says "check SOLID", "solid check", "review if xxx follows SOLID", "check xxx namespace", or needs to judge the design quality of a given class/namespace.
---

# Check SOLID / IoC Rules

## What To Check
`$ARGUMENTS`

`$ARGUMENTS` can be:
- One class FQCN (for example: `App\AI\Tools\UpdateArticleTool\UpdateArticleTool`)
- A PHP namespace (for example: `App\AI\Tools\UpdateArticleTool`)
- A JavaScript/TypeScript module or folder (for example: `resources/script/components/message/`, `useMessageScroll`)
- A file or folder path (for example: `app/AI/Tools/UpdateArticleTool/`, `resources/script/stores/product-store.ts`)

Languages it supports: PHP (Laravel), JavaScript / TypeScript (Vue Composition API, composables, stores, plain function modules).

## Run Flow

### 1. Gather the scope
- Use the Glob/Read tools to list all the files in that scope (`.php` / `.ts` / `.tsx` / `.js` / `.vue`). Do not use bash find/grep.
- Read each file in full. **Do not read only a summary.**
- Use Grep to find where this class/module/function is used in the rest of the project (callers, inject points, child classes, import sides).
- First tell what kind of language form it is (PHP class / Vue SFC / composable / store / plain TS module). Each form has its own check points.

### 2. Check each item

#### Single Responsibility Principle (SRP)
- Does the class / module / composable have only one reason to change?
- Does it handle HTTP/validation/business logic/data access/formatting/UI state all at once?
- Are the public methods or exports about one idea, or do they mix unrelated jobs?
- Does the file length and method count hint at too many jobs? (PHP class >300 lines or >8 public methods, Vue SFC `<script>` >250 lines, composable export >6 values are warning signs.)
- Vue: do template / script / style hold too much unrelated logic? Should you pull out a child component or composable?

#### Open-Closed Principle (OCP)
- When a new need comes, must you change the inside of an old class/module?
- Do you use `if/switch` to branch on a type/string, instead of polymorphism or a strategy map?
- Are the extension points shown through an interface, abstract class, Strategy/Factory (PHP), or callback, handler map, plugin object (JS/TS)?

#### Liskov Substitution Principle (LSP)
- Does a child class / child type break the parent contract (throw an exception the parent did not declare, return null, tighten the pre-conditions)?
- Do the type declarations of an overridden method match the parent class/interface?
- Is there an `instanceof` / `typeof` / discriminator string and then branch handling for a given child type (an LSP-break sign)?
- TS: does an object that implements an interface use `as` or `any` to force its way through and hide the break?

#### Interface Segregation Principle (ISP)
- Is an interface / TS type forced to implement methods it does not use (empty method, `throw new BadMethodCallException`, return `undefined`)?
- Should a "fat interface" or "fat props" be split into many small interfaces?
- Does the client / child component depend only on the methods or prop it needs?
- Vue: do the component props hold many fields that only a few branches use? Should they be split into a separate component or many prop groups?

#### Dependency Inversion Principle (DIP)
- Does a high-level module `new` a low-level concrete class / `import` a concrete build directly?
- Is the type in the constructor inject or function parameter an abstract one or a concrete one?
- PHP: is there a hidden dependency through a facade, `app()`, `resolve()`, `static::`?
- JS/TS: does the composable / module import `axios`, `fetch`, `localStorage`, `window`, or a given store directly, with no inject through a parameter or `inject`?
- Is the outside I/O (HTTP, file, DB, browser API) hidden behind an abstract layer so a test can swap it out?

#### IoC / container use
- **PHP / Laravel**:
  - Is the service registered through a Service Provider? If it needs to be a singleton, does it use `singleton`?
  - Is there a service location anti-pattern (run-time `app(SomeService::class)` in place of constructor inject)?
  - Does the interface have a matching binding? Is the binding written in a sensible Provider?
  - Are contextual binding and tagged services used well?
- **Vue / TS**:
  - Is shared state across layers passed through `provide` / `inject` or a Pinia store, not a module-level singleton sneaking through?
  - Does the composable return a factory, not a module-scope shared instance (to avoid SSR/multi-instance clashes)?
  - Do components state their dependencies clearly through props / emits / inject, not a global event bus or `window` variable?
  - Watch out for SSR-safe: can module-scope state taint other requests? (See the [[feedback_use_store_inject_fallback]] pattern.)

#### Laravel / PHP extra checks
- Should you use a PHP Attribute or Laravel Attribute in place of the old way?
- Does an Eloquent Model hold query/service logic that is not its job?
- Has the controller become a "fat controller" (it should be pulled into an Action / Service / Form Request)?
- Is there a static method that holds state, a global variable, or a Singleton anti-pattern?

#### Vue / TypeScript extra checks
- Should the Vue SFC be split into two layers: presentational + container?
- Are many `ref` / `reactive` / `watch` spread across `<script setup>`? Should they go into a composable?
- Does the composable name start with `use`, and is its return shape stable and easy to destructure?
- Does it misuse `any`, `as unknown as`, `@ts-ignore` to dodge the type system?
- Do side effects (fetch, subscribe, timer) pair with a cleanup (`onUnmounted` / `onScopeDispose`)?
- Does a store (Pinia / homemade) tie plain compute logic into the store state, instead of pulling it into a plain function?

### 3. Make a report
Report layout:
1. **Scope**: list which files you checked.
2. **One section per principle**: each section lists
   - The break point: `file_path:line_number` + a short note
   - The risk: what problem the break may cause (hard to test? coupling? painful to extend?)
3. **Overall judgment**: pass / room to improve / serious, needs refactor.
4. **Refactor advice**: each break point gets one clear, doable refactor way (do not write code, just describe the direction).

### 4. Talk with the user
- If all pass → just say the good points in short. Do not dig for problems to fill the report.
- If there are breaks → **do not change the code right away**. First show the report and refactor advice to the user, and wait for the user to decide whether to refactor and which way.
- Use AskUserQuestion to list 2-4 ways (keep as-is / pull out an interface in part / split the whole thing / ...) for the user to pick.

## Notes
- Only check and talk. **Do not change any code inside this skill.** To change code, wait for the user's go-ahead and switch to the `run` or `fix` skill.
- Do not treat "the code runs" as the pass bar. The point is how easy the design is to maintain.
- For a small Value Object, a plain DTO, or a single-use Action class, do not force every principle. Judge with common sense.
- If `$ARGUMENTS` is not clear (no matching file found, the namespace has more than one meaning), first use AskUserQuestion to check the scope with the user before you start.
- For the fixed shapes that a framework asks for (Filament Resource, Nova Resource, Livewire Component, the three-part Vue SFC, the three-piece Pinia store), do not treat them as SRP breaks.
- "IoC" means different things in PHP and JS/TS: in PHP look at the Laravel container binding; in JS/TS look at provide/inject, Pinia, dependency-inject style composables. Do not mix the two yardsticks.
