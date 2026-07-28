---
name: plan-run
description: Run a development plan. Use this when the user says "run the plan", "start the xxx plan", or talks about running a plan file in the storage/plans directory.
---

# Development Plan
## Plan
Read the `$ARGUMENTS.md` file or folder. Read it with care and run the development plan.

## Run Flow
Run this flow once for each plan file, in the plan order.

### 1. Build it test-first
Call the `tdd` skill and work in the red → green loop.
- Agree the seams with the user before you write the first test. The "check items" in the plan are a good place to start
- One seam, one test, one small build, then repeat. Do not write all the tests first
- Leave refactoring out of the loop. It belongs to step 3

### 2. Keep the checks running
- Run the type check often, and often run the one test file you are working on
- Run the whole test suite once when the plan file is done, not after every step

### 3. Review the plan file
When a plan file is done, review it on two sides before you move to the next one.
- Call the `code-review` skill. Give it a fixed point, the commit this plan file started from, and hand it the plan file as the spec, so it can check the work really did what the plan asked
- Call the `solid-check` skill on the classes or namespaces this plan file touched, for the design side
- Fix the breaks this plan brought in before you move on to the next plan file
- For a break that was already there before the plan, tell the user and ask first. Do not fix it on your own

## Development Plan Notes
- When you write code, take care that it is easy to test, easy to read, and low coupled
- When it fits, use PHP Attribute and Laravel Attribute in place of old ways
- Study the current code with care before you make changes
- When you build the development plan, make sure this plan does not break any current feature
- Never assume you know what the user wants. If anything is not clear, ask first before you act.
- Always make sure the code follows the SOLID/IoC rules. Do not write the rules here. Step 3 of the run flow does this check
- If the plan has many `*.md` files, run these plans one by one in the plan order