-  Prisma is a modern DB toolkit to query, migrate and model your database (https://prisma.io)
- ![[Pasted image 20251021153559.png]]

- ![[Pasted image 20251021153932.png]]

-  [Connect the backend to the database without the need of writing the complex queries...]
- ![[Pasted image 20251021154401.png]]
-  [Prisma migration ]
  ![[Pasted image 20251022120031.png]]

# Prisma migration
Migration in **Prisma** is a systematic process for managing changes to your **database schema** in a reproducible and version-controlled way.

## 1. Schema Definition in Prisma Schema File
You define your database models and their relationships using the **Prisma Schema Language (PSL)** in the `schema.prisma` file. This file acts as the single source of truth for your application's data models.
## 2. Generating Migration Files
When you make changes to your models in the `schema.prisma` file (e.g., adding a new table, changing a column type, or creating a relationship), you use the Prisma CLI command 
```js
npx prisma migrate dev
```
(or similar commands):
- It **compares** the current state of your `schema.prisma` file with the actual state of the database (or the previous migration history).
- It **calculates** the necessary SQL statements to bring the database schema in sync with your Prisma schema.
- It **generates** a new, timestamped **migration file** (a plain SQL file) containing these statements and places it in a dedicated `prisma/migrations` folder. This file is committed to your version control (like Git).    
## 3. Applying Migrations
The migration files are then used to actually modify t,he database structure:
- **During development**, the `prisma migrate dev` command automatically executes the generated SQL against your development database.
- **In production/staging**, you use `npx prisma migrate deploy` to apply all pending migration files against the target database, ensuring your deployed application's database matches the schema defined in your committed files.

#Example 
## A Realistic Example: Renovation History
Imagine you are developing an e-commerce platform.
### Step 1: Initial Blueprint (First Migration)
- **Prisma Schema:** You define a `Product` model (name, price) and a `User` model (email, password).
- **Action:** You run `prisma migrate dev`.
- **Result:** Prisma generates "Migration 20250101_init" containing SQL to create the `products` and `users` tables. Your database is now built.
### Step 2: Adding a Feature (Second Migration)
- **Prisma Schema Change:** You realize users need a shipping address, so you add an `Address` model and link it to the `User` model.
- **Action:** You run `prisma migrate dev` again.
- **Result:** Prisma compares the new schema to the old one. It generates "Migration 20250315_add_address" containing SQL to **CREATE TABLE `addresses`** and **ADD COLUMN `addressId`** to the `users` table.
### Step 3: Changing a Detail (Third Migration)
- **Prisma Schema Change:** You decide the product price should only be an integer (in cents), not a float. You change the type of the `price` field.
- **Action:** You run `prisma migrate dev`.
- **Result:** Prisma generates "Migration 20250520_change_price_type" containing SQL to **ALTER COLUMN `price`** on the `products` table.
    
The `prisma/migrations` folder now contains three separate, irreversible history files, ensuring that no matter where you deploy, the database will be built and renovated in that exact, safe sequence.
## [Prisma client]
The **Prisma Client** is an auto-generated, type-safe query builder that makes it easy to interact with your database from your application code (written in languages like TypeScript or JavaScript).

| **Component**                         | **Analogy Role**          | **Explanation**                                                                                                                                                                                                                                                                                               |
| ------------------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Your Code** (e.g., Node.js/Express) | **The Customer**          | You tell the Prisma Client _what_ data you want (e.g., "I need the user with ID 5 and their last 3 posts").                                                                                                                                                                                                   |
| **Prisma Client**                     | **The Waiter/Translator** | It takes your simple, easy-to-read request (e.g., `await prisma.user.findUnique(...)`), converts it into optimized **SQL** (or other database-specific language), sends it to the database, receives the raw data, and translates it back into neat, structured, and type-safe JavaScript/TypeScript objects. |
| **Your Database** (e.g., PostgreSQL)  | **The Kitchen**           | This is where the actual data storage and processing happens.                                                                                                                                                                                                                                                 |

#### Key Features of the Prisma Client
##### [ Auto-Generated and Type-Safe 🛡️]
The Prisma Client is generated automatically based on your **Prisma Schema** (`schema.prisma`).
- **Type Safety (The biggest advantage):** If you define a `User` model with an `email` field as a `String`, the Prisma Client will know this. If you try to access `user.emaiil` (with a typo) or try to save a number to the `email` field, the TypeScript compiler will immediately show an error **before you even run your code**. This prevents countless runtime bugs.
    
- **IntelliSense:** When you type `prisma.user.`, your editor will suggest all available methods like `findMany`, `create`, `update`, etc.
##### [Powerful Query Capabilities]
Prisma Client provides an intuitive API for common database operations:
- **CRUD Operations:** Simple methods for **C**reate, **R**ead, **U**pdate, and **D**elete data.
    ```js
     // Create a new user 
    const newUser = await prisma.user.create({ data: { email: '...' } });
    ```
    
- **Relational Queries (Includes):** Easily fetch related data in a single, efficient query.
    ```js
    // Find a user AND all their related posts in one go
    const userWithPosts = await prisma.user.findUnique({
      where: { id: 5 },
      include: { posts: true },
    });
    ```
    
- **Filtering, Sorting, and Pagination:** Complex logic is written clearly in code.
    ```js
        // Find published posts, sorted by title, skipping the first 10
      const posts = await prisma.post.findMany({
      where: { published: true },
      orderBy: { title: 'asc' },
      skip: 10,
      take: 5, // Pagination
    });
    ```
### 3. Transactions
It supports running multiple queries as a single, atomic unit. If any part of the unit fails, the entire change is rolled back, guaranteeing data integrity.

```js
await prisma.$transaction([
  // Query 1: Transfer money out
  prisma.account.update({ /* ... */ }),
  // Query 2: Transfer money in
  prisma.account.update({ /* ... */ }),
]);
```


## How it works 
- You define your database schema in `schema.prisma`.
- You run `npx prisma generate`. This command reads your schema and creates the `node_modules/@prisma/client` package, which is the actual Client library.
- In your code, you initialize the client: `const prisma = new PrismaClient()`.
- When you call a method like `prisma.user.findMany()`, the Client constructs an optimized SQL query string.
- It sends that SQL query to the database connector.
- The database executes the SQL and returns the raw result set.
- The Prisma Client maps the raw result set into the correctly structured, type-safe JavaScript/TypeScript objects that you use in your application.

### client generate 
![[Pasted image 20251023154805.png]]
### commands to sync the changes to your database schema according to the Prisma schema file.. 
- These two have Different Purpose and UseCase
![[Pasted image 20251023155127.png]]
```cli
npx prisma migrate dev 
npx prisma db push
```

## Prisma client queries 
https://www.prisma.io/docs/orm/prisma-client/queries

[step1]
***[Initialize Prisma]***
Initialize Prisma in your project, which creates a `prisma` folder and a `schema.prisma` file. This command also sets up a `.env` file (or uses `.env.local` in Next.js) for your database connection string.
```
npx prisma init
```
[step2]
Setting up Prisma with Next.js involves several key steps to integrate the ORM with your application's data layer. install prisma.
Begin by installing the Prisma CLI as a development dependency and the Prisma Client:

```
npm install prisma --save-dev  
npm install @prisma/client
```

[step3]
Configure schema.prisma.
Open the `prisma/schema.prisma` file.

- **Datasource:** Define your database provider (e.g., `postgresql`, `mysql`, `sqlite`, `mongodb`) and connect it to your database URL, typically sourced from an environment variable.

Code

```
        datasource db {          provider = "postgresql" // or "mysql", "sqlite", "mongodb"          url      = env("DATABASE_URL")        }
```

- **Generator:** Ensure the Prisma Client generator is configured.

Code

```
        generator client {          provider = "prisma-client-js"        }
```

- **Define Models:** Create your database models within this file, representing your tables or collections.

Code

```
        model User { id  Int   @id @default(autoincrement())          email String  @unique          name  String?        }
```

Database Connection String.

Ensure your `.env` (or `.env.local`) file contains the `DATABASE_URL` environment variable with the correct connection string for your database. Generate Prisma Client.

After defining your schema, generate the Prisma Client to get type-safe database access in your Next.js application.

Code

```
    npx prisma generate
```
It is recommended to include `prisma generate` in your `package.json` build script to ensure the client is always up-to-date, especially in deployment environments like Vercel.

Code

```
    "scripts": {      "build": "prisma generate && next build"    }
```

Create a Prisma Client Instance.

Create a utility file (e.g., `lib/prisma.ts`) to export a single, global instance of `PrismaClient` to avoid creating multiple instances in development, which can lead to performance issues.

TypeScript

```
    // lib/prisma.ts    import { PrismaClient } from '@prisma/client';    const globalForPrisma = global as unknown as { prisma: PrismaClient };    export const prisma =      globalForPrisma.prisma ||      new PrismaClient({        log: ['query'], // Optional: logs database queries      });    if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma;
```

Use Prisma in Next.js.

You can now import and use the `prisma` instance in your Next.js API routes, Server Components, Server Actions, or Route Handlers to interact with your database.

TypeScript

```
    // app/api/users/route.ts (Example API Route)    import { prisma } from '@/lib/prisma'; // Adjust path as needed    import { NextResponse } from 'next/server';    export async function GET() {      const users = await prisma.user.findMany();      return NextResponse.json(users);    }
```

# Prisma generators 
https://www.prisma.io/docs/orm/prisma-schema/overview/generators
