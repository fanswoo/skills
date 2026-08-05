---
name: discuss
description: Talk something through against the real code — read first, answer in prose, leave the tree untouched.
disable-model-invocation: true
---

# Discuss

`$ARGUMENTS` is what to talk about — a question, an idea, a design on the table, or a piece of the codebase. If it is empty, ask the user what they want to discuss before you start.

The run is **read-only** and the deliverable is prose in the chat: `git status` at the end shows the tree you started with.

## Run Flow

### 1. Read the domain docs
Read the repo's domain documentation before you open any code — `docs/agents/domain.md` says which files those are and how to use them. The words in `CONTEXT.md` are the words your reply uses.

### 2. Do the legwork
Then open the code the question turns on. Glob and Grep to find it, Read each file in full, and follow it out to its callers and its tests. Go wide enough that the shape of the answer stops changing as you read.

**Done when**: every claim you are about to make is **grounded** — you can name the `file_path:line_number` it came from. What the code does not answer is named as an unknown, never filled in from memory.

### 3. Reply
Answer what was asked, in the vocabulary of `CONTEXT.md`, and cite the `file_path:line_number` behind each claim.

Talk about the code — its shape, its trade-offs, what would have to move. When the answer wants to be code, describe the shape in prose and name the skill that would build it.

**Done when**: the question is answered, every unknown is named, and the tree is untouched.

## Where it hands off
This skill ends at the reply. When the talk settles into something else, name the skill and leave the call to the user.

- Ready to build it — `plan-create` for a feature, `run` for a task
- Wants their own thinking stress-tested — `grilling`
- The question is about a package or an external API, not this codebase — `research`
- A term got settled, or a decision that would be hard to undo — offer `domain-modeling` to record it in `CONTEXT.md` or as an ADR under `docs/adr/`
