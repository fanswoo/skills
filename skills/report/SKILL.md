---
name: report
description: Research this project against the real code and write a detailed report.
disable-model-invocation: true
---

# Report

`$ARGUMENTS` is the scope — a subsystem, a flow, a question, or a directory. Empty means the whole project.

The run touches no code. The deliverable is one Markdown file: `git status` at the end shows the tree you started with, plus that file.

A report is a **snapshot** of the code as it stands today, so it cites `file_path:line_number` everywhere. The repo rule that keeps paths out of `spec.md` does not reach here — that rule protects documents meant to outlive the code, and a snapshot is not one.

## Run Flow

### 1. Fix the scope
Say back in one line what you are about to report on, and name where the file lands: `.scratch/<yymmdd-slug>/report.md`, the slug named for the scope. When the scope belongs to a plan folder that already exists, write it there instead. Then start — the user reads the line and stops you if it is wrong.

### 2. Read the domain docs
Read the repo's domain documentation before you open any code — `docs/agents/domain.md` says which files those are and how to use them. The words in `CONTEXT.md` are the words the report uses.

### 3. Sweep
Breadth first, before you read anything closely. Walk the whole scope: top-level directories, entry points, routes and console commands, config, dependency manifests, the test tree, and what recent git history has been touching.

**Done when**: every directory and manifest in scope carries a line — what it holds, or why the report can pass over it. A part you never opened is a part you cannot leave out quietly.

### 4. Drill
Now go deep on what the sweep raised. Read each file that matters in full, follow it out to its callers and its tests, and trace each main flow end to end — entry point, through the layers, to the data and back.

**Done when**: every claim the report is about to make is **grounded** — you can name the `file_path:line_number` it came from. What the code does not answer is an **unknown**, named as one, never filled in from memory.

### 5. Write the report
Write the file with these sections, in this order. A section the scope does not reach gets one line saying so — the skeleton stays whole either way.

```markdown
# Report — <scope>

## What it is
What this project does, and who for.

## How it is built
The stack, the layout, and what each top-level part holds.

## How it runs
Entry points, commands, environments, and what has to be in place to boot it.

## Domain
The concepts the code works in, and the words it uses for them, in `CONTEXT.md` terms.

## Key flows
Each main flow traced end to end, step by step.

## Tests
What is covered, at which seams, and what is not covered.

## Findings
What is stale, risky, duplicated, or missing — one line each.

## Unknowns
What the code does not answer, and what it would take to settle each one.
```

**Done when**: every section is present, every claim outside Unknowns carries its `file_path:line_number`, and everything step 4 could not settle is sitting in Unknowns.

### 6. Hand back
Give the user the path and the Findings and Unknowns lists in the chat, so they can act without opening the file.

## Where it hands off
This skill ends at the report. When it points at work, name the skill and leave the call to the user.

- A finding worth building or fixing — `plan-create` for a feature, `run` for a task, `fix` for a bug
- The design of what the report walked through — `solid-check`
- The question turned out to be about a package or an external API, not this codebase — `research`
- A term the report had to invent, or a decision it uncovered that would be hard to undo — offer `domain-modeling` to record it in `CONTEXT.md` or as an ADR under `docs/adr/`
