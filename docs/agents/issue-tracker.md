# Issue tracker: Local Markdown

Issues and specs (you may know a spec as a PRD) for this repo live as markdown files in `.scratch/`.

## Conventions

- One feature per directory: `.scratch/<yymmdd-slug>/` — `yymmdd` is the date the work was planned (for example `260401-add-payment`)
- The spec is `.scratch/<yymmdd-slug>/spec.md`
- Implementation issues are one file per ticket at `.scratch/<yymmdd-slug>/issues/<NN>-<slug>.md`, numbered from `01` in dependency order (blockers first) — never a single combined tickets file
- Comments and conversation history append to the bottom of the file under a `## Comments` heading

## Who writes what

In this repo `/plan-create` owns the whole front half — it grills, picks the seams, writes the spec, slices the tickets, and fills in each ticket's Files touched and Run steps. `/plan-check` gates them and `/plan-run` works them.

`to-spec` and `to-tickets` are not used here; `plan-create` covers both. If you do run `to-spec` by hand, note that it applies `ready-for-agent` to the spec — that is wrong under these conventions, because a spec is never agent-grabbable. Only tickets carry a `Status:` line.

## Paths only at ticket level

`spec.md` and a ticket's **What to build** name domain concepts, never file paths or code — they outlive the code and go stale the moment a file moves.

File paths, class names, and namespaces appear in exactly one place: a ticket's **Files touched**. That section is consumed inside one session and thrown away, so it can afford to be concrete.

## The spec

```markdown
## Problem

The problem the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## Seams

Where this feature gets tested. Prefer existing seams to new ones, use the highest
seam available, and use as few as possible — the ideal number is one.

## Out of scope

What this effort explicitly does not do.
```

## The ticket

```markdown
# NN — Ticket title

**Blocked by:** 01, 02
**Status:** ready-for-agent

## What to build

The end-to-end behaviour this ticket makes work, from the user's perspective — not a
layer-by-layer implementation list.

## Check items

- [ ] Each one confirmable by a named test or command.

## Files touched

The classes, namespaces, or file paths this ticket adds or changes.

## Run steps

1. Numbered steps in prose, each specific enough to act on right away.
```

`Blocked by: None` when the ticket can start immediately. A ticket is **unblocked** when every ticket it lists is `resolved`.

## Status

One `Status:` line per ticket, drawn from one merged state machine — the five triage roles from `triage-labels.md` plus the two working states below:

```
needs-triage → needs-info → ready-for-agent → claimed → resolved
                              ↘ ready-for-human      ↘ wontfix
```

- `needs-triage` / `needs-info` / `ready-for-agent` / `ready-for-human` / `wontfix` — written by `/plan-check` and `/triage`.
- `claimed` — written by `/plan-run` before it starts work on a ticket.
- `resolved` — written by `/plan-run` once every check item passes.

## When a skill says "publish to the issue tracker"

Create a new file under `.scratch/<yymmdd-slug>/` (creating the directory if needed).

## When a skill says "fetch the relevant ticket"

Read the file at the referenced path. The user will normally pass the path or the issue number directly.

## Frontier

The **frontier** is every ticket that is open, unblocked, and unclaimed. Scan `.scratch/<slug>/issues/` for files whose `Status:` is `ready-for-agent` and whose every `Blocked by` entry is `resolved`. When more than one qualifies, the lowest number wins.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a file with one **child** file per ticket.

- **Map**: `.scratch/<effort>/map.md` — the Notes / Decisions-so-far / Fog body.
- **Child ticket**: `.scratch/<effort>/issues/NN-<slug>.md`, numbered from `01`, with the question in the body. A `Type:` line records the ticket type (`research`/`prototype`/`grilling`/`task`); the `Status:` line and the `Blocked by:` line work as above.
- **Claim**: set `Status: claimed` and save before any work.
- **Resolve**: append the answer under an `## Answer` heading, set `Status: resolved`, then append a context pointer (gist + link) to the map's Decisions-so-far in `map.md`.
