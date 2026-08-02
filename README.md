# fanswoo skills

Laravel/Filament workflow skills — dev plans, SOLID and Filament convention checks, Pest fixes, and Tailwind styling.

## Install

```bash
npx skills add fanswoo/skills --all
```

### Dependencies

`plan-create`, `fix`, `filament-check`, and `plan-run` call into
[mattpocock/skills](https://github.com/mattpocock/skills). `skills add` does not resolve dependencies,
so run the bundled skill once after installing:

```
/install-dependencies
```

It is yours to invoke — Claude never runs it on its own, since it writes to your repo.

## Setup

Run once per repo, before any planning skill:

```
/setup-matt-pocock-skills
```

The planning skills do not hardcode where your issues live — they read it from
`docs/agents/issue-tracker.md`. This skill writes that file, so nothing works until it has run.

It explores the repo first, then asks you up to three questions and shows you a draft before writing:

| Section | What it decides | Default |
| --- | --- | --- |
| Issue tracker | GitHub Issues (`gh`), GitLab (`glab`), local markdown under `.scratch/`, or your own workflow in prose | Whatever `git remote` points at |
| Triage labels | The five role strings: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix` | Keep the defaults — skipped entirely if `triage` is not installed |
| Domain docs | One root `CONTEXT.md` + `docs/adr/`, or a `CONTEXT-MAP.md` per context | Single-context, written without asking; multi-context is offered only in a monorepo |

It writes `docs/agents/issue-tracker.md`, `docs/agents/domain.md`, `docs/agents/triage-labels.md`, and
an `## Agent skills` block into whichever of `CLAUDE.md` / `AGENTS.md` already exists — never both,
never a duplicate block.

Afterwards you can edit `docs/agents/*.md` by hand. Re-run the skill only to switch trackers or start over.

## Planning a feature

Three skills, in order. Each stops at a gate you have to clear — none of them runs to the end on its own.

```
/plan-create <topic>  →  /plan-check <plan>  →  /plan-run <plan>
```

### `/plan-create <topic>`

Turns a feature idea into a spec plus numbered tickets on the tracker. **Touches no code.**

1. **Grills you** — calls `grilling` and `domain-modeling` and interviews you until you both hold the
   same understanding. Settled terms go into `CONTEXT.md` immediately; a hard-to-undo decision becomes
   an ADR under `docs/adr/`.
2. **Picks the seams** — where the feature gets tested. Prefers existing seams, takes the highest one,
   uses as few as possible; one is ideal. **Waits for your answer** before writing anything. Then writes
   `spec.md`.
3. **Slices into tracer bullets** — thin vertical slices through every layer, each demoable on its own
   and sized to one fresh context window, each declaring which tickets block it. Shows you a numbered
   list and asks three questions: is the granularity right, does each blocking edge really gate,
   should anything be merged or split. **Reworks until you approve.**
4. **Fills in the how** — calls `solid-check` on the namespaces the tickets will touch and lets the
   report shape them, then writes each ticket's Files touched and Run steps.
5. **Self-checks** — calls `plan-check` on what it just wrote and settles the findings before handing back.

A mechanical change whose blast radius fans across the codebase is a **wide refactor**; no vertical slice
can land green on it, so it gets sequenced as expand–contract instead
([WIDE-REFACTOR.md](skills/plan-create/WIDE-REFACTOR.md)).

Output: `.scratch/<yymmdd-slug>/spec.md` and `.scratch/<yymmdd-slug>/issues/NN-<slug>.md`.

### `/plan-check <plan>`

Judges a plan against the **current** code. Takes a whole folder or a single ticket. Four checks:

- **Risk** — could this break something that works? Missing edge cases or error handling? **Drift**: do
  the files and classes each ticket names still exist and still look the way the ticket says?
- **Runnable** — all four parts present, check items actually testable, run steps specific enough to act
  on, no `option A/B` or `may consider` left anywhere, no code blocks, no file paths in `spec.md`
- **Slicing** — vertical not horizontal (`01-migration`, `02-model`, `03-controller` is the shape it
  catches), each ticket one context window, every `Blocked by` genuinely gating, none missing
- **SOLID/IoC** — delegates to `solid-check` and pastes the report back verbatim

Then it writes a `Status:` line on every ticket, and grills you through whatever it found still open.
A break here is a **plan edit** — the code stays untouched.

### `/plan-run <plan>`

Works the **frontier**: tickets that are `ready-for-agent` with every blocker already `resolved`, lowest
number first. Per ticket it sets `Status: claimed`, calls `run` to build it, walks the check items naming
the test or command that confirms each one, sets `Status: resolved`, then works the frontier again —
finishing one ticket can unblock several.

A ticket with no check items cannot be run; it sends you to `plan-check` first.

## The upstream workflow

[mattpocock/skills](https://github.com/mattpocock/skills) ships its own planning chain:

```
/grill-me  →  /to-spec  →  /to-tickets  →  /implement
```

- `/grill-me` — a relentless interview to sharpen a plan or design before anything is written down
- `/to-spec` — synthesizes the conversation you just had into a PRD-style spec (Problem, Solution, a long
  list of user stories, Implementation Decisions, Testing Decisions, Out of Scope) and publishes it. No
  interview of its own — it only writes up what you already discussed
- `/to-tickets` — breaks a spec, plan, or conversation into tracer-bullet tickets with blocking edges
- `/implement` — builds from the spec or tickets using `/tdd` at the agreed seams, then `/code-review`

**This repo does not use `to-spec` or `to-tickets`** — `plan-create` covers both, and adds the seam
negotiation, the `solid-check` pass, and the Laravel/Filament conventions. See
[issue-tracker.md](docs/agents/issue-tracker.md#L16). If you do run `to-spec` by hand, note that it
labels the spec `ready-for-agent`, which is wrong under these conventions: a spec is never
agent-grabbable, only tickets carry a `Status:` line.

`/grill-me` is worth keeping either way, and `plan-create` calls the underlying `grilling` skill directly.

All four are `disable-model-invocation: true` — you invoke them, Claude never picks them up on its own.

## Skills

| Skill | Use when |
| --- | --- |
| `plan-create` | Breaking a feature down — grill the ask, pick test seams, write the spec, slice into tickets |
| `plan-check` | Reviewing a plan before it runs |
| `plan-run` | Running a plan from the issue tracker |
| `run` | A dev task: "run xxx", "change xxx", "build xxx", "implement xxx" |
| `fix` | A bug, error, stack trace, or wrong behaviour |
| `fix-testing` | A red test under Pest, Vitest, or `flutter test` |
| `solid-check` | Judging the design quality of a class or namespace |
| `filament-run` | Writing or changing a Resource, Page, Schema, Table, or RelationManager |
| `filament-check` | Reviewing Filament code against the project's conventions |
| `tailwindcss-development` | Any styling, CSS, or class change (Tailwind v4) |
| `frontend-design` | Building web components, pages, or applications |
| `git-push` | "commit", "push", "git push" |
| `install-dependencies` | Pulling in the upstream skills the ones above call into |

## Conventions

Written by `/setup-matt-pocock-skills`, summarized in [CLAUDE.md](CLAUDE.md):

- **Issue tracker** — issues and specs as markdown under `.scratch/<yymmdd-slug>/` ([docs](docs/agents/issue-tracker.md))
- **Triage labels** — five canonical roles ([docs](docs/agents/triage-labels.md))
- **Domain docs** — one `CONTEXT.md` and `docs/adr/` at the repo root ([docs](docs/agents/domain.md))

Paths appear in exactly one place: a ticket's **Files touched**. `spec.md` and a ticket's **What to
build** name domain concepts only — they outlive the code.

Status is one merged state machine:

```
needs-triage → needs-info → ready-for-agent → claimed → resolved
                              ↘ ready-for-human      ↘ wontfix
```
