---
description: Use server actions when creating or updating logic that mutates data in Next.js
globs: src/**/*.{ts,tsx}
alwaysApply: false
---

# Server Actions Rules

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- Applies to Next.js v15 projects leveraging Server Actions for data mutations
- Server actions can be called from both Server Components and Client Components
- Uses "use server" directive (inline or module-level) to mark actions as server-only

## Requirements

- Insert `"use server"` at the start of async function bodies (e.g., `function createPost() { "use server"; ... }`).
- Place `"use server"` at the file's top to mark all exported functions.
- Set `action={myServerAction}` on `<form>`; actions receive a `FormData` object.
- Call actions from `onClick`, `onChange`, or `useEffect`.
- Pass actions as props if named `action` or ending in `Action` to avoid serialization issues.
- Use `POST` for server actions.
- Ensure actions support progressive enhancement and work without JavaScript.
- Prevent automatic page refresh post-hydration.
- Encrypt closed-over values in inline actions before sending them to the client.
- Avoid placing sensitive data in closures.
- Let unused actions be stripped at build time; used actions get secure, non-deterministic IDs.
- Treat each server action as a public endpoint; enforce authentication checks.
- Rely on built-in `Origin & Host` validation (only same-origin requests succeed).
- Configure `serverActions.allowedOrigins` in `next.config.ts` for advanced control.
- Refresh caches with `revalidatePath("/...")` or `revalidateTag("my-tag")`.
- Call `redirect("/some-path")` after an action completes (outside `try/catch`).
- Use native HTML validation (`required`, `type="email"`).
- Validate `FormData` server-side with `zod` or similar libraries.
- Return serializable objects or throw errors for custom handling.
- Apply `useOptimistic` to update UI before the server response.
- Revert or adjust state based on the actual server result.
- Import actions from `"use server"` files in Client Components (`"use client"`) or define them inline.
- Use `useActionState` and `useFormStatus` to manage loading, errors, and optimistic updates.
- Access cookies with `await cookies()` (get, set, delete).
- Invoke actions in `useEffect` on mount or dependency change.

## Examples

<example>

   ```ts
   // Inline server action in a Server Component
   export default function Page() {
      async function createPost(formData: FormData) {
      "use server";
      // mutate data, revalidate, etc.
      }

      return <form action={createPost}>...</form>;
   }
   ```

</example>

<example>

   ```ts
   // Server action in a separate file
   // app/actions.ts
   "use server";

   export async function createUser(formData: FormData) {
      // ...
   }

   // app/button.tsx (Client Component)
   "use client";
   import { createUser } from "./actions";

   export function Button() {
      return <button onClick={() => createUser()}>Create</button>;
   }
   ```

</example>

<example type="invalid">

   ```ts
   // Missing "use server" => Not recognized as a server action
   export async function updateData(formData: FormData) {
      // ...
   }
   ```

</example>
