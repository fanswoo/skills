---
name: run
description: Do a dev task. Use it when the user says "run xxx", "change xxx", "build xxx", "fix xxx", or needs code changes that follow the SOLID/IoC rules.
---

# Dev Task
## Plan
`$ARGUMENTS`

## Notes
- When you write code, pay special heed to how easy it is to test, how easy it is to read, and how low the coupling is.
- When it fits, try to use PHP Attribute and Laravel Attribute in place of the old way.
- Before you change anything, always study the current code with care.
- When you change code, always make sure this change does not break a feature that already works.
- After each code change, always make sure you leave no unused class, variable, or function behind.
- Never assume you know what the user wants. If anything is not clear, you must ask first before you run.
- Always make sure the code follows the SOLID/IoC rules.
- Tell the run plan first, then run it.

### Important Build Note
The dev person will start the build mode on their own. You should never run the build on your own. If a build is truly needed, the most you may do is "remind the dev person to build".
```bash
npm run build
```
