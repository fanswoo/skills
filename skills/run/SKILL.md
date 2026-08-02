---
name: run
description: Run a dev task — "run xxx", "change xxx", "build xxx", "implement xxx", or a spec or set of tickets to implement. To find and fix a bug for a given error, use the fix skill instead.
---

# Run a Dev Task

`$ARGUMENTS` is the task — described inline, or pointing at the spec or set of tickets that holds it. If you cannot tell from it what the finished work should look like, ask the user before you start.

Tell the user how you plan to run the task, then run the flow.

## 1. Build it test-first

Call the `tdd` skill and hold its red → green loop.

Name the project from its root marker, then pick the tools from the language of the code you are editing. A task that touches both sides of a Laravel project takes both Laravel rows.

| Project | You edit | Test with | Keep running |
|---|---|---|---|
| Laravel — `artisan` in the root | PHP | Pest — call the `pest-testing` skill for how each test is written | Larastan / PHPStan, Pint |
| Laravel — `artisan` in the root | Vue / TypeScript / JavaScript | Vitest | the type check |
| Flutter — `pubspec.yaml` in the root | Dart | `flutter test` | `flutter analyze` |

On every slice, run the one test file you are working on and the checks in the last column. Run the whole suite once, at the end.

## 2. Review the work

When the build is done, review it on two sides, and carry the fixes yourself.

- `code-review` — the standards side and the spec side. Give it a fixed point: the commit your work started from, or the merge-base with the main branch
- `solid-check` — the design side, on the classes or namespaces you touched

Both stop at a report and hand it back to you. Work each report to empty:

- A break your own change brought in — take the fix, carry it out, then run that check again
- A break that was already there before you started — tell the user and ask before you touch it

Done when every finding is accounted for: fixed, or raised with the user.

## House rules

- When it fits, use PHP Attribute and Laravel Attribute in place of the old way
- Remove every class, variable, and function your change orphaned
- Build and commit belong to the dev person. Leave the work uncommitted and tell them it is ready; when a build is truly needed, remind them to run it themselves (`npm run build`)
