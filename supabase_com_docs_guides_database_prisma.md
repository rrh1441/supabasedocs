Database

# Prisma

* * *

This quickly shows how to connect your Prisma application to Supabase Postgres. If you encounter any problems, reference the [Prisma troubleshooting docs](https://supabase.com/docs/guides/database/prisma/prisma-troubleshooting).

If you plan to solely use Prisma instead of the Supabase Data API (PostgREST), turn it off in the [API Settings](https://supabase.com/dashboard/project/_/settings/api).

1

### Create a custom user for Prisma

- In the [SQL Editor](https://supabase.com/dashboard/project/_/sql/new), create a Prisma DB user with full privileges on the public schema.
- This gives you better control over Prisma's access and makes it easier to monitor using Supabase tools like the [Query Performance Dashboard](https://supabase.com/dashboard/project/_/advisors/query-performance) and [Log Explorer](https://supabase.com/dashboard/project/_/logs/explorer).

##### password manager

For security, consider using a [password generator](https://bitwarden.com/password-generator/) for the Prisma role.

`
-- Create custom user
create user "prisma" with password 'custom_password' bypassrls createdb;
-- extend prisma's privileges to postgres (necessary to view changes in Dashboard)
grant "prisma" to "postgres";
-- Grant it necessary permissions over the relevant schemas (public)
grant usage on schema public to prisma;
grant create on schema public to prisma;
grant all on all tables in schema public to prisma;
grant all on all routines in schema public to prisma;
grant all on all sequences in schema public to prisma;
alter default privileges for role postgres in schema public grant all on tables to prisma;
alter default privileges for role postgres in schema public grant all on routines to prisma;
alter default privileges for role postgres in schema public grant all on sequences to prisma;
`

`
-- alter prisma password if needed
alter user "prisma" with password 'new_password';
`

2

### Create a Prisma Project

Create a new Prisma Project on your computer

Create a new directory

Terminal

`
mkdir hello-prisma
cd hello-prisma
`

Initiate a new Prisma project

npmpnpmyarnbun

`
npm init -y
npm install prisma typescript ts-node @types/node --save-dev
npx tsc --init
npx prisma init
`

3

### Add your connection information to your .env file

- Visit the [Database Settings](https://supabase.com/dashboard/project/_/settings/database)
- Find your Supavisor Session Mode string. It should end with 5432. It will be used in your `.env` file.

If you're in an [IPv6 environment](https://github.com/orgs/supabase/discussions/27034) or have the IPv4 Add-On, you can use the direct connection string instead of Supavisor in Session mode.

- If you plan on deploying Prisma to a serverless or auto-scaling environment, you'll also need your Supavisor transaction mode string.
- The string is identical to the session mode string but uses port 6543 at the end.

server-based deploymentsserverless deployments

In your .env file, set the DATABASE\_URL variable to your connection string

.env

`
# Used for Prisma Migrations and within your application
DATABASE_URL="postgres://[DB-USER].[PROJECT-REF]:[PRISMA-PASSWORD]@[DB-REGION].pooler.supabase.com:5432/postgres"
`

Change your string's `[DB-USER]` to `prisma` and add the password you created in step 1

`
postgres://prisma.[PROJECT-REF]...
`

4

### Create your migrations

If you have already modified your Supabase database, synchronize it with your migration file. Otherwise create new tables for your database

New ProjectsPopulated Projects

Create new tables in your prisma.schema file

prisma/schema.prisma

`
model Post {
id        Int     @id @default(autoincrement())
title     String
content   String?
published Boolean @default(false)
author    User?   @relation(fields: [authorId], references: [id])
authorId  Int?
}
model User {
id    Int     @id @default(autoincrement())
email String  @unique
name  String?
posts Post[]
}
`

commit your migration

npmpnpmyarnbun

`
npx prisma migrate dev --name first_prisma_migration
`

5

### Install the prisma client

Install the Prisma client and generate its model

npmpnpmyarnbun

`
npm install @prisma/client
npx prisma generate
`

6

### Test your API

Create a index.ts file and run it to test your connection

index.ts

`
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();
async function main() {
//change to reference a table in your schema
const val = await prisma.<SOME_TABLE_NAME>.findMany({
    take: 10,
});
console.log(val);
}
main()
.then(async () => {
    await prisma.$disconnect();
})
.catch(async (e) => {
    console.error(e);
    await prisma.$disconnect();
process.exit(1);
});
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings