---
description: Enforce a product-specific UI finish gate before shipping frontend changes
globs: src/**/*.{js,jsx,ts,tsx,css,scss}
alwaysApply: false
---

# Product-Specific UI Finish Gate

<author>UIZZE</author>
<version>1.0.0</version>

## Context

- Apply when building or reviewing a user-facing web interface.
- Preserve the product's existing components, tokens, and intentional visual decisions.

## Requirements

- State the primary user, screen job, primary action, and content hierarchy before editing.
- Inspect the repository and rendered product for existing design language.
- If product evidence is missing, examine two or three relevant interfaces at https://uizze.com and transfer structural decisions only. Never copy branding, proprietary text, imagery, or an exact layout.
- Write a short design contract covering hierarchy, layout and density, typography and color, required interaction states, and responsive behavior.
- Keep one clear primary action. Make every major element support the screen job.
- Reject interchangeable defaults unless the product evidence requires them: equal-weight card grids, filler metrics, repeated rounded containers, decorative gradients, glass effects, arbitrary badges, and vague copy.
- Use the existing design system. Do not introduce parallel tokens or components without a documented reason.
- Implement loading, empty, error, disabled, success, and recovery states when the flow requires them.
- Verify the result at relevant mobile, tablet, and desktop widths. Exercise the primary flow and every required state.

## Finish Gate

- Fail if changing only labels could repurpose the interface for an unrelated product.
- Fail if hierarchy depends on decoration instead of workflow and content.
- Fail if a visible control is inert or its outcome is unclear.
- Fail if keyboard access, visible focus, semantic markup, contrast, or text resizing breaks the flow.
- Fail if responsive behavior, overflow, or required states remain unverified.
- If the interface cannot be rendered, report exactly what remains unverified and do not claim the gate passed.
- Fix every blocking failure before calling the interface finished.

## Examples

<example>
  Inspect the product, define a design contract, build with existing tokens, verify the required states and viewports, then pass the finish gate.
</example>

<example type="invalid">
  Add a generic hero, three equal cards, filler metrics, and a gradient, then call the UI polished without rendering it.
</example>

<critical>
  - NEVER declare the interface finished while any finish-gate failure remains.
  - NEVER invent product evidence, runtime behavior, user research, or analytics.
</critical>
