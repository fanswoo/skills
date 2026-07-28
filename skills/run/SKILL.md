---
name: run
description: Do a dev task. Use it when the user says "run xxx", "change xxx", "build xxx", "fix xxx", or needs code changes that follow the SOLID/IoC rules.
---

# Dev Task
## Plan
`$ARGUMENTS`

## Run Flow

### 1. Build it test-first
Call the `tdd` skill and work in the red → green loop.
- Agree the seams with the user before you write the first test. Never test at a seam the user has not agreed to
- One seam, one test, one small build, then repeat. Do not write all the tests first
- Leave refactoring out of the loop. It belongs to step 3

### 2. Keep the checks running
- Run the type check often, and often run the one test file you are working on
- Run the whole test suite once at the end, not after every step

### 3. Review the work
When the build is done, review it on two sides.
- Call the `code-review` skill for the standards side and the spec side. Give it a fixed point: the commit your work started from, or the merge-base with the main branch
- Call the `solid-check` skill on the classes or namespaces you touched, for the design side
- Fix the breaks your own change brought in, then check again
- For a break that was already there before you started, tell the user and ask first. Do not fix it on your own

## Notes
- When you write code, pay special heed to how easy it is to test, how easy it is to read, and how low the coupling is.
- When it fits, try to use PHP Attribute and Laravel Attribute in place of the old way.
- Before you change anything, always study the current code with care.
- When you change code, always make sure this change does not break a feature that already works.
- After each code change, always make sure you leave no unused class, variable, or function behind.
- Never assume you know what the user wants. If anything is not clear, you must ask first before you run.
- Always make sure the code follows the SOLID/IoC rules. Do not write the rules here. Step 3 of the run flow does this check.
- Tell the run plan first, then run it.

### Important Build Note
The dev person will start the build mode on their own. You should never run the build on your own. If a build is truly needed, the most you may do is "remind the dev person to build".
```bash
npm run build
```
