Database

# PGroonga: Multilingual Full Text Search

* * *

`PGroonga` is a Postgres extension adding a full text search indexing method based on [Groonga](https://groonga.org/). While native Postgres supports full text indexing, it is limited to alphabet and digit based languages. `PGroonga` offers a wider range of character support making it viable for a superset of languages supported by Postgres including Japanese, Chinese, etc.

## Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/pgroonga\#enable-the-extension)

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Extensions** in the sidebar.
3. Search for `pgroonga` and enable the extension.

## Creating a full text search index [\#](https://supabase.com/docs/guides/database/extensions/pgroonga\#creating-a-full-text-search-index)

Given a table with a `text` column:

`
create table memos (
id serial primary key,
content text
);
`

We can index the column for full text search with a `pgroonga` index:

`
create index ix_memos_content ON memos USING pgroonga(content);
`

To test the full text index, we'll add some data.

`
insert into memos(content)
values
('PostgreSQL is a relational database management system.'),
('Groonga is a fast full text search engine that supports all languages.'),
('PGroonga is a PostgreSQL extension that uses Groonga as index.'),
('There is groonga command.');
`

The Postgres query planner is smart enough to know that, for extremely small tables, it's faster to scan the whole table rather than loading an index. To force the index to be used, we can disable sequential scans:

`
-- For testing only. Don't do this in production
set enable_seqscan = off;
`

Now if we run an explain plan on a query filtering on `memos.content`:

`
explain select * from memos where content like '%engine%';
                               QUERY PLAN
-----------------------------------------------------------------------------
Index Scan using ix_memos_content on memos  (cost=0.00..1.11 rows=1 width=36)
Index Cond: (content ~~ '%engine%'::text)
(2 rows)
`

The `pgroonga` index is used to retrieve the result set:

`
| id  | content                                                                  |
| --- | ------------------------------------------------------------------------ |
| 2   | 'Groonga is a fast full text search engine that supports all languages.' |
`

## Full text search [\#](https://supabase.com/docs/guides/database/extensions/pgroonga\#full-text-search)

The `&@~` operator performs full text search. It returns any matching results. Unlike `LIKE` operator, `pgroonga` can search any text that contains the keyword case insensitive.

Take the following example:

`
select * from memos where content &@~ 'groonga';
`

And the result:

`
id | content
----+------------------------------------------------------------------------
2 | Groonga is a fast full text search engine that supports all languages.
3 | PGroonga is a PostgreSQL extension that uses Groonga as index.
4 | There is groonga command.
(3 rows)
`

### Match all search words [\#](https://supabase.com/docs/guides/database/extensions/pgroonga\#match-all-search-words)

To find all memos where content contains BOTH of the words `postgres` and `pgroonga`, we can just use space to separate each words:

`
select * from memos where content &@~ 'postgres pgroonga';
`

And the result:

`
id | content
----+----------------------------------------------------------------
3 | PGroonga is a PostgreSQL extension that uses Groonga as index.
(1 row)
`

### Match any search words [\#](https://supabase.com/docs/guides/database/extensions/pgroonga\#match-any-search-words)

To find all memos where content contain ANY of the words `postgres` or `pgroonga`, use the upper case `OR`:

`
select * from memos where content &@~ 'postgres OR pgroonga';
`

And the result:

`
id | content
----+----------------------------------------------------------------
1 | PostgreSQL is a relational database management system.
3 | PGroonga is a PostgreSQL extension that uses Groonga as index.
(2 rows)
`

### Search that matches words with negation [\#](https://supabase.com/docs/guides/database/extensions/pgroonga\#search-that-matches-words-with-negation)

To find all memos where content contain the word `postgres` but not `pgroonga`, use `-` symbol:

`
select * from memos where content &@~ 'postgres -pgroonga';
`

And the result:

`
id | content
----+--------------------------------------------------------
1 | PostgreSQL is a relational database management system.
(1 row)
`

## Resources [\#](https://supabase.com/docs/guides/database/extensions/pgroonga\#resources)

- Official [PGroonga documentation](https://pgroonga.github.io/tutorial/)

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2FMmmv9g_MiBA%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Enable the extension](https://supabase.com/docs/guides/database/extensions/pgroonga#enable-the-extension) [Creating a full text search index](https://supabase.com/docs/guides/database/extensions/pgroonga#creating-a-full-text-search-index) [Full text search](https://supabase.com/docs/guides/database/extensions/pgroonga#full-text-search) [Match all search words](https://supabase.com/docs/guides/database/extensions/pgroonga#match-all-search-words) [Match any search words](https://supabase.com/docs/guides/database/extensions/pgroonga#match-any-search-words) [Search that matches words with negation](https://supabase.com/docs/guides/database/extensions/pgroonga#search-that-matches-words-with-negation) [Resources](https://supabase.com/docs/guides/database/extensions/pgroonga#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings