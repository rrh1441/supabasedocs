Database

# HypoPG: Hypothetical indexes

* * *

`HypoPG` is Postgres extension for creating hypothetical/virtual indexes. HypoPG allows users to rapidly create hypothetical/virtual indexes that have no resource cost (CPU, disk, memory) that are visible to the Postgres query planner.

The motivation for HypoPG is to allow users to quickly search for an index to improve a slow query without consuming server resources or waiting for them to build.

## Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/hypopg\#enable-the-extension)

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Extensions** in the sidebar.
3. Search for `hypopg` and enable the extension.

### Speeding up a query [\#](https://supabase.com/docs/guides/database/extensions/hypopg\#speeding-up-a-query)

Given the following table and a simple query to select from the table by `id`:

`
create table account (
id int,
address text
);
insert into account(id, address)
select
id,
id || ' main street'
from
generate_series(1, 10000) id;
`

We can generate an explain plan for a description of how the Postgres query planner
intends to execute the query.

`
explain select * from account where id=1;
                      QUERY PLAN
-------------------------------------------------------
Seq Scan on account  (cost=0.00..180.00 rows=1 width=13)
Filter: (id = 1)
(2 rows)
`

Using HypoPG, we can create a hypothetical index on the `account(id)` column to check if it would be useful to the query planner and then re-run the explain plan.

Note that the virtual indexes created by HypoPG are only visible in the Postgres connection that they were created in. Supabase connects to Postgres through a connection pooler so the `hypopg_create_index` statement and the `explain` statement should be executed in a single query.

`
select * from hypopg_create_index('create index on account(id)');
explain select * from account where id=1;
                                     QUERY PLAN
------------------------------------------------------------------------------------
Index Scan using <13504>btree_account_id on hypo  (cost=0.29..8.30 rows=1 width=13)
Index Cond: (id = 1)
(2 rows)
`

The query plan has changed from a `Seq Scan` to an `Index Scan` using the newly created virtual index, so we may choose to create a real version of the index to improve performance on the target query:

`
create index on account(id);
`

## Functions [\#](https://supabase.com/docs/guides/database/extensions/hypopg\#functions)

- [`hypo_create_index(text)`](https://hypopg.readthedocs.io/en/rel1_stable/usage.html#create-a-hypothetical-index): A function to create a hypothetical index.
- [`hypopg_list_indexes`](https://hypopg.readthedocs.io/en/rel1_stable/usage.html#manipulate-hypothetical-indexes): A View that lists all hypothetical indexes that have been created.
- [`hypopg()`](https://hypopg.readthedocs.io/en/rel1_stable/usage.html#manipulate-hypothetical-indexes): A function that lists all hypothetical indexes that have been created with the same format as `pg_index`.
- [`hypopg_get_index_def(oid)`](https://hypopg.readthedocs.io/en/rel1_stable/usage.html#manipulate-hypothetical-indexes): A function to display the `create index` statement that would create the index.
- [`hypopg_get_relation_size(oid)`](https://hypopg.readthedocs.io/en/rel1_stable/usage.html#manipulate-hypothetical-indexes): A function to estimate how large a hypothetical index would be.
- [`hypopg_drop_index(oid)`](https://hypopg.readthedocs.io/en/rel1_stable/usage.html#manipulate-hypothetical-indexes): A function to remove a given hypothetical index by `oid`.
- [`hypopg_reset()`](https://hypopg.readthedocs.io/en/rel1_stable/usage.html#manipulate-hypothetical-indexes): A function to remove all hypothetical indexes.

## Resources [\#](https://supabase.com/docs/guides/database/extensions/hypopg\#resources)

- Official [HypoPG documentation](https://hypopg.readthedocs.io/en/rel1_stable/)

### Is this helpful?

NoYes

### On this page

[Enable the extension](https://supabase.com/docs/guides/database/extensions/hypopg#enable-the-extension) [Speeding up a query](https://supabase.com/docs/guides/database/extensions/hypopg#speeding-up-a-query) [Functions](https://supabase.com/docs/guides/database/extensions/hypopg#functions) [Resources](https://supabase.com/docs/guides/database/extensions/hypopg#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings