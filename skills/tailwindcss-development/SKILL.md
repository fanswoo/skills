---
name: tailwindcss-development
description: "Style the app with Tailwind CSS v4 utilities. Use this when you add styles, change a component's style, work on gradients, spacing, layout, flex, grid, responsive design, dark mode, colors, type, or borders. Also use it when the user talks about CSS, styles, classes, Tailwind, restyle, hero section, cards, buttons, or any look or UI change."
license: MIT
metadata:
  author: laravel
---

# Tailwind CSS Development

## When To Use

Use this skill in these cases:

- Add styles to a component or page
- Work on responsive design
- Build dark mode
- Pull repeated styles out into a component
- Debug spacing or layout problems

## Doc Search

Use `search-docs` to get Tailwind CSS v4 patterns and docs.

## Basic Use

- Use Tailwind CSS classes to style HTML. Before you bring in a new style pattern, check and follow the project's own Tailwind ways.
- Suggest pulling repeated style patterns out into a component that fits the project (like Blade, JSX, Vue).
- Watch where you put a class, its order, its priority, and its default. Remove extra classes. Move classes to the parent or child with care to cut repeats. Group elements in a way that makes sense.

## Strict Rules

- **No custom-named CSS**: do not write custom class names or custom CSS styles (like `.my-card`, `.custom-button`). Use only the utility classes that Tailwind gives you. If you need to reuse a style, pull it into a component (Blade/Vue/JSX), not custom CSS.
- **No arbitrary values**: do not use the square-bracket any-value form, like `w-[12px]`, `text-[20px]`, `bg-[#123456]`, `mt-[13px]`. Use the Tailwind default spacing / size / color scale. If the default scale is not enough, add a design token with `@theme` instead of arbitrary values.

## Tailwind CSS v4 Notes

- Always use Tailwind CSS v4, and do not use old, dropped utilities.
- Tailwind v4 does not support `corePlugins`.

### CSS-First Config

In Tailwind v4, the config is CSS-first. You set it with the `@theme` directive. You no longer need a separate `tailwind.config.js` file:

<!-- CSS-First Config -->
```css
@theme {
  --color-brand: oklch(0.72 0.11 178);
}
```

### Import Syntax

In Tailwind v4, bring in Tailwind with the standard CSS `@import` syntax. This takes the place of the `@tailwind` directive used in v3:

<!-- v4 Import Syntax -->
```diff
- @tailwind base;
- @tailwind components;
- @tailwind utilities;
+ @import "tailwindcss";
```

### Replaced Utilities

Tailwind v4 removed the old, dropped utilities. Use the swaps in the table below. The opacity numbers still use the number form.

| Dropped | Use Instead |
|------------|-------------|
| bg-opacity-* | bg-black/* |
| text-opacity-* | text-black/* |
| border-opacity-* | border-black/* |
| divide-opacity-* | divide-black/* |
| ring-opacity-* | ring-black/* |
| placeholder-opacity-* | placeholder-black/* |
| flex-shrink-* | shrink-* |
| flex-grow-* | grow-* |
| overflow-ellipsis | text-ellipsis |
| decoration-slice | box-decoration-slice |
| decoration-clone | box-decoration-clone |

## Spacing

Use `gap` utilities to set space between elements on the same level, not margin:

<!-- Gap Utilities -->
```html
<div class="flex gap-8">
    <div>Item 1</div>
    <div>Item 2</div>
</div>
```

## Dark Mode

If the pages and components already support dark mode, your new pages and components must support it the same way too. This is most often done with the `dark:` variant:

<!-- Dark Mode -->
```html
<div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-white">
    Content adapts to color scheme
</div>
```

## Common Patterns

### Flexbox Layout

<!-- Flexbox Layout -->
```html
<div class="flex items-center justify-between gap-4">
    <div>Left content</div>
    <div>Right content</div>
</div>
```

### Grid Layout

<!-- Grid Layout -->
```html
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
    <div>Card 1</div>
    <div>Card 2</div>
    <div>Card 3</div>
</div>
```

## Common Traps

- Using old, dropped v3 utilities (bg-opacity-*, flex-shrink-*, and so on)
- Using the `@tailwind` directive instead of `@import "tailwindcss"`
- Trying to use `tailwind.config.js` instead of the CSS `@theme` directive
- Using margin instead of gap utilities to set space between elements on the same level
- Forgetting dark mode variants when the project uses dark mode
- Writing custom class names or custom CSS styles
- Using arbitrary values (like `w-[12px]`, `text-[#abcdef]`)
