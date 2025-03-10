---
description: Use Drizzle with PostgreSQL to define and modify schemas in a flexible SQL-like manner
globs: src/**/*.{ts,tsx}, drizzle.config.ts
alwaysApply: false
---

# Drizzle ORM (Postgres) Rules

<author>blefnk/rules</author>
<version>1.0.0</version>

## Context

- Define and modify schemas type-safely with Drizzle ORM.
- Enable lightweight, serverless-ready, SQL-like interactions.
- Export schemas so Drizzle-Kit detects them for migrations.

## Requirements

- Use `pgTable` from `drizzle-orm/pg-core` to define tables.
- Organize schema files as needed and export all models.
- Use column aliases if TS keys differ from DB names.
- Enforce `casing: "snake_case"` and reuse shared definitions.
- Configure `drizzle.config.ts` with `dialect: "postgresql"`, schema paths, credentials, and output.
- Apply changes with `bun db:push` or generate and run migrations.
- Keep migration files version-controlled and succinct.
- Use `leftJoin`, `rightJoin`, `innerJoin`, and `fullJoin` for relational queries.
- Use table aliases for complex or self-joins.
- Use `.select({ ... })` for typed partial selects.
- Use `.select()`, `.insert()`, `.update()`, and `.delete()`.
- Build filters with `eq`, `lt`, `gt`, `and`, `or`, etc.
- Use raw `sql` templates for complex expressions when needed.
- Prefer relational query methods (e.g., `.query.[table].findMany({ with: { ... } })`) to fetch nested data in one call.
- Connect using drivers like `node-postgres` or `postgres.js`.
- Optimize connections and use caching (e.g., `unstable_cache`).
- Reuse queries or use partial selects to reduce DB hits.
- Use advanced features (`pgEnum`, `pgSchema`, sequences) for extra type safety.
- Use Drizzle’s `sql` templates with helpers (`.mapWith()`, `.join()`, `.append()`, `.as()`, `.fromList()`).
- Use `sql.raw()` for unescaped SQL when necessary.
- Insert: `db.insert(table).values({ ... })`
- Select: `db.select().from(table).where(eq(table.column, value))`
- Update: `db.update(table).set({ ... }).where(eq(table.column, value))`
- Delete: `db.delete(table).where(eq(table.column, value))`
- Organize schema files (single or multiple) and export all definitions.
- Use SQL template helpers and table aliasing for dynamic queries.
- Optimize performance with partial selects and caching.

## Examples

<example>
  // Define a basic table schema
  import { integer, varchar, pgTable } from "drizzle-orm/pg-core";

  export const users = pgTable("users", {
    id: integer("id").primaryKey(),
    email: varchar("email", { length: 256 }).notNull().unique(),
  });
</example>

<example>
  // Execute a SQL-like select query
  const userData = await db
    .select()
    .from(users)
    .where(eq(users.email, "john@example.com"));
</example>

<example type="invalid">
  // Do not omit exports; Drizzle-Kit requires table exports for migrations
  const posts = pgTable("posts", { id: integer("id").primaryKey() });
</example>
