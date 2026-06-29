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
- Do not run many tests at the same time, so the tests do not get in each other's way

## Find the Real Reason First (Most Important)
A test breaks for one of two reasons. You must use git to tell them apart before you change anything.

1. Look at the recent code change with git. Use these to see what changed:
   - `git status` and `git diff` for work not yet saved
   - `git log -p -n 5` and `git show <commit>` for the last few commits
2. Read the broken test. Find which code it calls. Then compare that code with the git change.
3. Decide the reason, then act:
   - **The test is out of date.** The code change is right, but the test still checks the old way (old method name, old field, old return shape). Then fix the test so it fits the new code.
   - **The code change is wrong.** The test is right, and it found a real bug the change made. Then fix the code, not the test. Changing the test here would hide a real bug.
4. Say out loud which of the two reasons it is, and show the git proof, before you make changes.
5. When you are not sure which reason it is, stop and ask the user first. Do not guess.

## Tips
- Use context7 to look up how laravel v11 and dusk work, so you do not use a method that does not exist
- The back office uses filament v5. If you need to change back office tests, look up how filament v5 works.
