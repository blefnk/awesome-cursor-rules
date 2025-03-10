---
description: Manage async client-side data with TanStack Query (AKA React Query) (queries, mutations, invalidation)
globs: src/**/*.{ts,tsx}
alwaysApply: false
---

# TanStack Query Rules

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- Use TanStack Query for async data management in React.
- Support queries, mutations, and targeted invalidation.

## Requirements

### 1. Installation & Setup

- Install: `bun add @tanstack/react-query && bun add -D @tanstack/eslint-plugin-query`
- Use `useQuery` with a unique `queryKey` and a `queryFn`.
- Manage query states: `isPending`, `isError`, `isSuccess`, `status`.
- Use `fetchStatus` for background fetching detection.
- Use `useMutation` with a `mutationFn`.
- Handle states: `isIdle`, `isPending`, `isError`, `isSuccess`.
- Utilize callbacks: `onMutate`, `onSuccess`, `onError`, `onSettled`.
- Use `mutateAsync` for promise-based execution.
- Reset mutation state with `mutation.reset()` if needed.
- Invalidate queries via: `queryClient.invalidateQueries({ queryKey: [...] });`
- Use `exact: true` or predicate functions for finer control.
- Wrap the app in `<QueryClientProvider>` with a `QueryClient`.
- Ensure unique query/mutation keys for proper caching.
- Use callback options for side effects and optimistic updates.

## Examples

<example>

    Basic Usage

    ```ts
    import {
    useQuery,
    useMutation,
    useQueryClient,
    QueryClient,
    QueryClientProvider,
    } from '@tanstack/react-query';
    import { getTodos, postTodo } from '../my-api';

    const queryClient = new QueryClient();

    function App() {
    return (
        <QueryClientProvider client={queryClient}>
        <Todos />
        </QueryClientProvider>
    );
    }

    function Todos() {
    const queryClient = useQueryClient();
    const query = useQuery({ queryKey: ['todos'], queryFn: getTodos });

    const mutation = useMutation({
        mutationFn: postTodo,
        onSuccess: () => queryClient.invalidateQueries({ queryKey: ['todos'] }),
    });

    return (
        <div>
        <ul>
            {query.data?.map((todo) => (
            <li key={todo.id}>{todo.title}</li>
            ))}
        </ul>
        <button
            onClick={() =>
            mutation.mutate({ id: Date.now(), title: 'Do Laundry' })
            }
        >
            Add Todo
        </button>
        </div>
    );
    }
    ```

</example>

<example type="invalid">

    ```ts
    const query = useQuery({ queryFn: getTodos });
    // No unique queryKey; required for caching and refetching.
    ```

</example>
