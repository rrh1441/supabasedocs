# Disabling Prepared statements

Last edited: 2/4/2025

* * *

### It is important to note that although the direct connections and Supavisor in session mode support prepared statements, Supavisor in transaction mode does not. [\#](https://supabase.com/docs/guides/troubleshooting/disabling-prepared-statements-qL8lEL\#it-is-important-to-note-that-although-the-direct-connections-and-supavisor-in-session-mode-support-prepared-statements-supavisor-in-transaction-mode-does-not)

## How to disable prepared statements for Supavisor in transaction mode [\#](https://supabase.com/docs/guides/troubleshooting/disabling-prepared-statements-qL8lEL\#how-to-disable-prepared-statements-for-supavisor-in-transaction-mode)

Each ORM or library configures prepared statements differently. Here are settings for some common ones. If you don't see yours, make a comment

# Prisma:

add ?pgbouncer=true to end of connection string:

`
postgres://[db-user].[project-ref]:[db-password]@aws-0-[aws-region].pooler.supabase.com:6543/[db-name]?pgbouncer=true
`

# Drizzle:

Add a prepared false flag to the client:

`
export const client = postgres(connectionString, { prepare: false })
`

# Node Postgres

[Just omit the "name" value in a query definition](https://node-postgres.com/features/queries#prepared-statements):

`
const query = {
name: 'fetch-user', // <--------- DO NOT INCLUDE
text: 'SELECT * FROM user WHERE id = $1',
values: [1],
}
`

# Psycopg

set the [prepare\_threshold](https://www.psycopg.org/psycopg3/docs/api/connections.html#psycopg.Connection.prepare_threshold) to `None`.

# asyncpg

Follow the recommendation in the [asyncpg docs](https://magicstack.github.io/asyncpg/current/faq.html#why-am-i-getting-prepared-statement-errors)

> disable automatic use of prepared statements by passing `statement_cache_size=0` to [asyncpg.connect()](https://magicstack.github.io/asyncpg/current/api/index.html#asyncpg.connection.connect) and [asyncpg.create\_pool()](https://magicstack.github.io/asyncpg/current/api/index.html#asyncpg.pool.create_pool) (and, obviously, avoid the use of [Connection.prepare()](https://magicstack.github.io/asyncpg/current/api/index.html#asyncpg.connection.Connection.prepare));

# Rust's Deadpool or `tokio-postgres`:

- Check [GitHub Discussion](https://github.com/bikeshedder/deadpool/issues/340#event-13642472475)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings