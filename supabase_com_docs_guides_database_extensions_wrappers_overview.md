Database

# Foreign Data Wrappers

## Connecting to external systems using Postgres Foreign Data Wrappers.

* * *

Foreign Data Wrappers (FDW) are a core feature of Postgres that allow you to access and query data stored in external data sources as if they were native Postgres tables.

Postgres includes several built-in foreign data wrappers, such as [`postgres_fdw`](https://www.postgresql.org/docs/current/postgres-fdw.html) for accessing other Postgres databases, and [`file_fdw`](https://www.postgresql.org/docs/current/file-fdw.html) for reading data from files. Supabase extends this feature to query other databases or any other external systems. We do this with our open source [Wrappers](https://github.com/supabase/wrappers) framework. In these guides we'll refer to them as "Wrappers", Foreign Data Wrappers, or FDWs. They are conceptually the same thing.

## Concepts [\#](https://supabase.com/docs/guides/database/extensions/wrappers/overview\#concepts)

Wrappers introduce some new terminology and different workflows.

![Foreign Data Wrappers (FDW)](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fdatabase%2Fforeign-data-wrappers%2Fextracting-data--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Remote servers [\#](https://supabase.com/docs/guides/database/extensions/wrappers/overview\#remote-servers)

A Remote Server is an external database, API, or any system containing data that you want to query from your Postgres database. Examples include:

- An external database, like Postgres or Firebase.
- A remote data warehouse, like ClickHouse, BigQuery, or Snowflake.
- An API, like Stripe or GitHub.

It's possible to connect to multiple remote servers of the same type. For example, you can connect to two different Firebase projects within the same Supabase database.

### Foreign tables [\#](https://supabase.com/docs/guides/database/extensions/wrappers/overview\#foreign-tables)

A table in your database which maps to some data inside a Remote Server.

Examples:

- An `analytics` table which maps to a table inside your data warehouse.
- A `subscriptions` table which maps to your Stripe subscriptions.
- A `collections` table which maps to a Firebase collection.

Although a foreign table behaves like any other table, the data is not stored inside your database. The data remains inside the Remote Server.

### ETL with Wrappers [\#](https://supabase.com/docs/guides/database/extensions/wrappers/overview\#etl-with-wrappers)

ETL stands for Extract, Transform, Load. It's an established process for moving data from one system to another. For example, it's common to move data from a production database to a data warehouse.

There are many popular ETL tools, such as [Fivetran](https://fivetran.com/) and [Airbyte](https://airbyte.io/).

Wrappers provide an alternative to these tools. You can use SQL to move data from one table to another:

`
-- Copy data from your production database to your
-- data warehouse for the last 24 hours:
insert into warehouse.analytics
select * from public.analytics
where ts > (now() - interval '1 DAY');
`

This approach provides several benefits:

1. **Simplicity:** the Wrappers API is just SQL, so data engineers don't need to learn new tools and languages.
2. **Save on time:** avoid setting up additional data pipelines.
3. **Save on Data Engineering costs:** less infrastructure to be managed.

One disadvantage is that Wrappers are not as feature-rich as ETL tools. They also couple the ETL process to your database.

### On-demand ETL with Wrappers [\#](https://supabase.com/docs/guides/database/extensions/wrappers/overview\#on-demand-etl-with-wrappers)

Supabase extends the ETL concept with real-time data access. Instead of moving gigabytes of data from one system to another before you can query it, you can instead query the data directly from the remote server. This additional option, "Query", extends the ETL process and is called [QETL](https://www.sciencedirect.com/science/article/abs/pii/S0169023X1730438X) (pronounced "kettle"): Query, Extract, Transform, Load.

`
-- Get all purchases for a user from your data warehouse:
select
auth.users.id as user_id,
warehouse.orders.id as order_id
from
warehouse.orders
join
auth.users on auth.users.id = warehouse.orders.user_id
where
auth.users.id = '<some_user_id>';
`

This approach has several benefits:

1. **On-demand:** analytical data is immediately available within your application with no additional infrastructure.
2. **Always in sync:** since the data is queried directly from the remote server, it's always up-to-date.
3. **Integrated:** large datasets are available within your application, and can be joined with your operational/transactional data.
4. **Save on bandwidth:** only extract/load what you need.

### Batch ETL with Wrappers [\#](https://supabase.com/docs/guides/database/extensions/wrappers/overview\#batch-etl-with-wrappers)

A common use case for Wrappers is to extract data from a production database and load it into a data warehouse. This can be done within your database using [pg\_cron](https://supabase.com/docs/guides/database/extensions/pg_cron). For example, you can schedule a job to run every night to extract data from your production database and load it into your data warehouse.

`
-- Every day at 3am, copy data from your
-- production database to your data warehouse:
select cron.schedule(
'nightly-etl',
'0 3 * * *',
$$
    insert into warehouse.analytics
    select * from public.analytics
    where ts > (now() - interval '1 DAY');
$$
);
`

![FDW with pg_cron](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fdatabase%2Fforeign-data-wrappers%2Fextracting-data-pgcron--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

This process can be taxing on your database if you are moving large amounts of data. Often, it's better to use an external tool for batch ETL, such as [Fivetran](https://fivetran.com/) or [Airbyte](https://airbyte.io/).

### WebAssembly Wrappers [\#](https://supabase.com/docs/guides/database/extensions/wrappers/overview\#webassembly-wrappers)

WebAssembly (Wasm) is a binary instruction format that enables high-performance execution of code on the web. Wrappers now includes a Wasm runtime, which provides a sandboxed execution environment, to run Wasm foreign data wrappers. Combined Wrappers with Wasm, developing and distributing new FDW becomes much easier and you can even build your own Wasm FDW and use it on Supabase platform.

To learn more about Wasm FDW, visit [Wrappers official documentation](https://supabase.github.io/wrappers/).

## Security [\#](https://supabase.com/docs/guides/database/extensions/wrappers/overview\#security)

Foreign Data Wrappers do not provide Row Level Security, thus it is not advised to expose them via your API. Wrappers should _always_ be stored in a private schema. For example, if you are connecting to your Stripe account, you should create a `stripe` schema to store all of your foreign tables inside. This schema should _not_ be added to the “Additional Schemas” setting in the API section.

If you want to expose any of the foreign table columns to your public API, you can create a [Database Function with security definer](https://supabase.com/docs/guides/database/functions#security-definer-vs-invoker) in the `public` schema, and then you can interact with your foreign table through API. For better access control, the function should have appropriate filters on the foreign table to apply security rules based on your business needs.

As an example, go to [SQL Editor](https://supabase.com/dashboard/project/_/sql/new) and then follow below steps,

1. Create a Stripe Products foreign table:



`
create foreign table stripe.stripe_products (
id text,
name text,
active bool,
default_price text,
description text,
created timestamp,
updated timestamp,
attrs jsonb
)
server stripe_fdw_server
options (
       object 'products',
       rowid_column 'id'
);
`

2. Create a security definer function that queries the foreign table and filters on the name prefix parameter:



`
create function public.get_stripe_products(name_prefix text)
returns table (
id text,
name text,
active boolean,
default_price text,
description text
)
language plpgsql
security definer set search_path = ''
as $$
begin
return query
select
       t.id,
       t.name,
       t.active,
       t.default_price,
       t.description
from
       stripe.stripe_products t
where
       t.name like name_prefix || '%'
;
end;
$$;
`

3. Restrict the function execution to a specific role only, for example, the authenticated users:





By default, the function created can be executed by any roles like `anon`, that means the
foreign table is public accessible. Always limit the function execution permission to
appropriate roles.







`
   -- revoke public execute permission
revoke execute on function public.get_stripe_products from public;
revoke execute on function public.get_stripe_products from anon;
   -- grant execute permission to a specific role only
grant execute on function public.get_stripe_products to authenticated;
`


Once the preceding steps are finished, the function can be invoked from Supabase client to query the foreign table:

`
const { data, error } = await supabase
.rpc('get_stripe_products', { name_prefix: 'Test' })
.select('*')
if (error) console.error(error)
else console.log(data)
`

## Resources [\#](https://supabase.com/docs/guides/database/extensions/wrappers/overview\#resources)

- Official [`supabase/wrappers` documentation](https://supabase.github.io/wrappers/)

### Is this helpful?

NoYes

### On this page

[Concepts](https://supabase.com/docs/guides/database/extensions/wrappers/overview#concepts) [Remote servers](https://supabase.com/docs/guides/database/extensions/wrappers/overview#remote-servers) [Foreign tables](https://supabase.com/docs/guides/database/extensions/wrappers/overview#foreign-tables) [ETL with Wrappers](https://supabase.com/docs/guides/database/extensions/wrappers/overview#etl-with-wrappers) [On-demand ETL with Wrappers](https://supabase.com/docs/guides/database/extensions/wrappers/overview#on-demand-etl-with-wrappers) [Batch ETL with Wrappers](https://supabase.com/docs/guides/database/extensions/wrappers/overview#batch-etl-with-wrappers) [WebAssembly Wrappers](https://supabase.com/docs/guides/database/extensions/wrappers/overview#webassembly-wrappers) [Security](https://supabase.com/docs/guides/database/extensions/wrappers/overview#security) [Resources](https://supabase.com/docs/guides/database/extensions/wrappers/overview#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings


![Foreign Data Wrappers (FDW)](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fdatabase%2Fforeign-data-wrappers%2Fextracting-data--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![FDW with pg_cron](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fdatabase%2Fforeign-data-wrappers%2Fextracting-data-pgcron--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)