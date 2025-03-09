Database

# Drop all tables in a PostgreSQL schema

* * *

Execute the following query to drop all tables in a given schema.
Replace `my-schema-name` with the name of your schema. In Supabase, the default schema is `public`.

This deletes all tables and their associated data. Ensure you have a recent [backup](https://supabase.com/docs/guides/platform/backups) before proceeding.

`
do $$ declare
    r record;
begin
    for r in (select tablename from pg_tables where schemaname = 'my-schema-name') loop
        execute 'drop table if exists ' || quote_ident(r.tablename) || ' cascade';
    end loop;
end $$;
`

This query works by listing out all the tables in the given schema and then executing a `drop table` for each (hence the `for... loop`).

You can run this query using the [SQL Editor](https://supabase.com/dashboard/project/_/sql) in the Supabase Dashboard, or via `psql` if you're [connecting directly to the database](https://supabase.com/docs/guides/database/connecting-to-postgres#direct-connections).

### Is this helpful?

NoYes

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings