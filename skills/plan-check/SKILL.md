---
name: plan-check
description: Check a dev plan. Use it when the user says "check plan", "review plan", or needs to make sure plan files under the storage/plans folder have no possible problems.
---

# Check a Dev Plan
## Plan
`$ARGUMENTS` names the plan to check. It may be a single `*.md` plan file or a whole plan folder, and it is looked up under `./storage/plans/`. If `$ARGUMENTS` is empty or matches nothing there, ask the user which plan to check before you start.

Go through the plan with care, and when it is a folder, take the files in the plan order.

## Run Flow

### 1. Check for possible problems
- Could the changes in the plan break a feature that already works?
- Are there missing edge cases or missing error handling?
- Are there hidden risks in speed or safety?
- Do the files or classes named in the plan really exist in the current code?

### 2. Check for clear writing
- Does each plan file hold all four parts: goal, files touched, run steps, and check items?
- Are the check items there and can they really be tested? The `plan-run` skill starts its tests from them, so a plan file without them cannot be run.
- Is the design the plan asks for easy to test, easy to read, and low coupled?
- Where it fits, does the plan use PHP Attribute and Laravel Attribute in place of the old way?
- Are there steps in the plan that are vague or unclear in meaning?
- Is the wanted result of each step clear?
- Are the tech details in the plan specific enough to run right away?
- If the plan covers many files, is the run order clear?
- Is the folder named in the `yymmdd-my-plan` form, and is each file named in the `01-my-step.md` form? A style like 1-a 2-b must be changed to 01 02.
- If one plan file is too big or too complex, it should be split into many plans and the plan order set again.
- There should be no code in the plan. If you find any, you must ask for it to be removed.

### 3. Check the SOLID/IoC rules
Do not write the SOLID rules here. Call the `solid-check` skill and let it do this part, so there is only one place that holds the rules.

- Pull out every class, namespace, or file path the plan will touch, and pass them to `solid-check` as its scope. If the plan touches scopes that are far apart, call it once for each scope.
- If the plan makes a class that is not in the code yet, pass the closest parent namespace or folder, and judge the planned design by the same rules.
- Put the report that comes back into section 3 of your own report. Do not write it again in your own words.
- Here you check a plan, not code that is already written. So when `solid-check` finds a break, ask for the **plan** to be changed. Do not change any code, and do not run the refactor flow at the end of `solid-check`.

### 4. Make a report
Report layout:
1. **Scope**: list the plan files you checked, in the plan order.
2. **One section per check**: sections 1, 2, and 3 above, each listing
   - Where it is: the plan file plus the step it sits in, and the code path it clashes with
   - What is wrong, and which way to change the plan
3. **Overall judgment**: ready to run / room to improve / must be rewritten before it runs.

### 5. Settle what is not clear
When step 1-3 turn up something vague, undecided, or at odds with the current code, do not guess and do not just list it.

- Call the `grilling` skill and walk the user through each open point, one question at a time, until you both hold the same understanding.
- Call the `domain-modeling` skill while you grill. Put each settled term into `CONTEXT.md`, and record a hard-to-undo decision as an ADR under `docs/adr/`.
- Then write the answers back into the plan file, so the plan is left with clear run steps and no open items.

## Notes
- When you check, always compare with the current code to make sure the plan can be done.
- If you find a problem, you must point out where it is and give advice to make it better.
- Never assume you know what the user wants. If anything is not clear, you must ask first.
- Check the plan for undecided content like "option A/B", "suggest XXX", "may consider". A plan is made to be run, not to be talked over. If you find such content, you must ask for it to be changed into clear run steps. Settle it with step 5.
