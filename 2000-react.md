---
description: Use React rules when building UI to produce maintainable components
globs: src/**/*.tsx
alwaysApply: false
---

# React Rules

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- For developing React components within Next.js
- Emphasizes clear, safe JSX and modern React 19 practices

## Requirements

- In Next.js 15 and React 19, client components must start with `"use client"` at the top.
- Server components require no directive. Server action files and any functions that call server actions should start with `"use server"`.
- Never use `import * as React from "react"`, do explicit imports instead.
- Use `{condition ? <Element /> : null}` for conditional JSX rendering; avoid `&&`.
- Destructure props/state for clarity.
- Keep boolean props accurate (e.g., `<Button disabled />`).
- Set `displayName` on complex components or contexts.
- Clean up side effects in `useEffect` (e.g., timers, listeners).
- To handle refs in React 19 pass `ref` as a standard prop. `forwardRef` and string refs like `ref="myRef"` are deprecated.
- No direct DOM manipulation (e.g., `findDOMNode`) or `dangerouslySetInnerHTML`.
- Avoid using array index as a key; prefer stable IDs.
- Use `rel="noreferrer noopener"` with `target="_blank"`.
- Use `useCallback`/`React.memo`/`useMemo` only if performance gains are measured.
- Keep naming consistent (e.g., `[count, setCount]`) while destructuring `useState`.
- Custom hooks must call at least one React hook. Use lazy initialization in `useState` for expensive computations.
- Default props for arrays or objects, define them as constants outside the component to avoid creating new references on every render.
- Avoid calling a `useState` setter in `useEffect` without a functional update or guard.
- Avoid default Props in Function Components using inline referential values. Use ES6 defaults or optional props with constants declared outside the component.
- Avoid defining Components inside another component’s render. Extract them.
- Avoid unstable Default Props: Do not use inline array/object literals as defaults; define them outside.
- For `useCallback(fn, deps)` provide stable dependencies (props, state) to control updates. Declare hook at the component’s top level (not in loops/conditions).

<example>
  import { useCallback } from "react";

  function ProductPage({ productId }) {
    const handleSubmit = useCallback(() => {
      post("/product/" + productId + "/buy");
    }, [productId]);

    return <ShippingForm onSubmit={handleSubmit} />;
  }
</example>

<example type="invalid">
  // Missing dependencies => new function on every render
  const handleClick = useCallback(() => {
    // ...
  });
</example>
