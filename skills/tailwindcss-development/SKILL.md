---
name: tailwindcss-development
description: "Tailwind CSS v4 styling — house style, on-scale values, utility-only classes. Use when writing or changing any styling, CSS, or classes, or when another skill needs the project's Tailwind conventions."
license: MIT
metadata:
  author: laravel
---

# Tailwind CSS Development

## House style first

Read the components around the one you are touching before you add a class. Match what they already do — their spacing rhythm, their color tokens, their breakpoints, their component boundaries. The house style outranks any default you would otherwise reach for.

Dark mode is the part most often missed: if the surrounding pages carry `dark:` variants, every new element carries them too.

## Stay on the scale

Every value comes from Tailwind's default spacing, size, and color scale. When the scale genuinely lacks a value, extend the scale — add a token in `@theme` — and use the token.

Two escape hatches lead off the scale, and both stay shut:

- **Arbitrary values** — the square-bracket form (`w-[12px]`, `text-[20px]`, `bg-[#123456]`). Use the scale or an `@theme` token instead.
- **Hand-written CSS** — custom class names and CSS rules (`.my-card`, `.custom-button`). Reuse comes from extracting a component (Blade, Vue, JSX) instead.

## Utilities

Use `gap` for space between siblings; reach for margin only to push an element away from something that is not its sibling.

Keep the class list minimal: drop classes that restate what a parent already sets, and hoist a class to the parent when every child repeats it.

## v4, not v3

The config is CSS-first. There is no `tailwind.config.js` and no `corePlugins`; theme values live in `@theme`:

```css
@theme {
  --color-brand: oklch(0.72 0.11 178);
}
```

Tailwind is imported as plain CSS — `@import "tailwindcss";` — in place of the v3 `@tailwind base/components/utilities` directives.

These v3 utilities were removed. Opacity is now a slash modifier:

| Removed | v4 |
|---|---|
| `bg-opacity-*` | `bg-black/*` |
| `text-opacity-*` | `text-black/*` |
| `border-opacity-*` | `border-black/*` |
| `divide-opacity-*` | `divide-black/*` |
| `ring-opacity-*` | `ring-black/*` |
| `placeholder-opacity-*` | `placeholder-black/*` |
| `flex-shrink-*` | `shrink-*` |
| `flex-grow-*` | `grow-*` |
| `overflow-ellipsis` | `text-ellipsis` |
| `decoration-slice` | `box-decoration-slice` |
| `decoration-clone` | `box-decoration-clone` |

## Before you finish

Re-read every class you wrote or changed. Each one is a v4 utility, on the scale, and consistent with the house style around it. Fix the ones that are not before you report done.
