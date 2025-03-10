---
description: Enforce accessibility guidelines when building or reviewing UI to ensure inclusive user experiences
globs: src/**/*.tsx
alwaysApply: false
---

# Accessibility (A11y) Standards

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- Ensures all user-facing pages and components meet basic accessibility
- Applies to interactions, visual elements, and markup structure

## Requirements

- Provide keyboard navigation with visible focus states.
- Use semantic HTML (correct headings, list elements, etc.).
- Include ARIA attributes or roles when necessary.
- Maintain WCAG-compliant color contrast for text and interactive elements.
- Ensure form fields have labels or `aria-label`s; group related fields with `<fieldset>` if appropriate.
- Use consistent skip links or nav landmarks for clear page structure.

## Examples

<example>
  <!-- Properly labeled input with helper text -->
  <label for="email">Email</label>
  <input type="email" id="email" aria-describedby="email-helper"/>
  <p id="email-helper">We'll never share your email address.</p>
</example>

<example type="invalid">
  <!-- Missing label and no ARIA attributes -->
  <input type="text"/>
</example>
