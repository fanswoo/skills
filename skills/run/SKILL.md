---
name: run
description: Run a dev task — "run xxx", "change xxx", "build xxx", "implement xxx", or a spec or set of tickets to implement. To find and fix a bug for a given error, use the fix skill instead.
---

# Run a Dev Task

`$ARGUMENTS` is the task — described inline, or pointing at the spec or set of tickets that holds it.

## Briefing

Told **briefing held**, build on the answers that came with the task and open step 1. Otherwise call the `briefing` skill over this task, to settle three things:

- **The finished work** — what it looks like when it is done. This is the spec `code-review` judges against
- **The seams** — where the tests go. These are the seams `tdd` asks you to confirm, and settled here they hold for every slice
- **The fixed point** — the commit your work started from, or the merge-base with the main branch. The review runs against it

## 1. Build it test-first

Call the `tdd` skill and hold its red → green loop, writing every test at the seams the briefing settled.

Name the project from its root marker, then pick the tools from the language of the code you are editing. A task that touches both sides of a Laravel project takes both Laravel rows.

| Project | You edit | Test with | Keep running |
|---|---|---|---|
| Laravel — `artisan` in the root | PHP | Pest — call the `pest-testing` skill for how each test is written | Larastan / PHPStan, Pint |
| Laravel — `artisan` in the root | Vue / TypeScript / JavaScript | Vitest | the type check |
| Flutter — `pubspec.yaml` in the root | Dart | `flutter test` | `flutter analyze` |

On every slice, run the one test file you are working on and the checks in the last column. Run the whole suite once, at the end.

## 2. Review the work

When the build is done, review it on two sides, and carry the fixes yourself.

- `code-review` — the standards side and the spec side. Hand it both from the briefing: the fixed point, and the finished work as the spec it judges against
- `solid-check` — the design side, on the classes or namespaces you touched. You are the caller, so its report comes back to you: work it here

Both stop at a report and hand it back to you. Work each report to empty:

- A break your own change brought in — take the fix, carry it out, then run that check again
- A break that was already standing before you started — leave it standing and write it into the debrief; a refactor outside the task comes back later as its own task

## 3. Debrief

Call the `debrief` skill to close.

**Done when**: the whole suite has run green, and every finding from step 2 is either fixed or standing in the debrief.

## House rules

- When it fits, use PHP Attribute and Laravel Attribute in place of the old way
- Remove every class, variable, and function your change orphaned
- Build and commit belong to the dev person. Leave the work uncommitted; when a build is truly needed, remind them to run it themselves (`npm run build`)
