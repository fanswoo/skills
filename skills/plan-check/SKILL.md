---
name: plan-check
description: Check a development plan on the issue tracker. Use when the user asks to check or review a plan, or when another skill needs a plan checked before it runs.
---

# Check a Dev Plan

## What To Check
`$ARGUMENTS` names the plan to check. It may be a single ticket file or a whole plan folder. Read `docs/agents/issue-tracker.md` for where plans live and what the files look like. If `$ARGUMENTS` is empty or matches nothing there, ask the user which plan to check before you start.

Read the whole plan first — `spec.md`, then the tickets in `01-`, `02-` order. Every check below is judged against the current code, so open the files and classes the tickets name as you go.

## Run Flow

### 1. Check the risk
- Could the changes in the plan break a feature that already works?
- Are there missing edge cases or missing error handling?
- Are there hidden risks in speed or safety?
- **Drift**: do the files, classes, and namespaces in each ticket's Files touched really exist in the current code, and do they still look the way the ticket says?

### 2. Check that it is runnable
A runnable plan can be handed to `plan-run` and done as written, with nothing left to decide.

- Does each ticket hold all four parts — what to build, check items, files touched, run steps — plus its `Blocked by` and `Status` lines?
- Are the check items there, and can each one really be tested? `plan-run` starts its tests from them, so a ticket without them cannot be run.
- Can each run step be done as written — the action, the tech detail, and the wanted result all specific enough to act on right away?
- **Open items**: does the plan still carry "option A/B", "suggest XXX", "may consider", or any other undecided wording? A plan is made to be run, not to be talked over. Settle each one in step 6.
- Does the plan describe the work in prose only? A code block in a ticket is a finding — ask for it to be written back as prose.
- Where it fits, does the plan use PHP Attribute and Laravel Attribute in place of the old way?
- Does `spec.md` carry a file path or a class name anywhere? Paths belong in Files touched and nowhere else.
- Is the folder and every file named the way `issue-tracker.md` says, with zero-padded numbers?

### 3. Check the slicing
The tickets should be **tracer bullets** — thin slices that each prove the whole path works.

- Is each ticket a vertical slice through every layer, or a horizontal slice of one layer? `01-migration`, `02-model`, `03-controller` is the shape to catch.
- Is each ticket demoable or verifiable on its own?
- Does any ticket hold more than one fresh context window of work? Ask for it to be split, and set the blocking edges again.
- Does every `Blocked by` entry really gate the ticket, or is it just the order someone imagined? A blocker that does not gate costs parallel work.
- Is any blocking edge missing — a ticket that would land red because something it needs has not been built yet?
- Is a **wide refactor** hiding inside a ticket? A mechanical change with a wide blast radius cannot land green as one slice; ask for it to be sequenced as expand–contract.

### 4. Check the SOLID/IoC rules
Call the `solid-check` skill for this part, so the design rules live in one place.

- Pull out every class, namespace, or file path the plan will touch, and pass them to `solid-check` as its scope. If the plan touches scopes that are far apart, call it once for each scope.
- If the plan makes a class that is not in the code yet, pass the closest parent namespace or folder, and judge the planned design by the same rules.
- Paste the report that comes back into section 4 of your own report, as it came.
- A break here is a plan edit: fold the fix into the ticket. The code stays untouched until `plan-run`.

**Done when**: every ticket in scope carries an explicit verdict on all four checks, and every class or path the plan names has been looked up in the current code. A check you did not mention is a check you did not run.

### 5. Make a report, then set the status
1. **Scope**: the tickets you checked, in the plan order.
2. **One section per check** — risk, runnable, slicing, SOLID/IoC — each listing
   - Where it is: the ticket plus the step it sits in, and the code path it clashes with
   - What is wrong, and which way to change the ticket
3. **Status**: write the `Status:` line of each ticket, using the state machine in `issue-tracker.md`.
   - `ready-for-agent` — can be handed to `plan-run` as written
   - `needs-info` — waiting on the user to settle something in step 6
   - `needs-triage` — must be rewritten before it runs
   - `ready-for-human` — sound, but an agent should not run it: it needs judgment, external access, or manual testing

### 6. Settle the open items
Every open item steps 1-4 turned up — vague, undecided, or at odds with the current code — is settled with the user here, before the plan is handed back.

- Call the `grilling` skill and walk through each open item, one question at a time, until you both hold the same understanding.
- Call the `domain-modeling` skill while you grill. Put each settled term into `CONTEXT.md`, and record a hard-to-undo decision as an ADR under `docs/adr/`.
- Write the answers back into the ticket, then move its `Status:` line if the verdict changed.

**Done when**: every ticket is left with clear run steps, no open items, and a `Status:` line that matches what you found.
