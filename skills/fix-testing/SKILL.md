---
name: fix-testing
description: Fix a red test — one test failed, or a whole suite went red, under Pest, Vitest, or flutter test. For an error outside the test suite, use the fix skill instead.
---

# Fix a Red Test

`$ARGUMENTS` names what is red — a test file, a class, pasted failure output, or nothing at all. When it is nothing, run the suite and let the failures name themselves.

## 1. Reproduce

Name the project from its root marker, then pick the row by the language of the red test. A Laravel project that is red on both sides takes both Laravel rows.

| Project | Red test | Run one file | Also run |
|---|---|---|---|
| Laravel — `artisan` in the root | Pest (PHP) — call the `pest-testing` skill for how each test is written | `php artisan test --compact tests/Feature/FooTest.php` | Larastan / PHPStan, Pint |
| Laravel — `artisan` in the root | Vitest (Vue / TypeScript / JavaScript) | `npx vitest run resources/…/foo.test.ts` | `npm run types:check` |
| Flutter — `pubspec.yaml` in the root | Dart | `flutter test test/foo_test.dart` | `flutter analyze` |

Run the red test on its own and read the failure. One test file at a time, start to finish — tests run together share state, a database, a module cache, a filesystem, and a neighbour's leftovers will send you chasing the wrong failure.

Done when you can name the assertion that failed and the line of app code it reached.

## 2. Reach a verdict

A test goes red for one of two reasons, and git tells them apart. Read the recent change — `git status` and `git diff` for uncommitted work, `git log -p -n 5` and `git show <commit>` for what landed recently — then compare the code the test reaches against what changed there.

| Verdict | The evidence | What you fix |
|---|---|---|
| **Stale test** | The change deliberately moved what the test asserts on — renamed method, new field, different return shape | The test, so it asserts on the new shape |
| **Regression** | The change altered behaviour it never meant to, and the test caught it | The code — call the `fix` skill. Editing the test here buries a live bug |

State the verdict and quote the diff that proves it before you edit anything. When the evidence supports both readings, or neither, ask the user which it is.

Done when every red test carries a named verdict backed by a diff.

## 3. Carry out the fix

Take the verdicts one at a time, re-running that test file and its `Also run` checks after each. Run the whole suite once at the end.

Done when the suite is green and every test still asserts as much as it did before.

## House rules

- The back office runs Filament v5; look up its testing helpers before you change a back-office test
- Look a package up with context7 before you reach for a method you have not already seen in this codebase
