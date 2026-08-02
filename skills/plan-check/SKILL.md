---
name: plan-check
description: Check a development plan under docs/plan. Use when the user asks to check or review a plan, or when another skill needs a plan checked before it runs.
---

# Check a Dev Plan

## What To Check
`$ARGUMENTS` names the plan to check. It may be a single `*.md` plan file or a whole plan folder, and it is looked up under `./docs/plan/`. If `$ARGUMENTS` is empty or matches nothing there, ask the user which plan to check before you start.

Read the whole plan first, and when it is a folder, take the files in the `01-`, `02-` order. Every check below is judged against the current code, so open the files and classes the plan names as you go.

## Run Flow

### 1. Check the risk
- Could the changes in the plan break a feature that already works?
- Are there missing edge cases or missing error handling?
- Are there hidden risks in speed or safety?
- **Drift**: do the files, classes, and namespaces the plan names really exist in the current code, and do they still look the way the plan says?

### 2. Check that it is runnable
A runnable plan can be handed to `plan-run` and done as written, with nothing left to decide.

- Does each plan file hold all four parts: goal, files touched, run steps, and check items?
- Are the check items there, and can each one really be tested? `plan-run` starts its tests from them, so a plan file without them cannot be run.
- Can each run step be done as written — the action, the tech detail, and the wanted result all specific enough to act on right away?
- **Open items**: does the plan still carry "option A/B", "suggest XXX", "may consider", or any other undecided wording? A plan is made to be run, not to be talked over. Settle each one in step 5.
- Does the plan describe the work in prose only? A code block in a plan file is a finding — ask for it to be written back as prose.
- Where it fits, does the plan use PHP Attribute and Laravel Attribute in place of the old way?
- Is the folder named in the `yymmdd-my-plan` form, each file named in the `01-my-step.md` form with a zero-padded number, and the run order clear across the files?
- Is any one plan file too big or too complex to run in one go? Ask for it to be split into many files, with the plan order set again.

### 3. Check the SOLID/IoC rules
Call the `solid-check` skill for this part, so the design rules live in one place.

- Pull out every class, namespace, or file path the plan will touch, and pass them to `solid-check` as its scope. If the plan touches scopes that are far apart, call it once for each scope.
- If the plan makes a class that is not in the code yet, pass the closest parent namespace or folder, and judge the planned design by the same rules.
- Paste the report that comes back into section 3 of your own report, as it came.
- A break here is a plan edit: fold the fix into the plan file. The code stays untouched until `plan-run`.

**Done when**: every plan file in scope carries an explicit verdict on all three checks, and every class or path the plan names has been looked up in the current code. A check you did not mention is a check you did not run.

### 4. Make a report
1. **Scope**: the plan files you checked, in the plan order.
2. **One section per check** — risk, runnable, SOLID/IoC — each listing
   - Where it is: the plan file plus the step it sits in, and the code path it clashes with
   - What is wrong, and which way to change the plan
3. **Overall judgment**: ready to run / room to improve / must be rewritten before it runs.

### 5. Settle the open items
Every open item steps 1-3 turned up — vague, undecided, or at odds with the current code — is settled with the user here, before the plan is handed back.

- Call the `grilling` skill and walk through each open item, one question at a time, until you both hold the same understanding.
- Call the `domain-modeling` skill while you grill. Put each settled term into `CONTEXT.md`, and record a hard-to-undo decision as an ADR under `docs/adr/`.
- Write the answers back into the plan file, then say the overall judgment again if it moved.

**Done when**: every plan file is left with clear run steps and no open items.
