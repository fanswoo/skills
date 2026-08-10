---
name: debrief
description: Use when another skill needs to close its run on a debrief.
---

# Write a Debrief

A **debrief** is a run's last message. The run ends here: you write it, the user reads it.

A run that was handed its answers by a caller is one leg of a wider **span** — its lines go up to that caller, and the caller's debrief closes.

## The three parts

Walk the span back to collect them. Each part carries its lines, or the word `none`.

1. **Built** — what the run produced, one line per task or ticket, and the state the suite was left in: the last full run and its result.
2. **Assumed** — every `Assumed:` sentence written during the run, in the words you wrote it.
3. **Standing** — every break left in place, each at `file_path:line_number`, with why it stayed: it was already red before you started, or the fix sits outside the task.

## Every line is a statement

A question you find yourself writing is a decision you already made: the reading goes under **Assumed**, the work you left goes under **Standing**. An assumption the user would rather flip, or a break they want cleared, comes back later as its own task.

**Written when**: every assumption carried since the briefing is listed, every finding the run's reviews turned up is either fixed or standing here with its reason, and the debrief has gone out as the run's last message — or up to the caller whose span it closes.
