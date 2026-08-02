# Vue / TypeScript Yardstick

The JS/TS-side check points for [`solid-check`](SKILL.md), applied on top of the questions in `SKILL.md` step 2.

## SRP
- Size smells: a Vue SFC `<script>` over 250 lines, or a composable exporting more than 6 values.
- Do the template / script / style hold unrelated logic? Should a child component or a composable come out?

## OCP
- Extension points in JS/TS: a callback, a handler map, or a plugin object.

## LSP
- Does an implementation use `as`, `any`, `as unknown as`, or `@ts-ignore` to force its way past a contract it does not honour?

## ISP
- Do the component props hold many fields that only a few branches use? Should they split into a separate component, or into several prop groups?

## DIP
- Does the composable or module import `axios`, `fetch`, `localStorage`, `window`, or a given store directly, with nothing injected through a parameter or `inject`?

## IoC / container use
- Is shared state across layers passed through `provide` / `inject` or a Pinia store, rather than a module-level singleton sneaking through?
- Does the composable return a factory rather than a module-scope shared instance, so SSR and multi-instance runs stay clean?
- Do components state their dependencies through props / emits / inject, rather than a global event bus or a `window` variable?
- SSR safety: can module-scope state taint another request?

## Extra checks
- Should the Vue SFC split into two layers, presentational + container?
- Are many `ref` / `reactive` / `watch` spread across `<script setup>`? They belong in a composable.
- Does the composable name start with `use`, and is its return shape stable and easy to destructure?
- Do side effects (fetch, subscribe, timer) pair with a cleanup (`onUnmounted` / `onScopeDispose`)?
- Does a store (Pinia or homemade) tie plain compute logic into store state, where a plain function would do?
