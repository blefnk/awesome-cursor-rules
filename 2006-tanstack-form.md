---
description: Implement TanStack Form when creating or updating forms to achieve robust validation and a type-safe solution
globs: src/**/*.{ts,tsx}
alwaysApply: false
---

# TanStack Form Rules

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- Use when building or refactoring forms with @tanstack/react-form.
- Ensure consistent validation, submission, and type safety.
- Treat TanStack Form as a fully controlled, type-safe form library.
- Leverage reactive data binding, advanced validation, and minimal boilerplate.

## Requirements

- Install: `bun add @tanstack/react-form`; lock package versions.
- Use `@tanstack/react-form/nextjs` with React Server Actions and `createServerValidate`.
- Manage all input values via centralized form state.
- Use helper functions (`formOptions`, `createFormHook`) to keep configs DRY.
- Choose inline or schema-based validation (Zod, Valibot, ArkType).
- Apply `"use client"` and `"use server"` where needed.
- Create a form instance via `useForm({ defaultValues, onSubmit })` or `createFormHook`.
- Define strong types in `defaultValues`.
- Wrap inputs with `<form.Field name="..." />` or custom components.
- Configure shared defaults via `formOptions`.
- Attach sync/async validators via the `validators` prop.
- Return clear, user-friendly error messages.
- Access form state via `.state`, `.meta`, or `useStore(form.store)`.
- Subscribe to specific state slices using `form.Subscribe` or selectors.
- Monitor field flags: `isTouched`, `isPristine`, `isDirty` (`isDirty` remains `true` after any change).
- Read and update form state exclusively for predictable behavior and testability.
- Separate server-only logic from client code.
- Enable repeating input sets with `mode="array"` on `<form.Field>`.
- Manage arrays using `pushValue`, `removeValue`, `swapValues`, `moveValue`.
- Ensure nested fields retain types and validation.
- Reduce boilerplate via `createFormHookContexts` or `createFormHook`.
- Merge server-supplied state via `mergeForm`/`useTransform` for partial hydration and optimistic UI.
- Design multi-step forms to preserve and merge state.
- Attach field listeners for side effects (e.g., reset related fields on change).
- Define shared config in `formOptions` for consistent client/server defaults.
- Use field API methods (`handleChange`, `handleBlur`, etc.) to manage inputs.
- Enforce strict TypeScript, relying on type inference to minimize manual generics.
- Maintain scalable, predictable, type-safe form behavior.

## Examples

<example>
  // Basic usage with React
  import { useForm } from "@tanstack/react-form";

  function MyForm() {
    const form = useForm({
      defaultValues: { firstName: "", age: 0 },
      onSubmit: ({ value }) => alert(JSON.stringify(value)),
    })

    return (
      <form
        onSubmit={(e) => {
          e.preventDefault()
          form.handleSubmit()
        }}
      >
        <form.Field name="firstName">
          {(field) => (
            <>
              <label htmlFor={field.name}>First Name</label>
              <input
                id={field.name}
                value={field.state.value}
                onChange={(e) => field.handleChange(e.target.value)}
                onBlur={field.handleBlur}
              />
            </>
          )}
        </form.Field>
        <button type="submit">Submit</button>
      </form>
    )
  }
</example>

<example type="invalid">
  // Missing name prop => Field won't map to form state
  <form.Field>
    {(field) => <input />}
  </form.Field>
</example>
