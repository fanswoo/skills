---
name: git-push
description: Commit and push. Use when the user says "commit", "push", or "git push".
---

# Git Commit and Push

Sweep every change into one commit and push it, in a single run. The target is a **clean tree**: `git status` reporting nothing left to commit and nothing left to push.

## Run Flow

### 1. Survey
Run `git status`, `git diff HEAD`, and `git log --oneline -5` in one batch — the diff is what you describe, the log is the commit style you match.

If the tree is already clean, tell the user "There are no changes to commit" and stop.

### 2. Write the message
`$ARGUMENTS` is the message when the user supplied one — use it verbatim. Otherwise write your own: English, short, in the style the log shows.

### 3. Ship
Run these three as one action, straight through:
- `git add -A` — every change goes in: staged edits, unstaged edits, and untracked files alike
- `git commit -m "message"`
- `git push` — the current branch, as it stands

Secrets stay out of the commit: if the survey turned up `.env`, keys, or credential files, `git reset` those paths after staging and name them to the user.

**Done when**: `git status` shows a **clean tree** — working tree clean, branch in sync with its remote — and you have reported the commit subject and the branch it went to.
