Database

# Drizzle

* * *

### Connecting with Drizzle [\#](https://supabase.com/docs/guides/database/drizzle\#connecting-with-drizzle)

[Drizzle ORM](https://github.com/drizzle-team/drizzle-orm) is a TypeScript ORM for SQL databases designed with maximum type safety in mind. You can use their ORM to connect to your database.

If you plan on solely using Drizzle instead of the Supabase Data API (PostgREST), you can turn off the latter in the [API Settings](https://supabase.com/dashboard/project/_/settings/api).

1

### Install

Install Drizzle and related dependencies.

`
npm i drizzle-orm postgres
npm i -D drizzle-kit
`

2

### Create your models

Create a `schema.ts` file and define your models.

schema.ts

`
import { pgTable, serial, text, varchar } from "drizzle-orm/pg-core";
export const users = pgTable('users', {
id: serial('id').primaryKey(),
fullName: text('full_name'),
phone: varchar('phone', { length: 256 }),
});
`

3

### Connect

Connect to your database using the Connection Pooler.

In your [`Database Settings`](https://supabase.com/dashboard/project/_/settings/database), make sure `Use connection pooler` is checked, then copy the URI and save it as the `DATABASE_URL` environment variable. Remember to replace the password placeholder with your actual database password.

db.ts

`
import 'dotenv/config'
import { drizzle } from 'drizzle-orm/postgres-js'
import postgres from 'postgres'
const connectionString = process.env.DATABASE_URL
// Disable prefetch as it is not supported for "Transaction" pool mode
export const client = postgres(connectionString, { prepare: false })
export const db = drizzle(client);
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings