---
description: Apply Tailwind utilities when styling to keep layouts simple and uniform
globs: src/**/*.{tsx,css}
alwaysApply: false
---

# Tailwind CSS Rules

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- For styling with Tailwind CSS v4
- Emphasizes utility classes for consistency

## Requirements

- Maintain consistent spacing (e.g., `p-4`, `m-2`, `space-y-4`).
- Combine conditional classes with `cn()`.
- Use only custom colors defined in `globals.css`.
- Ensure dark mode support via `.dark:` variants.

## Examples

<example>
  import { cn } from "~/lib/utils";
  
  export function ExampleBox({ isActive }: { isActive: boolean }) {
    return (
      <div className={cn("p-4 rounded-md", isActive ? "bg-blue-500" : "")}>
        Content
      </div>
    );
  }
</example>

<example type="invalid">
  <div style={{ padding: "20px" }}>Inline styled box</div>
</example>

## Tailwind v4 Updates

- Config: `tailwind.config.ts` deprecated; now configure in `globals.css` with `@import "tailwindcss"`.
- PostCSS: Plugin moved to `@tailwindcss/postcss`.
- Utility Renames:
  - `shadow-sm` → `shadow-xs`; `shadow` → `shadow-sm`; similar for `blur`, `drop-shadow`.
  - `outline-none` → `outline-hidden`.
  - `ring` defaults to 1px; use `ring-3` for old 3px behavior.
- Removed Utilities: `bg-opacity-*`, `text-opacity-*`, `flex-shrink-*`, `flex-grow-*` → replaced by new patterns (`bg-black/50`, `shrink-*`, `grow-*`, etc.).
- Placeholder Text: Now 50% of current color, not fixed gray.
- Buttons: Default `cursor: default`.
- Border Color: Defaults to `currentColor`.
- `@layer`: `@layer utilities/components` replaced by `@utility`.
- Variant Stacking: Applied left to right (e.g., `.hover:focus:bg-red-500`).
- `space-y-*`: Uses new selector, may affect inline layouts.
- Theming: Use `var(--color-...)` instead of `theme()` in CSS.
