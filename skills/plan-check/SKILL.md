---
name: plan-check
description: Check a dev plan. Use it when the user says "check plan", "review plan", or needs to make sure plan files under the storage/plans folder have no possible problems.
---

# Check a Dev Plan
## Plan
Read the `$ARGUMENTS.md` file or folder, read it with care, and check the dev plan.

## What To Check

### 1. Check for possible problems
- Could the changes in the plan break a feature that already works?
- Are there missing edge cases or missing error handling?
- Are there hidden risks in speed or safety?
- Do the files or classes named in the plan really exist in the current code?

### 2. Check for clear writing
- When you write code, pay special heed to how easy it is to test, how easy it is to read, and how low the coupling is.
- When it fits, try to use PHP Attribute and Laravel Attribute in place of the old way.
- Are there steps in the plan that are vague or unclear in meaning?
- Is the wanted result of each step clear?
- Are the tech details in the plan specific enough to run right away?
- If the plan covers many files, is the run order clear?
- If one plan file is too big or too complex, it should be split into many plans and the plan order set again.
- Do not write the plan file with a style like 1-a 2-b. Use 01 02 instead.
- There should be no code in the plan. If you find any, you must ask for it to be removed.

### 3. Check the SOLID/IoC rules
- **Single Responsibility Principle (SRP)**: does each class or module handle only one job?
- **Open-Closed Principle (OCP)**: do you add a feature by extending, not by changing?
- **Liskov Substitution Principle (LSP)**: can a child class stand in for the parent class without breaking it?
- **Interface Segregation Principle (ISP)**: is the interface lean enough, so it does not force you to implement methods you do not need?
- **Dependency Inversion Principle (DIP)**: do you depend on an abstract layer, not a concrete build, and do you use the IoC container the right way?

## Notes
- When you check, always compare with the current code to make sure the plan can be done.
- If you find a problem, you must point out where it is and give advice to make it better.
- Never assume you know what the user wants. If anything is not clear, you must ask first.
- Check the plan for undecided content like "option A/B", "suggest XXX", "may consider". A plan is made to be run, not to be talked over. If you find such content, you must ask for it to be changed into clear run steps.
