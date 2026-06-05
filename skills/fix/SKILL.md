---
name: fix
description: Fix code problems. Use this when the user says "fix xxx", "debug xxx", or needs to find and fix a bug for a given error.
---

# Fix Problem
## Need
Fix `$ARGUMENTS`

## Rules
- When you write code, take care that it is easy to test, easy to read, and low coupled
- When it fits, use PHP Attribute and Laravel Attribute in place of old ways
- First study the current code and the error message with care. Clearly list the cause of the error. You must confirm the problem before you make changes.
- When you change code, make sure this change does not break any current feature
- After each code change, make sure no unused class, variable, or function is left behind
- Never assume you know what the user wants. If anything is not clear, ask first before you act.
- Always make sure the code follows the SOLID/IoC rules
- After the fix, run the related tests to confirm the problem is solved and has no side effect

## Tips
- Use context7 to look up how the related package works, so you do not use a method that does not exist
- The back office uses filament v5. If you need to change back office code, look up how filament v5 works.

### Important Build Note
The developer will start the build mode on their own. You should never run the build on your own. If a build is truly needed, the most you may do is "remind the developer to build"
```bash
npm run build
```
