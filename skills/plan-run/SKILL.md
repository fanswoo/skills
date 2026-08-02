---
name: plan-run
description: Run a development plan from the issue tracker. Use this when the user says "run the plan", or names a plan folder or ticket to start.
---

# Run a Development Plan
## Plan
`$ARGUMENTS` names the plan to run. It may be a single ticket file or a whole plan folder. Read `docs/agents/issue-tracker.md` for where plans live and what the files look like. If `$ARGUMENTS` is empty or matches nothing there, ask the user which plan to run before you start.

Read `spec.md` and every ticket before you write anything, so you know what the later tickets expect from this one, and note the commit the plan starts from — every ticket is reviewed against it. A ticket with no **check items** cannot be run; call the `plan-check` skill on it first.

## Run Flow
Work the **frontier**: the tickets that are `ready-for-agent` and whose every `Blocked by` entry is already `resolved`. When more than one qualifies, take the lowest number. Finish one ticket before you open the next, and work the frontier again after each one — finishing a ticket can unblock several.

### 1. Claim the ticket
Set its `Status:` line to `claimed` and save the file before you touch any code.

### 2. Build it
Call the `run` skill with this ticket as the task. Its "Run steps" are the work, its "Files touched" are the scope.
- The ticket's check items are where `tdd`'s seams come from, and `spec.md` says which seams the plan settled on
- At `run`'s review step, the fixed point is the commit the plan started from, and the spec is `spec.md` plus the tickets resolved so far, so the diff and the spec cover the same work

### 3. Walk the check items
Take the check items from this ticket one by one and confirm each one really passes.
- Name the test or run that covers each check item, so nothing is marked done on a guess
- A check item you cannot confirm means the ticket is not done. Fix it, or tell the user and ask first

### 4. Resolve the ticket
Set its `Status:` line to `resolved`, then work the frontier again.

**Done when**: every check item in the ticket carries a named test or run that confirms it, `run`'s review came back clean, and the ticket reads `resolved`.
