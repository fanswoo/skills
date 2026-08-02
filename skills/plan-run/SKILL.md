---
name: plan-run
description: Run a development plan under docs/plan. Use this when the user says "run the plan", or names a plan file or plan folder to start.
---

# Run a Development Plan
## Plan
`$ARGUMENTS` names the plan to run. It may be a single `*.md` plan file or a whole plan folder, and it is looked up under `./docs/plan/`. If `$ARGUMENTS` is empty or matches nothing there, ask the user which plan to run before you start.

Read the whole plan before you write anything, so you know what the later files expect from this one, and note the commit the plan starts from — every plan file is reviewed against it. A plan file with no **check items** cannot be run; call the `plan-check` skill on it first.

## Run Flow
Run this flow once for each plan file, in the `01-`, `02-` order, and finish one file before you open the next.

### 1. Build the plan file
Call the `run` skill with this plan file as the task. Its "Run steps" are the work, its "Files touched" are the scope.
- The plan file's check items are where `tdd`'s seams come from
- At `run`'s review step, the fixed point is the commit the plan started from, and the spec is the plan files run so far, so the diff and the spec cover the same work

### 2. Walk the check items
Take the check items from this plan file one by one and confirm each one really passes.
- Name the test or run that covers each check item, so nothing is marked done on a guess
- A check item you cannot confirm means the plan file is not done. Fix it, or tell the user and ask first

**Done when**: every check item in the file carries a named test or run that confirms it, and `run`'s review came back clean.
