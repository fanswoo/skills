---
name: plan-create
description: Create a development plan — grill the ask, pick the test seams, write the spec, and slice it into runnable tickets. Use this when the user says "create plan", "add a development plan", or hands you a feature to break down.
---

# Create Development Plan

`$ARGUMENTS` is the topic of the plan. If it is empty or not clear, ask the user what the plan is for before you start.

The plan lives on the repo's issue tracker. Read `docs/agents/issue-tracker.md` for where that is and what the files look like — one `spec.md` for the whole effort, and one file per **ticket** under `issues/`. Both templates live there; do not invent your own.

Do not change any code while you make a plan.

## Run Flow

### 1. Grill the ask
Never write a plan straight from a one-line ask. Call the `grilling` and `domain-modeling` skills together and interview the user until you both hold the same understanding. If those skills are not installed here, run the interview yourself: one question at a time, look up every fact in the code, and put only real decisions to the user.

- When a term is settled, put it into `CONTEXT.md` right away. Do not save them all for the end
- When a decision is hard to undo, would puzzle a later reader, and came from a real trade-off, record it as an ADR under `docs/adr/`
- `CONTEXT.md` holds words only. How-to-build detail belongs in the ticket, not there

**Done when**: nothing is left open. Every "option A/B", "suggest XXX", or "may think about" is settled here, not carried into a file. A plan is made to run, not to talk about.

### 2. Pick the seams, then write the spec
A **seam** is the place the feature gets tested. Prefer a seam that already exists to a new one, take the highest seam you can, and use as few as possible — the ideal number is one. When you need a new seam, propose it at the highest point that works.

Show the user the seams you picked and wait for their answer before you write anything.

Then write `spec.md` with the words settled in `CONTEXT.md`, so the spec and the glossary use the same names for the same things.

**Done when**: the user has confirmed the seams, and `spec.md` carries all four parts with no file path anywhere in it.

### 3. Slice the work into tickets
Cut the spec into **tracer bullets** — thin slices that each prove the whole path works.

- Each slice cuts a narrow but complete path through every layer — schema, API, UI, tests. Vertical, never a horizontal slice of one layer
- A finished slice is demoable or verifiable on its own
- Each slice fits in one fresh context window
- Prefactoring goes first — make the change easy, then make the easy change

Give each ticket its **blocking edges**: the tickets that must finish before it can start. A ticket with no blockers can start immediately.

One mechanical change whose **blast radius** fans across the whole codebase is a **wide refactor**, and no vertical slice can land green on it. Sequence those as expand–contract instead — see [WIDE-REFACTOR.md](WIDE-REFACTOR.md).

Show the user a numbered list — title, blocked by, and what it delivers — and ask three things: is the granularity right, does each blocking edge really gate, should anything be merged or split. Rework until they approve.

**Done when**: the user has approved the breakdown, and every ticket is written to `issues/NN-<slug>.md` in dependency order, blockers first, each carrying its What to build and its check items.

### 4. Fill in how each ticket gets built
Do not write the SOLID rules here. Call the `solid-check` skill on the classes or namespaces the tickets will touch, and let its report shape them.

- If a class is not in the code yet, pass the closest parent namespace or folder
- If the tickets touch scopes that are far apart, call it once for each scope
- A break must be settled inside the ticket, not in the code

Then fill in each ticket's **Files touched** and **Run steps**, and sharpen its check items until each one names something you can actually run.

- Where it fits, use PHP Attribute and Laravel Attribute in place of the old way
- Write the run steps in prose. A code block in a ticket is a finding

**Done when**: every ticket carries all four parts, and every check item names the test or command that will confirm it.

### 5. Check the plan you just wrote
Call the `plan-check` skill on the folder you just made, and settle what it finds before you hand the plan to the user.
