# Polling 
![[Pasted image 20251110180730.png]]
![[Pasted image 20251110181019.png]]

# use Mutation 
![[Pasted image 20251110183915.png]]
## delete post 
## update post using react-query [use mutation hook]


# UseinfiniteQuery 

# Enabled
In TanStack Query (formerly React Query), the `enabled` option is a boolean switch that tells the query **whether it is allowed to run**.
By default, `enabled` is `true`, meaning the query automatically runs as soon as the component mounts. When you set it to `false`, the query is "disabled" or "paused" until you turn it back on.
Here is the breakdown of how to use it and the most common patterns.
### 1. The Basic Concept

If you pass `enabled: false`, the query will **not** automatically fetch when the component mounts, and it will ignore background refetches (like window focus or network reconnects).

JavaScript

```ts
const { data, status, fetchStatus } = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodos,
  enabled: false, // Query will NOT run automatically
})
```
- **Status:** The query will start in a `pending` status (because it has no data)
- **Fetch Status:** The `fetchStatus` will be `idle` (because it isn't trying to fetch).
---
### 2.Use Case: Dependent Queries (Most Common)
This is the most standard use case. You often can't run "Query B" until "Query A" has finished.
**Example:** Fetch a User first, _then_ fetch their Projects.
JavaScript
```ts
// 1. Fetch the User
const { data: user } = useQuery({
  queryKey: ['user', email],
  queryFn: fetchUser,
})
const userId = user?.id
// 2. Fetch Projects (Dependent on User)
const { data: projects } = useQuery({
  queryKey: ['projects', userId],
  queryFn: () => fetchProjects(userId),
  // Only run this query if 'userId' exists (is not undefined/null)
  enabled: !!userId, 
})
```
In this scenario, the `projects` query sits idle until `userId` becomes available. As soon as `userId` is defined, `enabled` flips to `true` and the query runs.

---
### 3. Use Case: Lazy Queries (Wait for User Input)
Sometimes you don't want to fetch data until a user performs an action, like typing in a search bar or applying a filter.
**Example:** Only search when the user types something.
JavaScript
```ts
const [filter, setFilter] = useState('')
const { data } = useQuery({
  queryKey: ['search', filter],
  queryFn: () => searchItems(filter),
  // Disable the query if the filter is empty string
  enabled: filter.length > 0, 
})
```
### 4. A Note on `refetch()` vs `enabled`
A common mistake is setting `enabled: false` and trying to force the query to run later by calling the `refetch()` function manually (like on a button click).
While this _technically_ works, it is often considered an anti-pattern because you lose the declarative power of TanStack Query.
- **Anti-Pattern:** `enabled: false` + `onClick={() => refetch()}`.
- **Better approach:** Use a state variable to toggle `enabled` to true when the button is clicked.
### 5. TypeScript Tip: `skipToken` (v5+)
If you are using **TanStack Query v5** with TypeScript, using `enabled: false` can sometimes mess up type inference (TypeScript might think `data` is available when it isn't).
The recommended way to disable a query in v5 is using `skipToken`:
JavaScript
```ts
import { skipToken, useQuery } from '@tanstack/react-query'
const { data } = useQuery({
  queryKey: ['todos', userId],
  // If userId is missing, pass skipToken instead of the function
  queryFn: userId ? () => fetchTodos(userId) : skipToken, 
})
```

This achieves the same result as `enabled: !!userId` but is much safer for TypeScript types.