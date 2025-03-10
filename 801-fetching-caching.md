---
description: Standardize fetching and caching in Next.js for performance and security.
globs: src/**/*.{ts,tsx}
alwaysApply: false
---

# Fetching and Caching in Next.js

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- Applies to Next.js 15 (Server & Client data fetching).
- Covers caching, revalidation, parallel fetching, preloading, and security.
- Defaults to `fetch` with `cache: "no-store"`, requiring explicit caching.

## Requirements

- Prefer server components or direct DB calls for fetching.
- Prerender static routes unless using cookies, headers, or query params.
- Force dynamic rendering as needed: `export const dynamic = "force-dynamic";`
- Cache DB calls with `unstable_cache` or `react.cache`.
- Use React's taint APIs to prevent exposing sensitive data.
- Mark client components with `"use client"`.
- Use SWR or React Query instead of `useEffect` for data fetching.
- Provide a loading state: `if (!data) return <div>Loading...</div>;`
- Never expose tokens, secrets, or DB calls in client-side code.
- `fetch` is not cached unless explicitly set: `fetch(url, { cache: "force-cache" });`
- Use `unstable_cache` / `react.cache` for expensive fetch calls: `import { unstable_cache } from "next/cache"; const getPosts = unstable_cache(async () => { return db.select().from(posts); }, ["posts"], { revalidate: 3600 });`
- Invalidate caches on demand: `revalidatePath("/path"); revalidateTag("posts");`
- Sequential Fetching (dependent calls): `const first = await fetchFirst(); const second = await fetchSecond(first.id);`
- Parallel Fetching (independent calls): `const [data1, data2] = await Promise.all([fetchData1(), fetchData2()]);`
- Fetch in parallel whenever possible to optimize performance.
- Preload before rendering: `export const preload = (id) => void getItem(id);`
- `preload(id)` initiates a fetch in advance. Combine with `cache()` and `"server-only"` for safe server data fetching.
- Use `server-only` to prevent client-side execution and restrict execution to the server.
- Avoid waterfall fetches by initiating early.
- Keep DB calls, tokens, and secrets on the server.
- Use React’s taint APIs to prevent data leaks: `import { experimental_taintObjectReference } from "react"; experimental_taintObjectReference("Do not leak user data", user);`.
- Throw `notFound()` if data is missing.
- Use `<Suspense>` or `loading.js` for server fallbacks.
- Handle client errors with React Query, SWR, or error boundaries.

## Examples

<example>

   ✅ Basic Server-Side Fetch

   ```ts
   export default async function Page() {
      const data = await fetch("https://api.vercel.app/blog");
      const posts = await data.json();
      return <ul>{posts.map((p) => <li key={p.id}>{p.title}</li>)}</ul>;
   }
   ```

</example>

<example>

   ✅ Caching with `force-cache`

   ```ts
   export default async function Page() {
      const data = await fetch("https://api.vercel.app/blog", { cache: "force-cache" });
      const posts = await data.json();
      return <ul>{posts.map((p) => <li key={p.id}>{p.title}</li>)}</ul>;
   }
   ```

</example>

<example>

   ✅ DB Call with `unstable_cache`

   ```ts
   import { unstable_cache } from "next/cache";
   import { db, posts } from "@/lib/db";

   const getPosts = unstable_cache(async () => {
      return db.select().from(posts);
   }, ["posts"], { revalidate: 3600 });

   export default async function Page() {
      const allPosts = await getPosts();
      return <ul>{allPosts.map((post) => <li key={post.id}>{post.title}</li>)}</ul>;
   }
   ```

</example>

<example>

   ✅ Client-Side Fetching

   ```ts
   "use client";
   import { useState, useEffect } from "react";

   export default function ClientPage() {
      const [posts, setPosts] = useState([]);
      useEffect(() => {
         async function loadPosts() {
            const res = await fetch("https://api.vercel.app/blog");
            const data = await res.json();
            setPosts(data);
         }
         loadPosts();
      }, []);

      if (!posts.length) return <div>Loading...</div>;
      return <ul>{posts.map((p) => <li key={p.id}>{p.title}</li>)}</ul>;
   }
   ```

</example>

<example type="invalid">

   ❌ Missing `"use client"`

   ```ts
   export default function Page() {
      "use client";
      // Entire route becomes client-side unnecessarily.
   }
   ```

</example>
