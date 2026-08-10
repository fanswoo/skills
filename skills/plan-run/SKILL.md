---
name: plan-run
description: Run a development plan from the issue tracker. Use this when the user says "run the plan", or names a plan folder or ticket to start.
---

# Run a Development Plan
## Plan
`$ARGUMENTS` names the plan to run. It may be a single ticket file or a whole plan folder. Read `docs/agents/issue-tracker.md` for where plans live and what the files look like.

Read `spec.md` and every ticket before you write anything, so you know what the later tickets expect from this one, and note the commit the plan starts from — every ticket is reviewed against it. A ticket with no **check items** cannot be run; call the `plan-check` skill on it first.

## Briefing
Call the `briefing` skill once, before you claim the first ticket. Its span is every ticket you are about to run:

- the steps are the tickets in the order you will run them
- the seams are the ones `spec.md` and the check items settled
- the fixed point is the commit the plan starts from
- the open items are whatever `spec.md` and the tickets leave undecided, plus which plan to run when `$ARGUMENTS` matched nothing on the tracker

## Run Flow
Work the **frontier**: the tickets that are `ready-for-agent` and whose every `Blocked by` entry is already `resolved`. When more than one qualifies, take the lowest number. Finish one ticket and open the next in the same run — the frontier carries you to the end of the plan, and finishing a ticket can unblock several.

### 1. Claim the ticket
Set its `Status:` line to `claimed` and save the file before you touch any code.

### 2. Build it
Call the `run` skill with this ticket as the task, telling it **briefing held** and handing over what the plan's briefing settled. Its "Run steps" are the work, its "Files touched" are the scope, and its debrief folds into the plan's.

At `run`'s review step, the fixed point is the commit the plan started from, and the spec is `spec.md` plus the tickets resolved so far, so the diff and the spec cover the same work.

### 3. Walk the check items
Take the check items from this ticket one by one and verify each one really passes.
- Name the test or run that covers each check item, so nothing is marked done on a guess
- A check item that will not pass means the ticket is not done. Fix it; when no fix inside this ticket's scope will make it pass, set its `Status:` line to `ready-for-human`, name the failing item in the file, and work the frontier again — the plan carries on without it and the ticket lands in the debrief

### 4. Resolve the ticket
Set its `Status:` line to `resolved`, then work the frontier again.

**Done when**: every check item in the ticket carries a named test or run that verifies it, `run`'s review came back clean, and the ticket reads `resolved`.

## Debrief
When the frontier runs dry, call the `debrief` skill to close the plan.
