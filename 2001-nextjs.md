---
description: Use these rules when building Next.js projects
globs: src/**/*.{ts,tsx}, next.config.ts
alwaysApply: false
---

# Next.js Rules

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- For building Next.js v15 (App Router) projects
- Guides server vs. client component usage

## Requirements

- React 19 is required for Next.js 15.
- Default `layout.tsx` and `page.tsx` to server components. Place client components into them.
- Use client components for local state or interactivity.
- Use `<Link>` instead for navigation unless `useRouter` is essential; `<a>` is disallowed.
- Provide `loading.tsx` and e.g. `<Suspense fallback={<Skeleton />}>` for asynchronous data fetching. Use Shadcn UI `Skeleton` for loading states.
- Prefer server actions instead of client API calls.
- Maintain Edge Runtime middleware.ts compatibility (no Node.js APIs).
- Do not pass server-only event handlers or data to client components.
- `useFormState` replaced by `useActionState`.
- Imports: `ImageResponse` moved from `next/server` to `next/og`.
- Async APIs: `cookies`, `headers`, `draftMode`, and `params` return Promises—use `await` or `use()`.
- Caching: Fetch requests aren’t cached by default; set `cache: "force-cache"` if needed.
- Geo/IP: Removed from `NextRequest`; use `@vercel/functions` for `ipAddress` or `geolocation`.
- Route Handlers: No caching unless explicitly set (`dynamic = "force-static"`).

## Examples

<example>
  // server component
  export default async function Page() {
    const data = await getData(); // server action
    return <div>{data}</div>;
  }
</example>

<example type="invalid">
  "use client";
  export default async function Page() {
    const data = await getData();
    return <div>{data}</div>;
  }
</example>
