---
name: plan-create
description: Create a development plan file. Use this when the user says "create plan", "add a development plan", or needs to make a new plan file in the storage/plans directory.
---

# Create Development Plan
## Plan
Read `$ARGUMENTS` as the topic of the plan. If it is empty or not clear, ask the user what the plan is for before you start.

In the `./storage/plans/` directory, make a new development plan folder. Name the folder in the form `yymmdd-my-plan`, where `yymmdd` is today's date (for example `260401-add-payment-feature`). The folder must hold at least one `*.md` file, named in the form `01-my-step.md`.

## Run Flow

### 1. Grill the user first
Never write a plan straight from a one-line ask. Call the `grilling` skill and interview the user until you both hold the same understanding.
- Ask one question at a time and wait for the answer before the next one
- Look up facts in the code yourself. Only bring real decisions to the user
- Keep going until nothing is left open. Every "option A/B" must be settled here, not carried into the plan file

Call the `domain-modeling` skill at the same time, so the words and decisions you settle do not get lost.
- When a term is settled, put it into `CONTEXT.md` right away. Do not save them all for the end
- When a decision is hard to undo, would puzzle a later reader, and came from a real trade-off, record it as an ADR under `docs/adr/`
- `CONTEXT.md` holds words only. How-to-build detail belongs in the plan file, not there

### 2. Check the design before you write
Do not write the SOLID rules here. Call the `solid-check` skill on the classes or namespaces the plan will touch, and let its report shape the plan.
- If a class is not in the code yet, pass the closest parent namespace or folder
- If the plan touches scopes that are far apart, call it once for each scope
- A break must be settled inside the plan. Do not change any code while you make a plan, and do not run the refactor flow at the end of `solid-check`

### 3. Then write the plan
Write the plan file with the words settled in `CONTEXT.md`, so the plan and the glossary use the same names for the same things.

Each plan file holds these parts, in this order:
1. **Goal**: what this plan file finishes, in one or two lines
2. **Files touched**: the classes, namespaces, or file paths this plan file adds or changes
3. **Run steps**: numbered steps, each one clear enough to run right away
4. **Check items**: how to confirm the feature works after the work is done. The `plan-run` skill starts its tests from these

### 4. Check the plan you just wrote
Call the `plan-check` skill on the folder you just made, and settle what it finds before you hand the plan to the user.

## Development Plan Notes
- When you plan the code, take care that it is easy to test, easy to read, and low coupled
- When it fits, use PHP Attribute and Laravel Attribute in place of old ways
- Confirm all "to be confirmed" items clearly before you write them into the plan
- Study the current code with care before you make changes
- When you build the development plan, make sure this plan does not break any current feature
- You must add "check items" to the plan, so you can confirm the feature works well after the work is done
- Never assume you know what the user wants. If anything is not clear, ask first before you act.
- Always make sure the plan follows the SOLID/IoC rules. Do not write the rules here. Step 2 of the run flow does this check
- If the plan is large, make many `*.md` plans in the folder that can each run on their own, and mark the order to run them
- The plan must not hold open items like "option A/B", "suggest XXX", or "may think about". A plan is made to run, not to talk about. You must settle all choices before you write the plan. If you cannot settle a choice, ask the user first.
- Do not write the plan files in a form like 1-a 2-b. Use 01 02 instead.
- Do not write any code in the plan
