---
name: fix-testing
description: Fix tests. Use this when the user says "test failed", "fix test", or needs to fix the tests for a given class.
---

# Fix Tests
## Need
Fix the tests for the `$ARGUMENTS` class

## Rules
- When you write code, take care that it is easy to test, easy to read, and low coupled
- First run the test command once. Find where the test fails with care, then clearly list the cause of the error. You must confirm the problem before you make changes.
- Unless it is needed, do not change the testing code itself. Change the test content instead.
- Do not run many tests at the same time, so the tests do not get in each other's way

## Tips
- Use context7 to look up how laravel v11 and dusk work, so you do not use a method that does not exist
- The back office uses filament v5. If you need to change back office tests, look up how filament v5 works.
