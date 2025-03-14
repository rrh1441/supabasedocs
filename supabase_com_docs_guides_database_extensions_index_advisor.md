Database

# index\_advisor: query optimization

* * *

[Index advisor](https://github.com/supabase/index_advisor) is a Postgres extension for recommending indexes to improve query performance.

Features:

- Supports generic parameters e.g. `$1`, `$2`
- Supports materialized views
- Identifies tables/columns obfuscated by views
- Skips duplicate indexes

`index_advisor` is accessible directly through Supabase Studio by navigating to the [Query Performance Report](https://supabase.com/dashboard/project/_/advisors/query-performance) and selecting a query and then the "indexes" tab.

![Supabase Studio index_advisor integration.](https://supabase.com/docs/img/index_advisor_studio.png)

Alternatively, you can use index\_advisor directly via SQL.

For example:

`
select
    *
from
index_advisor('select book.id from book where title = $1');
startup_cost_before | startup_cost_after | total_cost_before | total_cost_after |                  index_statements                   | errors
---------------------+--------------------+-------------------+------------------+-----------------------------------------------------+--------
0.00                | 1.17               | 25.88             | 6.40             | {"CREATE INDEX ON public.book USING btree (title)"},| {}
(1 row)
`

## Installation [\#](https://supabase.com/docs/guides/database/extensions/index_advisor\#installation)

To get started, enable index\_advisor by running

`
create extension index_advisor;
`

## API [\#](https://supabase.com/docs/guides/database/extensions/index_advisor\#api)

Index advisor exposes a single function `index_advisor(query text)` that accepts a query and searches for a set of SQL DDL `create index` statements that improve the query's execution time.

The function's signature is:

`
index_advisor(query text)
returns
    table  (
        startup_cost_before jsonb,
        startup_cost_after jsonb,
        total_cost_before jsonb,
        total_cost_after jsonb,
        index_statements text[],
        errors text[]
    )
`

## Usage [\#](https://supabase.com/docs/guides/database/extensions/index_advisor\#usage)

As a minimal example, the `index_advisor` function can be given a single table query with a filter on an unindexed column.

`
create extension if not exists index_advisor cascade;
create table book(
id int primary key,
title text not null
);
select
*
from
index_advisor('select book.id from book where title = $1');
startup_cost_before | startup_cost_after | total_cost_before | total_cost_after |                  index_statements                   | errors
---------------------+--------------------+-------------------+------------------+-----------------------------------------------------+--------
0.00                | 1.17               | 25.88             | 6.40             | {"CREATE INDEX ON public.book USING btree (title)"},| {}
(1 row)
`

and will return a row recommending an index on the unindexed column.

More complex queries may generate additional suggested indexes:

`
create extension if not exists index_advisor cascade;
create table author(
    id serial primary key,
    name text not null
);
create table publisher(
    id serial primary key,
    name text not null,
    corporate_address text
);
create table book(
    id serial primary key,
    author_id int not null references author(id),
    publisher_id int not null references publisher(id),
    title text
);
create table review(
    id serial primary key,
    book_id int references book(id),
    body text not null
);
select
    *
from
    index_advisor('
        select
            book.id,
            book.title,
            publisher.name as publisher_name,
            author.name as author_name,
            review.body review_body
        from
            book
            join publisher
                on book.publisher_id = publisher.id
            join author
                on book.author_id = author.id
            join review
                on book.id = review.book_id
        where
            author.id = $1
            and publisher.id = $2
    ');
startup_cost_before | startup_cost_after | total_cost_before | total_cost_after |                  index_statements                         | errors
---------------------+--------------------+-------------------+------------------+-----------------------------------------------------------+--------
27.26               | 12.77              | 68.48             | 42.37            | {"CREATE INDEX ON public.book USING btree (author_id)",   | {}
                                                                                    "CREATE INDEX ON public.book USING btree (publisher_id)",
                                                                                    "CREATE INDEX ON public.review USING btree (book_id)"}
(3 rows)
`

## Limitations [\#](https://supabase.com/docs/guides/database/extensions/index_advisor\#limitations)

- index\_advisor will only recommend single column, B-tree indexes. More complex indexes will be supported in future releases.
- when a generic argument's type is not discernible from context, an error is returned in the `errors` field. To resolve those errors, add explicit type casting to the argument. e.g. `$1::int`.

## Resources [\#](https://supabase.com/docs/guides/database/extensions/index_advisor\#resources)

- [`index_advisor`](https://github.com/supabase/index_advisor) repo

### Is this helpful?

NoYes

### On this page

[Installation](https://supabase.com/docs/guides/database/extensions/index_advisor#installation) [API](https://supabase.com/docs/guides/database/extensions/index_advisor#api) [Usage](https://supabase.com/docs/guides/database/extensions/index_advisor#usage) [Limitations](https://supabase.com/docs/guides/database/extensions/index_advisor#limitations) [Resources](https://supabase.com/docs/guides/database/extensions/index_advisor#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings