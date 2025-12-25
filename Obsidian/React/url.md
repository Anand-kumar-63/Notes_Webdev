# url and query params
Accessing URL parameters depends on the framework or routing library you are using. Below are the **most common and correct approaches**, with clear examples.

---

## 1. **Next.js (App Router – `app/` directory)**
### Example URL
`/dashboard/123`
### Folder Strucure
`app/dashboard/[id]/page.tsx`
### Access params
`type PageProps = {   params: {     id: string;   }; };  export default function Page({ params }: PageProps) {   console.log(params.id); // "123"   return <div>Workspace ID: {params.id}</div>; }`

### Client Component (if needed)
`"use client";  import { useParams } from "next/navigation";  export default function Page() {   const params = useParams();   console.log(params.id); // "123" }`

---
## 2. **Next.js (Pages Router – `pages/` directory)**
### File
`pages/dashboard/[id].tsx`
### Access params
`import { useRouter } from "next/router";  export default function Page() {   const router = useRouter();   const { id } = router.query;    console.log(id); // "123"   return <div>ID: {id}</div>; }`

---
## 3. **React Router (v6)**
### Route
`<Route path="/dashboard/:id" element={<Dashboard />} />`
### Access params
`import { useParams } from "react-router-dom";  export default function Dashboard() {   const { id } = useParams();   console.log(id); // "123" }`

---
## 4. **Query Parameters (`?id=123`)**
### URL
`/dashboard?id=123`
### Next.js (App Router)
`import { useSearchParams } from "next/navigation";  const searchParams = useSearchParams(); const id = searchParams.get("id");`
### React / Browser API
`const params = new URLSearchParams(window.location.search); const id = params.get("id");`

---
## Quick Summary

| Framework            | Dynamic URL (`/user/123`)   | Query (`?id=123`)   |
| -------------------- | --------------------------- | ------------------- |
| Next.js App Router   | `params.id` / `useParams()` | `useSearchParams()` |
| Next.js Pages Router | `router.query.id`           | `router.query.id`   |
| React Router         | `useParams()`               | `useSearchParams()` |
|                      |                             |                     |



# URL Parameters (Route / Path Parameters)

### What they are
They are **part of the URL path itself** and usually represent a **required resource identifier**.
### Example
`/users/42`
Here, `42` is a **URL parameter**.
### Typical use cases
- Identifying a specific resource
- Pages that **must exist** for a given ID
- SEO-friendly URLs

### Framework behavior
- URL structure is fixed
- Route **will not match** if the parameter is missing
- Often maps to database primary keys
    
### Example (Next.js / React Router)
`/users/:id`
`const { id } = useParams();`
### Real-world meaning
> “Show me the user whose ID is 42.”

---
## 2. Query Parameters
### What they are
They are **key-value pairs** appended after `?` in the URL and are **optional**.
### Example
`/users?role=admin&page=2`
Here:
- `role=admin`
- `page=2`

are **query parameters**.
### Typical use cases
- Filtering
- Sorting
- Pagination
- Search
- Optional configuration
### Framework behavior
- URL works even if query params are missing
- Order does not matter
- Multiple params allowed
### Example
`const page = searchParams.get("page");`
### Real-world meaning
> “Show users, filtered by admin role, page 2.”
---
## 3. Key Differences (Side-by-Side)

| Aspect         | URL Parameters      | Query Parameters      |
| -------------- | ------------------- | --------------------- |
| Position       | Inside path         | After `?`             |
| Required       | Yes                 | Optional              |
| Purpose        | Identify a resource | Modify or filter data |
| SEO            | Better              | Neutral               |
| Order matters  | Yes                 | No                    |
| Route matching | Required to match   | Not required          |
| Example        | `/posts/10`         | `/posts?page=1`       |

---
## 4. When to Use What (Rule of Thumb)
### Use **URL parameters** when:
- The page **cannot exist without it**
- You are identifying a **single entity**
- The parameter defines the route
    
**Example**
`/product/iphone-15`

---
### Use **Query parameters** when:
- Data is **optional**
- You are **filtering or modifying** the result
- Multiple configurations are needed
    
**Example**
`/products?brand=apple&sort=price`

---

## 5. Combined Usage (Very Common)
`/products/iphone-15?color=black&storage=256`
- `iphone-15` → URL parameter (what product)
- `color`, `storage` → query parameters (how to view it)
    
---
## 6. Interview-Ready One-Liner

> **URL parameters identify _what_ resource you want; query parameters define _how_ you want it.**

