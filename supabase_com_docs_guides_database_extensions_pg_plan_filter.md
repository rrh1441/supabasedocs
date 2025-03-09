Database

# pg\_plan\_filter: Restrict Total Cost

* * *

[`pg_plan_filter`](https://github.com/pgexperts/pg_plan_filter) is Postgres extension to block execution of statements where query planner's estimate of the total cost exceeds a threshold. This is intended to give database administrators a way to restrict the contribution an individual query has on database load.

## Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/pg_plan_filter\#enable-the-extension)

`pg_plan_filter` can be enabled on a per connection basis:

`
load 'plan_filter';
`

or for all connections:

`
alter database some_db set session_preload_libraries = 'plan_filter';
`

## API [\#](https://supabase.com/docs/guides/database/extensions/pg_plan_filter\#api)

`plan_filter.statement_cost_limit`: restricts the maximum total cost for executed statements
`plan_filter.limit_select_only`: restricts to `select` statements

Note that `limit_select_only = true` is not the same as read-only because `select` statements may modify data, for example, through a function call.

## Example [\#](https://supabase.com/docs/guides/database/extensions/pg_plan_filter\#example)

To demonstrate total cost filtering, we'll compare how `plan_filter.statement_cost_limit` treats queries that are under and over its cost limit. First, we set up a table with some data:

`
create table book(
id int primary key
);
-- CREATE TABLE
insert into book(id) select * from generate_series(1, 10000);
-- INSERT 0 10000
`

Next, we can review the explain plans for a single record select, and a whole table select.

`
explain select * from book where id =1;
                                QUERY PLAN
---------------------------------------------------------------------------
Index Only Scan using book_pkey on book  (cost=0.28..2.49 rows=1 width=4)
Index Cond: (id = 1)
(2 rows)
explain select * from book;
                       QUERY PLAN
---------------------------------------------------------
Seq Scan on book  (cost=0.00..135.00 rows=10000 width=4)
(1 row)
`

Now we can choose a `statement_cost_filter` value between the total cost for the single select (2.49) and the whole table select (135.0) so one statement will succeed and one will fail.

`
load 'plan_filter';
set plan_filter.statement_cost_limit = 50; -- between 2.49 and 135.0
select * from book where id = 1;
id
----
1
(1 row)
-- SUCCESS
`

`
select * from book;
ERROR:  plan cost limit exceeded
HINT:  The plan for your query shows that it would probably have an excessive run time. This may be due to a logic error in the SQL, or it maybe just a very costly query. Rewrite your query or increase the configuration parameter "plan_filter.statement_cost_limit".
-- FAILURE
`

## Resources [\#](https://supabase.com/docs/guides/database/extensions/pg_plan_filter\#resources)

- Official [`pg_plan_filter` documentation](https://github.com/pgexperts/pg_plan_filter)

### Is this helpful?

NoYes

### On this page

[Enable the extension](https://supabase.com/docs/guides/database/extensions/pg_plan_filter#enable-the-extension) [API](https://supabase.com/docs/guides/database/extensions/pg_plan_filter#api) [Example](https://supabase.com/docs/guides/database/extensions/pg_plan_filter#example) [Resources](https://supabase.com/docs/guides/database/extensions/pg_plan_filter#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings