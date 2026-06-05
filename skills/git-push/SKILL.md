---
name: git-push
description: Git commit and push. Use this when the user says "commit", "push", "git push", or needs to commit changes and push them to the remote.
---

# Git Commit and Push

## Steps

1. Run `git status` to see the current changes
2. Run `git diff --staged` and `git diff` to see what changed
3. Run `git log --oneline -5` to see the recent commit style
4. Based on the changes, write a short commit message (follow the commit style this project already uses)
5. If the user gives `$ARGUMENTS`, use that text as the commit message
6. Run these right away, with no need to ask the user:
   - `git add -A` (add all changes, both unstaged edits and new untracked files)
   - `git commit -m "message"`
   - `git push`

## Notes
- You must commit all changes (staged + unstaged + untracked). Do not pick only some files.
- If there are no changes, tell the user "There are no changes to commit" and stop.
- Do not commit files with secret data (.env, credentials, and so on).
- Write the commit message in English. Keep it short and clear.
