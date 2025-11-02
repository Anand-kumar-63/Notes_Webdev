https://clerk.com/docs/reference/nextjs/clerk-middleware

# Event driven Architecture 
![[Pasted image 20251027202607.png]]

![[Pasted image 20251028203037.png]]

# Clerk UseSignUp hook 
Readmore - 
https://clerk.com/docs/reference/javascript/sign-up

# Clerk overview 
Readmore - 
https://clerk.com/docs/reference/nextjs/overview

# ClerkClients Users object

- The **`clerkClient.users`** object is part of the **Clerk Backend API** client (like for JavaScript/Next.js) and is used to **manage user accounts** programmatically. It provides methods to perform server-side operations on users, such as creating, retrieving, updating, and deleting user records.

## Key Functions of `clerkClient.users`

  The `clerkClient.users` object exposes various methods to interact with your application's user base in the backend:
- **`createUser(params)`**: Creates a new user in your Clerk application.
- **`getUser(userId)`**: Retrieves a specific **User object** by its ID.
- **`getUserList(params)`**: Fetches a list of all users, often supporting pagination and filtering.
- **`updateUser(userId, params)`**: Modifies properties of an existing user.
- **`deleteUser(userId)`**: Permanently deletes a user record.
- **`updateUserMetadata(userId, params)`**: Specifically updates the `publicMetadata` or `privateMetadata` fields of a user.

### The Clerk **User Object**
The methods in `clerkClient.users` often return or operate on a **User object**, which holds all the information for a single user, including:
- **Authentication Identifiers**: Email addresses, phone numbers, or a username.
- **Profile Data**: First name, last name, profile picture, etc.
- **Metadata**:
    - **`publicMetadata`**: Custom data accessible from both the Frontend and Backend APIs.
    - **`privateMetadata`**: Custom data only accessible from the Backend API.
- **Account Management Data**: Associated external accounts (like Google or GitHub), organization memberships, sessions, and multi-factor authentication details (like TOTP or backup codes).

- https://clerk.com/docs/guides/users/managing#in-the-clerk-dashboard

# Clerk user methods 
Let’s go deep into **Clerk’s backend methods** — the ones you’ll use in Node.js, Express, or Next.js **API routes** (not in React components).

---
## 🧩 1. Importing Clerk Backend Client
When you use Clerk on the backend, you’ll usually import the **Clerk SDK client** like this:
`import { clerkClient } from "@clerk/nextjs/server";`
or if you’re using Express or Node (without Next.js):

`import Clerk from "@clerk/clerk-sdk-node"; const clerkClient = Clerk();`

> ✅ `clerkClient` is your **bridge to the Clerk API**, just without you manually calling the REST endpoints.

It has methods for:
- Managing **users**
- Managing **sessions**
- Managing **emails / phones**
- Verifying **tokens**
- Managing **organizations**
---
## 👤 2. **User Methods**
You can perform almost anything on a user — create, fetch, update, or delete.

| Action        | Method                                        | Description                          |
| ------------- | --------------------------------------------- | ------------------------------------ |
| Get a user    | `clerkClient.users.getUser(userId)`           | Fetches full user data by ID         |
| Get all users | `clerkClient.users.getUserList()`             | Returns list of all users            |
| Create user   | `clerkClient.users.createUser({...})`         | Programmatically create a new user   |
| Update user   | `clerkClient.users.updateUser(userId, {...})` | Modify attributes (like name, email) |
| Delete user   | `clerkClient.users.deleteUser(userId)`        | Removes a user account               |

Example:
`const user = await clerkClient.users.getUser("user_2qSdfL3..."); console.log(user.emailAddresses);`

Create user example:
`await clerkClient.users.createUser({   emailAddress: "test@example.com",   password: "password123",   firstName: "John", });`

---
## 🔐 3. **Session Methods**
Sessions represent active logins. You’ll mostly **verify or fetch** them.

|Action|Method|Description|
|---|---|---|
|Get session|`clerkClient.sessions.getSession(sessionId)`|Retrieve info about an existing session|
|Revoke session|`clerkClient.sessions.revokeSession(sessionId)`|Log a user out|
|Verify session token|`clerkClient.sessions.verifySessionToken(token)`|Validates a session token (JWT)|

Example:
`const session = await clerkClient.sessions.getSession("sess_123..."); console.log(session.userId);`

Or verify a token:
`import { verifySessionToken } from "@clerk/nextjs/server";  const { userId } = await verifySessionToken(token);`

---

## 📧 4. **Email & Phone Methods**
For managing user contact information.

| Action            | Method                                                 | Description                        |
| ----------------- | ------------------------------------------------------ | ---------------------------------- |
| Get email address | `clerkClient.emailAddresses.getEmailAddress(id)`       | Fetches details about an email     |
| Create email      | `clerkClient.emailAddresses.createEmailAddress({...})` | Adds a new email to a user         |
| Delete email      | `clerkClient.emailAddresses.deleteEmailAddress(id)`    | Removes email address from account |

Same pattern applies for `phoneNumbers`.

---
## 🧾 5. **Organization Methods** (if you use orgs)

| Action     | Method                                                       | Description             |
| ---------- | ------------------------------------------------------------ | ----------------------- |
| Get org    | `clerkClient.organizations.getOrganization(orgId)`           | Fetch details of an org |
| Create org | `clerkClient.organizations.createOrganization({...})`        | Make a new organization |
| Update org | `clerkClient.organizations.updateOrganization(orgId, {...})` | Edit org data           |
| Delete org | `clerkClient.organizations.deleteOrganization(orgId)`        | Remove org              |
Example:
`const org = await clerkClient.organizations.createOrganization({   name: "My Dev Team",   slug: "my-dev-team", });`

---
## 🪄 6. **Backend Authentication Helpers**
If you’re using **Next.js**, Clerk provides special helper functions.
### `getAuth(req)`
Used in API routes or middleware to identify who’s making the request.
Example:
``import { getAuth } from "@clerk/nextjs/server";  export default function handler(req, res) {   const { userId, sessionId } = getAuth(req);      if (!userId) {     return res.status(401).json({ error: "Not signed in" });   }    res.status(200).json({ message: `Welcome user ${userId}` }); }``

This is one of the most **important** backend functions — it extracts user/session data from Clerk’s cookies or headers.

---
## ⚙️ 7. **Webhooks & JWT Templates**
You can extend backend logic with:
- **Webhooks** → when Clerk events happen (`user.created`, `email.updated`, etc.)
- **JWT Templates** → Clerk issues signed tokens your backend can verify and trust
For example, if you’re giving a user access to your own API:
`const token = await clerkClient.users.getUserToken(userId, "my-jwt-template");`

---
## 🧠 8. **Under the Hood**
All these methods are wrappers around Clerk’s REST API endpoints:
- `clerkClient.users.getUser(id)` → GET `/v1/users/{id}`
- `clerkClient.sessions.verifySessionToken(token)` → POST `/v1/sessions/verify`
- `clerkClient.organizations.createOrganization()` → POST `/v1/organizations`
    
So when you use the SDK, it handles:
- Authorization (adds your secret key)
- Base URLs
- Response parsing
- Error handling



# Specific to NEXT JS 
## 🧩 1. Importing Backend Functions
In your **Next.js backend (API routes or route handlers)**, you’ll usually import from:
`import { getAuth, clerkClient, verifySessionToken } from "@clerk/nextjs/server";`
### 🧠 What each one does:

| Function                    | Purpose                                                                                     |
| --------------------------- | ------------------------------------------------------------------------------------------- |
| `getAuth(req)`              | Extracts the user/session info from the current request                                     |
| `clerkClient`               | The official Clerk API client — lets you manage users, sessions, orgs, etc.                 |
| `verifySessionToken(token)` | Verifies a raw session token manually (for custom auth flows, e.g., mobile or desktop apps) |

---
## ⚙️ 2. `getAuth(req)` – Identify the Current User
This is the **most common** backend method.
``import { getAuth } from "@clerk/nextjs/server";  export default async function handler(req, res) {   const { userId, sessionId, getToken } = getAuth(req);    if (!userId) {     return res.status(401).json({ error: "Not signed in" });   }    res.status(200).json({ message: `Welcome user ${userId}` }); }``

### 💡 Behind the scenes:
- `getAuth(req)` reads the **Clerk cookies** attached to the request.
- Clerk verifies them using your `CLERK_SECRET_KEY`. 
- Returns info like:
    `{   userId: 'user_2qX123...',   sessionId: 'sess_123...',   getToken: [AsyncFunction] }`
- You can call `await getToken()` to get a **JWT** for API calls or external services.
    
---
## 👤 3. Using `clerkClient` – Managing Users, Sessions, and More
### 📍 Example: Fetch a user
```js
import { clerkClient } from "@clerk/nextjs/server";  
export default async function handler(req, res) 
{ 
const user = await clerkClient.users.getUser("user_2qSdfL3...");   res.status(200).json(user); 
}
```

### 📍 Example: Create a new user

`const newUser = await clerkClient.users.createUser({   emailAddress: ["newuser@example.com"],   firstName: "John",   lastName: "Doe",   password: "Password123", });`
### 📍 Example: Update a user

`await clerkClient.users.updateUser("user_2qSdfL3...", {   firstName: "Johnny", });`
### 📍 Example: Delete a user

`await clerkClient.users.deleteUser("user_2qSdfL3...");`
All these methods internally call Clerk’s REST API using your **`CLERK_SECRET_KEY`** — so they’re safe only to use **server-side** (never client-side).

---

## 🔐 4. Session Verification on API Routes

If you have a custom API route and want to verify the session token manually (instead of using Clerk’s middleware):

`import { verifySessionToken } from "@clerk/nextjs/server";  export default async function handler(req, res) {   const token = req.headers.authorization?.replace("Bearer ", "");   const session = await verifySessionToken(token);    if (!session) {     return res.status(401).json({ error: "Invalid session token" });   }    res.status(200).json({ message: "Session verified", session }); }`

This is useful when you’re building something like:
- A **desktop app** or **mobile app** that calls your API directly
- A **custom frontend** that uses Clerk’s session token header
    
---
## 🧾 5. Getting a JWT for Other APIs
Sometimes you need to call another backend (say, a Flask or Go API) and need a signed JWT.

`import { getAuth } from "@clerk/nextjs/server";  export default async function handler(req, res) {   const { getToken } = getAuth(req);   const token = await getToken({ template: "my-backend-template" });    // Send this token to your other backend   res.status(200).json({ token }); }`

> This uses a **JWT Template** you define in your Clerk dashboard.  
> The backend can then verify that token using the public key Clerk provides.

---
## 🧰 6. Protecting API Routes in Next.js (the simple way)
Instead of verifying manually, you can use **middleware**:
`middleware.ts`

`import { authMiddleware } from "@clerk/nextjs";  export default authMiddleware({   publicRoutes: ["/", "/about"], // routes that don't need login });  export const config = {   matcher: ["/((?!_next|api/webhooks).*)"], };`

Now all protected routes automatically get user info injected into `getAuth(req)`.

---
## 🏗️ 7. Putting It Together (Full Example)
`pages/api/userinfo.js`

`import { getAuth, clerkClient } from "@clerk/nextjs/server";  export default async function handler(req, res) {   const { userId } = getAuth(req);    if (!userId) {     return res.status(401).json({ error: "Not signed in" });   }    const user = await clerkClient.users.getUser(userId);   res.status(200).json({ email: user.emailAddresses[0].emailAddress }); }`

✅ This endpoint:
- Reads current user from Clerk cookie
- Fetches their full data from Clerk
- Returns their email to the frontend

---
## 🚀 8. In Summary

| Method                      | Purpose                                      |
| --------------------------- | -------------------------------------------- |
| `getAuth(req)`              | Reads user/session info from current request |
| `clerkClient.users.*`       | Manage user data programmatically            |
| `clerkClient.sessions.*`    | Manage user sessions                         |
| `verifySessionToken(token)` | Manually validate a Clerk session token      |
| `getToken()`                | Generate a custom JWT for other APIs         |
| `authMiddleware()`          | Protect routes globally in Next.js           |
