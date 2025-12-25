# operators 
## or operators
`OR` in Prisma is used inside a `where` clause to match records that satisfy **at least one** of the given conditions.
It works exactly like logical OR in queries.

Syntax:
```
where: {   
          OR: [   
                { condition1 }, 
                { condition2 }   
               ] 
        }
```
#example 
```ts 
const users = await prisma.user.findMany({
  where: {
    OR: [
      { email: "abc@example.com" },
      { username: "abc123" }
    ]
  }
});
```

# select and include clause

Both `select` and `include` are used in Prisma queries to control which fields and relations are returned from the database, but they serve different purposes.

---
## Key Differences

|**Feature**|**select**|**include**|
|---|---|---|
|**Purpose**|**Choose specific fields** from the _main model_ being queried.|**Include related models** (relations) in the result set.|
|**Main Model Fields**|If used, you **must explicitly list** the fields you want, otherwise they are omitted.|The fields of the main model are returned **by default** (unless you use `select` inside the `include`).|
|**Output**|Returns an object/array containing **only the selected fields/relations**.|Returns an object/array containing **all fields of the main model** plus the specified relations.|
|**Granularity**|Used to make the primary result set smaller and faster.|Used to fetch a graph of connected data in a single query.|

---
## 1. `select`: Choosing Fields on the Main Model

`select` is used when you only want a **subset of fields** from the model you are currently querying. This is useful for performance when you don't need all the data.
### Example: Fetching only the ID and Name of Users
JavaScript

```ts
const userList = await prisma.user.findMany({
  select: {
    id: true,
    name: true,
  },
});
```

- **Result:** An array of objects containing _only_ the `id` and `name` properties. All other user fields (like `email`, `password`, etc.) are _omitted_.
    

---
## 2. `include`: Adding Related Models
`include` is used to fetch the **relational data** (i.e., data from other tables) along with the main model data. The main model's fields are included automatically.
### Example: Fetching a User and their Posts
JavaScript
```ts
const userWithPosts = await prisma.user.findUnique({
  where: {
    id: 1,
  },
  include: {
    posts: true, // Includes all fields and all posts related to this user
  },
});
```

- **Result:** A `user` object that includes all its standard fields, plus an array field named `posts` containing all the user's related posts.

---
## 💡 Combining `select` and `include`
You can use `select` inside an `include` to control which fields of the **related model** are returned, or you can use `select` on the main query to limit its fields while still including a relation.
### Example: Fetching only the User Name and the Titles of their Posts
JavaScript
```ts
const limitedData = await prisma.user.findUnique({
  where: {
    id: 1,
  },
  select: { // Selecting fields on the main model (User)
    name: true,
    posts: { // Including the 'posts' relation
      select: { // Selecting fields on the related model (Post)
        title: true,
      },
    },
  },
});
```
- **Result:** A user object containing **only** the `name` field, and the `posts` array will contain objects with **only** the `title` field.