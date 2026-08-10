---
name: briefing
description: Use when another skill needs to open its run on a briefing.
---

# Hold a Briefing

A **briefing** is a run's one interview: every question the work depends on, in a single message at the top, before any code. One per run — when the skill that called you hands you its answers, the interview is already held. Take them and go.

`$ARGUMENTS` carries the **span** the briefing opens: the stretch of work its answers govern, from a single task to a whole plan. A briefing reaches as far as its span — every skill the work inside it calls runs on its answers, and a caller holding a wide briefing hands those answers down with the work.

## 1. Answer what you can

Read the task, the code it turns on, and the docs it names. A question the code, the spec, or the project docs already answer is answered — the digging is yours, not the user's.

A question earns its place when a wrong guess would change what gets built. The rest you settle yourself, out of the code and the material you have just read.

**Done when**: every question you can still name is one the codebase does not settle and a wrong guess would change what gets built.

## 2. Send one message

Sweep the span for what is left:

- **The shape of finished work** — where the task reads two ways and the two readings build different things
- **The inputs the span's skills run on** — the seams the tests go at, the fixed point a review runs against, whatever else a skill inside the span would otherwise stop mid-run to ask for
- **The irreversible moves** — anything the task implies that editing code and running again cannot undo: data dropped, history rewritten, a live system touched. Their go-ahead is a briefing item

Put it in one message: how you plan to run the work and in what order, then the questions, numbered, each carrying the answer you take if it goes unanswered. One reply then covers every question, and "go" takes every default.

A message with no questions in it is a plan line, not a gate: post it and start.

**Done when**: the message is sent and the answers are in, or the message carried no question.

## After the briefing

Everything the span raises from here is yours to settle. Take the most reasonable reading, write it as one sentence — `Assumed: <the reading>` — and carry on. Those sentences are the **debrief**'s middle part.

Three states reach the user mid-run — a **hard stop**. Answered, the run picks up where it stopped and carries to the end of the span.

- **The assumption cannot be written** — every reading you could put in an `Assumed:` sentence contradicts what the briefing settled. Put the readings side by side and ask which one
- **The move cannot be undone** — data dropped, history rewritten, a live system touched. Name the move and wait
- **The run cannot go on** — what the task names is nowhere in the code and nothing resembles it, or the access it needs is not yours. Say what you searched

Everything else is an assumption. Where it is close, the line runs:

| What came up | What you do |
|---|---|
| The task never fixed a detail — a TTL, a column type, a label, a route name | Take the reading the codebase already uses |
| Two classes or files match the name you were given | Take every match into scope, and write down which ones you took |
| No seam was named for a behaviour under test | Test at the highest public boundary it crosses |

**Done when**: every question the span raised carries either an `Assumed:` sentence or a hard stop the user answered.
