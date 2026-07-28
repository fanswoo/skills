---
name: plan-run
description: Run a development plan. Use this when the user says "run the plan", "start the xxx plan", or talks about running a plan file in the storage/plans directory.
---

# Run a Development Plan
## Plan
`$ARGUMENTS` names the plan to run. It may be a single `*.md` plan file or a whole plan folder, and it is looked up under `./storage/plans/`. If `$ARGUMENTS` is empty or matches nothing there, ask the user which plan to run before you start.

Go through the plan with care before you write anything.

## Run Flow
Run this flow once for each plan file, in the plan order.

### 1. Build it test-first
Call the `tdd` skill for the red → green loop, and the `pest-testing` skill for how each test is written.
- Agree the seams with the user before you write the first test. The "check items" in the plan are a good place to start
- One seam, one test, one small build, then repeat. Do not write all the tests first
- Leave refactoring out of the loop. It belongs to step 4

### 2. Keep the checks running
- Often run the one test file you are working on
- If the project has static analysis (Larastan / PHPStan) or a code style tool (Pint) set up, run them often too
- Run the whole test suite once when the plan file is done, not after every step

### 3. Walk the check items
Before you call a plan file done, take the "check items" from that plan file one by one and confirm each one really passes.
- Say which check item each test or run covers, so nothing is marked done on a guess
- If a check item cannot be confirmed, the plan file is not done. Fix it, or tell the user and ask first

### 4. Review the plan file
When a plan file is done, review it on two sides before you move to the next one.
- Call the `code-review` skill. Give it a fixed point, the commit this plan file started from, and hand it the plan file as the spec, so it can check the work really did what the plan asked
- Call the `solid-check` skill on the classes or namespaces this plan file touched, for the design side
- `solid-check` stops at its report and asks the user which way to refactor. Here that is the right place to answer: for a break this plan brought in, take the fix and carry it out before you move on to the next plan file
- For a break that was already there before the plan, tell the user and ask first. Do not fix it on your own

## Development Plan Notes
- When you write code, take care that it is easy to test, easy to read, and low coupled
- When it fits, use PHP Attribute and Laravel Attribute in place of old ways
- Study the current code with care before you make changes
- When you run the development plan, make sure this plan does not break any current feature
- Never assume you know what the user wants. If anything is not clear, ask first before you act.
- Always make sure the code follows the SOLID/IoC rules. Do not write the rules here. Step 4 of the run flow does this check
- If the plan has many `*.md` files, run these plans one by one in the plan order
