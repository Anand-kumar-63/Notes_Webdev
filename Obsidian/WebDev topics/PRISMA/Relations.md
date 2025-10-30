Relations in a Prisma schema define how **models** (representing tables or collections in your database) are connected to each other. They specify the cardinality (one-to-one, one-to-many, many-to-many) and the fields that link the models.

Here's a breakdown of how relations work and how they're defined in Prisma:

## Defining Relations

A relation in Prisma is typically defined using two parts across the connecting models:
1. **The Relation Field:** A field on a model whose type is another model. This field represents the link.
2. **The `@relation` Attribute:** This attribute is used on the **relation field** of one of the models to configure the relation.

The general syntax for the `@relation` attribute is:
Code snippet
```
@relation([fields: [<local_field>], references: [<remote_field>], name: <optional_name>])
```

| Component    | Description                                                                                                              |
| ------------ | ------------------------------------------------------------------------------------------------------------------------ |
| `fields`     | A list of the **local fields** on the _current_ model that hold the foreign key(s).                                      |
| `references` | A list of the **remote fields** (usually the `@id` or `@unique` field) on the _other_ model that the `fields` reference. |
| `name`       | An optional name for the relation, primarily used to disambiguate multiple relations between the same two models.        |

---

## Types of Relations
Prisma supports the standard relational types:
### 1. One-to-One Relations (1-1)
In a 1-1 relation, one record in Model A is linked to at most one record in Model B, and vice-versa.
**Key characteristic:** The **foreign key** field (the one with the `@relation` attribute) must be decorated with the `@unique` attribute.
**Example:** A `User` can have one `Profile`, and a `Profile` belongs to one `User`.
Code snippet

```
model User {
  id      Int      @id @default(autoincrement())
  email   String   @unique
  profile Profile? // Relation field: optional on User
}

model Profile {
  id     Int    @id @default(autoincrement())
  bio    String
  userId Int    @unique // Foreign key: must be unique
  user   User   @relation(fields: [userId], references: [id]) // Relation side
}
```

- **`Profile`** holds the **foreign key** (`userId`) and the **`@relation`** attribute, making it the **relation owner**.
---
### 2. One-to-Many Relations (1-N)
This is the most common type. One record in Model A can be linked to multiple records in Model B, but a record in Model B is linked to only one record in Model A.
**Key characteristic:** The **foreign key** is on the "many" side of the relation.
**Example:** An `Author` can write many `Post`s, but a `Post` has only one `Author`.
Code snippet
```
model Author {
  id    Int     @id @default(autoincrement())
  name  String
  posts Post[] // Relation field: List of Posts
}

model Post {
  id         Int      @id @default(autoincrement())
  title      String
  authorId   Int      // Foreign key
  author     Author   @relation(fields: [authorId], references: [id]) // Relation side
}
```

- **`Post`** holds the **foreign key** (`authorId`) and the **`@relation`** attribute.
- The relation field on the "many" side (`posts` on `Author`) is defined as an **array** (`Post[]`).

---

### 3. Many-to-Many Relations (M-N)

In an M-N relation, one record in Model A can be linked to multiple records in Model B, and one record in Model B can be linked to multiple records in Model A.
**Prisma's approach:** Prisma will automatically create an **intermediate table** (often called a **join table** or **connector table**) in the database to handle the relationship.
**Example:** A `Student` can enroll in many `Course`s, and a `Course` can have many `Student`s.
Code snippet

```
model Student {
  id      Int      @id @default(autoincrement())
  name    String
  courses Course[] // Relation field: List of Courses
}

model Course {
  id       Int       @id @default(autoincrement())
  title    String
  students Student[] // Relation field: List of Students
}
```

- **No `@relation` attribute** or **foreign key fields** are manually added to the models.
- Prisma handles the creation and management of the join table behind the scenes.
- To access the join table directly (e.g., to add extra fields like `enrollmentDate`), you must explicitly define it as a **three-model relation** (see "Explicit Many-to-Many" below).

---

## Relation Configuration Options
The `@relation` attribute also allows configuration for database behavior:
- **`onUpdate`** and **`onDelete`**: Define the **referential actions** that should happen when the referenced record is updated or deleted. Common values are:
    - `Cascade` (Default): Update/delete the related records as well.
    - `NoAction`: Do nothing.
    - `Restrict`: Prevent the update/delete if related records exist.
    - `SetNull`: Set the foreign key to `null` (requires the foreign key field to be optional: `Int`).
    - `SetDefault`: Set the foreign key to its default value (requires the foreign key to have a `@default` value).

**Example with Referential Actions:**
Code snippet
```
model Post {
  // ...
  authorId Int
  author   Author @relation(fields: [authorId], references: [id], onDelete: Cascade, onUpdate: Cascade)
}
```
## Self-Relations
A model can also have a relation to itself, useful for hierarchical data like organizational charts or comments/replies.
**Example:** A `Category` can have a parent `Category` and multiple child `Category`s.
Code snippet
```
model Category {
  id       Int        @id @default(autoincrement())
  name     String
  parentId Int?       // Optional foreign key to self
  parent   Category?  @relation("Children", fields: [parentId], references: [id])
  children Category[] @relation("Children")
}
```
- The **`name`** argument (`"Children"`) is required here to disambiguate the two relation fields connecting the same two models (to itself).