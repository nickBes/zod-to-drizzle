# zod-to-drizzle
Create drizzle sql tables from zod validation schemas

# The Problem
Using `drizzle-orm` for generating Zod validation schemas on the client side introduces significant challenges. The inclusion of `drizzle-zod` dependencies results in increased bundle size and potential leaks of server-side code, which are critical issues for client-side applications. Although creating separate Zod validation schemas for the client can mitigate this, it leads to duplicated code and potential inconsistencies, making maintenance cumbersome. These problems can deter developers from using `drizzle-orm`, especially in applications with numerous forms requiring extensive client-side validation. Instead, they might opt for alternatives like `prisma`, which supports Zod schema generation.

Here's a link to a [disscussion about this issue](https://www.answeroverflow.com/m/1130657305722101782).

# Use Example
```ts
// src/schemas/user.ts

import { serial, text, table } from "zod-to-drizzle/pg";

// define schema and determine pg types
export const userSchema = table("users", {
    id: serial("id") // --> z.number()
    fullName: text("full_name") // --> z.string()
});

// src/models/user.ts

import { generateTable } from "zod-to-drizzle-gen/pg";
import { userSchema } from "@/schemas/user";

// generates table and overrides properties
const users = generateTable(userSchema, ({ id }) => ({
    id.primaryKey(),
}));
```