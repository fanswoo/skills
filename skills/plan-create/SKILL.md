---
name: plan-create
description: Create a development plan file. Use this when the user says "create plan", "add a development plan", or needs to make a new plan file in the storage/plans directory.
---

# Create Development Plan
## Plan
In the `./storage/plans/` directory, make a new development plan folder. Name the folder in the form `yymmdd-my-plan` (for example `260401-add-payment-feature`). The folder must hold at least one `*.md` file.

## Development Plan Notes
- When you write code, take care that it is easy to test, easy to read, and low coupled
- When it fits, use PHP Attribute and Laravel Attribute in place of old ways
- Confirm all "to be confirmed" items clearly before you write them into the plan
- Study the current code with care before you make changes
- When you build the development plan, make sure this plan does not break any current feature
- You must add "check items" to the plan, so you can confirm the feature works well after the work is done
- Never assume you know what the user wants. If anything is not clear, ask first before you act.
- Always make sure the code follows the SOLID/IoC rules
- If the plan is large, make many `*.md` plans in the folder that can each run on their own, and mark the order to run them
- The plan must not hold open items like "option A/B", "suggest XXX", or "may think about". A plan is made to run, not to talk about. You must settle all choices before you write the plan. If you cannot settle a choice, ask the user first.
- Do not write the plan files in a form like 1-a 2-b. Use 01 02 instead.
- Do not write any code in the plan