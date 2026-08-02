# Wide Refactor

A **wide refactor** is one mechanical change — rename a column, retype a shared symbol, move a namespace — whose **blast radius** fans across the whole codebase. A single edit breaks thousands of call sites at once, so no vertical slice can land green. Do not force it into a tracer bullet.

Sequence it as **expand–contract** instead.

## Expand

One ticket. Add the new form beside the old one so nothing breaks. Both forms work; every call site still passes.

## Migrate

One ticket per batch, each blocked by the expand ticket. Move the call sites over in batches sized by blast radius — per package, per namespace, per folder, whatever cuts the work into pieces one session can hold.

CI stays green batch to batch, because the old form is still there.

## Contract

One ticket, blocked by every migrate ticket. Delete the old form once no caller is left.

## When even a batch cannot stay green

Keep the same sequence, but let the batches share an integration branch, and add a final integrate-and-verify ticket blocked by all of them. Green is promised only there. Say so in the tickets, so nobody reads a red batch as a broken build.
