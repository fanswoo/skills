---
name: install-dependencies
description: Install the upstream skills these skills call into.
disable-model-invocation: true
---

# Install Dependencies

These skills call into [mattpocock/skills](https://github.com/mattpocock/skills). `npx skills add` does
not resolve dependencies, so nothing pulls them in on its own — install them explicitly.

## Run Flow

### 1. Check what is already there
List `.claude/skills/` and `~/.claude/skills/`. When `grilling`, `domain-modeling`, `diagnosing-bugs`,
and `tdd` are all present, say so and stop — there is nothing to install.

### 2. Pick the scope
- **This project only** — the default. Writes `.agents/skills/`, `.claude/skills/`, `skills-lock.json`
- **Every project on this machine** — add `-g`. Writes `~/.claude/skills/` and touches nothing in the repo

When the user has not said which, ask. Installing into their repo adds tracked files, so it is their call.

### 3. Install
```bash
npx skills add mattpocock/skills -s '*' -a claude-code -y
```

Do not use `--all`. It expands to `--agent '*'`, which creates a stray non-hidden `agent/` directory
next to `.agents/`.

**Done when**: 22 skills are listed as installed, and you have named the paths that were written.

### 4. Confirm they loaded
Claude Code watches `.claude/skills/` and picks new skills up in this session — no restart. The one
exception is a project where `.claude/skills/` did not exist when the session started; if the skills
do not resolve, tell the user to restart Claude Code.
