---
name: fix
description: Fix a bug — prove the cause, carry the smallest fix, leave it green. Use when the user says "fix" or "debug", reports an error, a stack trace, or wrong behaviour, or when another skill hands a regression back to the code. For a test that went red, use the fix-testing skill instead.
---

# Fix a Bug

`$ARGUMENTS` names what is broken — an error message, a stack trace, a class, or a description of the wrong behaviour. When you cannot tell from it what the right behaviour would be, ask the user before you start.

## 1. Prove the cause

Call the `diagnosing-bugs` skill and hold its loop: one command you have already run that goes **red** on this bug and will go green once it is fixed.

Then name the cause — the line the symptom comes from and why it produces it — and quote the code or the diff that proves it. A cause the red loop has not confirmed is a guess; keep digging until the two agree.

Done when you can paste the red command with its output and state the cause in one sentence.

## 2. Carry the smallest fix

Turn the repro into a regression test where a seam reaches the real bug, watch it go red, then fix. When no such seam exists, say so and fix without one.

Hold to the cause you named — a bug fix stays small. What you notice on the way that is not this bug goes to the user as a note, not into the diff.

Name the project from its root marker and keep its checks green alongside the tests, after every change.

| Project | Keep running |
|---|---|
| Laravel — `artisan` in the root | Larastan / PHPStan, Pint; the type check when you touched Vue / TypeScript |
| Flutter — `pubspec.yaml` in the root | `flutter analyze` |

Done when the loop that was red is green, the whole suite has run green once, and every class, variable, and function your change orphaned is gone.

## 3. Review what you touched

Call `solid-check` on the classes or namespaces you touched, and work its report to empty.

- A break your own fix brought in — carry the fix, then run the check again
- A break that was there before you started — tell the user and ask; a bug fix stays small, so the refactor is theirs to call

Done when every finding is accounted for: fixed, or raised with the user.

## House rules

- When it fits, use PHP Attribute and Laravel Attribute in place of the old way
- The back office runs Filament v5; look its API up before you change back-office code
- Look a package up with context7 before you reach for a method you have not already seen in this codebase
- Build and commit belong to the dev person. Leave the work uncommitted and tell them it is ready; when a build is truly needed, remind them to run it themselves (`npm run build`)
