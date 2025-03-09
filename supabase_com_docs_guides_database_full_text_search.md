Database

# Full Text Search

## How to use full text search in PostgreSQL.

* * *

Postgres has built-in functions to handle `Full Text Search` queries. This is like a "search engine" within Postgres.

How to use textSearch to perform full text search - SupabaseTips - YouTube

Supabase

45.5K subscribers

[How to use textSearch to perform full text search - SupabaseTips](https://www.youtube.com/watch?v=b-mgca_2Oe4)

Supabase

Search

Info

Shopping

Tap to unmute

If playback doesn't begin shortly, try restarting your device.

You're signed out

Videos you watch may be added to the TV's watch history and influence TV recommendations. To avoid this, cancel and sign in to YouTube on your computer.

CancelConfirm

Share

Include playlist

An error occurred while retrieving sharing information. Please try again later.

Watch later

Share

Copy link

Watch on

0:00

/ •Live

•

[Watch on YouTube](https://www.youtube.com/watch?v=b-mgca_2Oe4 "Watch on YouTube")

## Preparation [\#](https://supabase.com/docs/guides/database/full-text-search\#preparation)

For this guide we'll use the following example data:

DataSQL

| id | title | author | description |
| --- | --- | --- | --- |
| 1 | The Poky Little Puppy | Janette Sebring Lowrey | Puppy is slower than other, bigger animals. |
| 2 | The Tale of Peter Rabbit | Beatrix Potter | Rabbit eats some vegetables. |
| 3 | Tootle | Gertrude Crampton | Little toy train has big dreams. |
| 4 | Green Eggs and Ham | Dr. Seuss | Sam has changing food preferences and eats unusually colored food. |
| 5 | Harry Potter and the Goblet of Fire | J.K. Rowling | Fourth year of school starts, big drama ensues. |

## Usage [\#](https://supabase.com/docs/guides/database/full-text-search\#usage)

The functions we'll cover in this guide are:

### `to_tsvector()` [\#](https://supabase.com/docs/guides/database/full-text-search\#to-tsvector)

Converts your data into searchable tokens. `to_tsvector()` stands for "to text search vector." For example:

`
select to_tsvector('green eggs and ham');
-- Returns 'egg':2 'green':1 'ham':4
`

Collectively these tokens are called a "document" which Postgres can use for comparisons.

### `to_tsquery()` [\#](https://supabase.com/docs/guides/database/full-text-search\#to-tsquery)

Converts a query string into tokens to match. `to_tsquery()` stands for "to text search query."

This conversion step is important because we will want to "fuzzy match" on keywords.
For example if a user searches for `eggs`, and a column has the value `egg`, we probably still want to return a match.

### Match: `@@` [\#](https://supabase.com/docs/guides/database/full-text-search\#match)

The `@@` symbol is the "match" symbol for Full Text Search. It returns any matches between a `to_tsvector` result and a `to_tsquery` result.

Take the following example:

SQLJavaScriptDartSwiftKotlinPython

`
select *
from books
where title = 'Harry';
`

The equality symbol above ( `=`) is very "strict" on what it matches. In a full text search context, we might want to find all "Harry Potter" books and so we can rewrite the
example above:

SQLJavaScriptDartSwiftKotlin

`
select *
from books
where to_tsvector(title) @@ to_tsquery('Harry');
`

## Basic full text queries [\#](https://supabase.com/docs/guides/database/full-text-search\#basic-full-text-queries)

### Search a single column [\#](https://supabase.com/docs/guides/database/full-text-search\#search-a-single-column)

To find all `books` where the `description` contain the word `big`:

SQLJavaScriptDartSwiftKotlinPythonData

`
select
*
from
books
where
to_tsvector(description)
@@ to_tsquery('big');
`

### Search multiple columns [\#](https://supabase.com/docs/guides/database/full-text-search\#search-multiple-columns)

Right now there is no direct way to use JavaScript or Dart to search through multiple columns but you can do it by creating [computed columns](https://postgrest.org/en/stable/api.html#computed-virtual-columns) on the database.

To find all `books` where `description` or `title` contain the word `little`:

SQLJavaScriptDartSwiftKotlinPythonData

`
select
*
from
books
where
to_tsvector(description || ' ' || title) -- concat columns, but be sure to include a space to separate them!
@@ to_tsquery('little');
`

### Match all search words [\#](https://supabase.com/docs/guides/database/full-text-search\#match-all-search-words)

To find all `books` where `description` contains BOTH of the words `little` and `big`, we can use the `&` symbol:

SQLJavaScriptDartSwiftKotlinPythonData

`
select
*
from
books
where
to_tsvector(description)
@@ to_tsquery('little & big'); -- use & for AND in the search query
`

### Match any search words [\#](https://supabase.com/docs/guides/database/full-text-search\#match-any-search-words)

To find all `books` where `description` contain ANY of the words `little` or `big`, use the `|` symbol:

SQLJavaScriptDartSwiftKotlinPythonData

`
select
*
from
books
where
to_tsvector(description)
@@ to_tsquery('little | big'); -- use | for OR in the search query
`

Notice how searching for `big` includes results with the word `bigger` (or `biggest`, etc).

## Partial search [\#](https://supabase.com/docs/guides/database/full-text-search\#partial-search)

Partial search is particularly useful when you want to find matches on substrings within your data.

### Implementing partial search [\#](https://supabase.com/docs/guides/database/full-text-search\#implementing-partial-search)

You can use the `:*` syntax with `to_tsquery()`. Here's an example that searches for any book titles beginning with "Lit":

`
select title from books where to_tsvector(title) @@ to_tsquery('Lit:*');
`

### Extending functionality with RPC [\#](https://supabase.com/docs/guides/database/full-text-search\#extending-functionality-with-rpc)

To make the partial search functionality accessible through the API, you can wrap the search logic in a stored procedure.

After creating this function, you can invoke it from your application using the SDK for your platform. Here's an example:

SQLJavaScriptDartSwiftKotlinPython

`
create or replace function search_books_by_title_prefix(prefix text)
returns setof books AS $$
begin
return query
select * from books where to_tsvector('english', title) @@ to_tsquery(prefix || ':*');
end;
$$ language plpgsql;
`

This function takes a prefix parameter and returns all books where the title contains a word starting with that prefix. The `:*` operator is used to denote a prefix match in the `to_tsquery()` function.

## Handling spaces in queries [\#](https://supabase.com/docs/guides/database/full-text-search\#handling-spaces-in-queries)

When you want the search term to include a phrase or multiple words, you can concatenate words using a `+` as a placeholder for space:

`
select * from search_books_by_title_prefix('Little+Puppy');
`

## Creating indexes [\#](https://supabase.com/docs/guides/database/full-text-search\#creating-indexes)

Now that we have Full Text Search working, let's create an `index`. This will allow Postgres to "build" the documents preemptively so that they
don't need to be created at the time we execute the query. This will make our queries much faster.

### Searchable columns [\#](https://supabase.com/docs/guides/database/full-text-search\#searchable-columns)

Let's create a new column `fts` inside the `books` table to store the searchable index of the `title` and `description` columns.

We can use a special feature of Postgres called
[Generated Columns](https://www.postgresql.org/docs/current/ddl-generated-columns.html)
to ensure that the index is updated any time the values in the `title` and `description` columns change.

SQLData

`
alter table
books
add column
fts tsvector generated always as (to_tsvector('english', description || ' ' || title)) stored;
create index books_fts on books using gin (fts); -- generate the index
select id, fts
from books;
`

### Search using the new column [\#](https://supabase.com/docs/guides/database/full-text-search\#search-using-the-new-column)

Now that we've created and populated our index, we can search it using the same techniques as before:

SQLJavaScriptDartSwiftKotlinPythonData

`
select
*
from
books
where
fts @@ to_tsquery('little & big');
`

## Query operators [\#](https://supabase.com/docs/guides/database/full-text-search\#query-operators)

Visit [Postgres: Text Search Functions and Operators](https://www.postgresql.org/docs/current/functions-textsearch.html)
to learn about additional query operators you can use to do more advanced `full text queries`, such as:

### Proximity: `<->` [\#](https://supabase.com/docs/guides/database/full-text-search\#proximity)

The proximity symbol is useful for searching for terms that are a certain "distance" apart.
For example, to find the phrase `big dreams`, where the a match for "big" is followed immediately by a match for "dreams":

SQLJavaScriptDartSwiftKotlinPython

`
select
*
from
books
where
to_tsvector(description) @@ to_tsquery('big <-> dreams');
`

We can also use the `<->` to find words within a certain distance of each other. For example to find `year` and `school` within 2 words of each other:

SQLJavaScriptDartSwiftKotlinPython

`
select
*
from
books
where
to_tsvector(description) @@ to_tsquery('year <2> school');
`

### Negation: `!` [\#](https://supabase.com/docs/guides/database/full-text-search\#negation)

The negation symbol can be used to find phrases which _don't_ contain a search term.
For example, to find records that have the word `big` but not `little`:

SQLJavaScriptDartSwiftKotlinPython

`
select
*
from
books
where
to_tsvector(description) @@ to_tsquery('big & !little');
`

## Resources [\#](https://supabase.com/docs/guides/database/full-text-search\#resources)

- [Postgres: Text Search Functions and Operators](https://www.postgresql.org/docs/12/functions-textsearch.html)

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2Fb-mgca_2Oe4%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Preparation](https://supabase.com/docs/guides/database/full-text-search#preparation) [Usage](https://supabase.com/docs/guides/database/full-text-search#usage) [to\_tsvector()](https://supabase.com/docs/guides/database/full-text-search#to-tsvector) [to\_tsquery()](https://supabase.com/docs/guides/database/full-text-search#to-tsquery) [Match: @@](https://supabase.com/docs/guides/database/full-text-search#match) [Basic full text queries](https://supabase.com/docs/guides/database/full-text-search#basic-full-text-queries) [Search a single column](https://supabase.com/docs/guides/database/full-text-search#search-a-single-column) [Search multiple columns](https://supabase.com/docs/guides/database/full-text-search#search-multiple-columns) [Match all search words](https://supabase.com/docs/guides/database/full-text-search#match-all-search-words) [Match any search words](https://supabase.com/docs/guides/database/full-text-search#match-any-search-words) [Partial search](https://supabase.com/docs/guides/database/full-text-search#partial-search) [Implementing partial search](https://supabase.com/docs/guides/database/full-text-search#implementing-partial-search) [Extending functionality with RPC](https://supabase.com/docs/guides/database/full-text-search#extending-functionality-with-rpc) [Handling spaces in queries](https://supabase.com/docs/guides/database/full-text-search#handling-spaces-in-queries) [Creating indexes](https://supabase.com/docs/guides/database/full-text-search#creating-indexes) [Searchable columns](https://supabase.com/docs/guides/database/full-text-search#searchable-columns) [Search using the new column](https://supabase.com/docs/guides/database/full-text-search#search-using-the-new-column) [Query operators](https://supabase.com/docs/guides/database/full-text-search#query-operators) [Proximity: <->](https://supabase.com/docs/guides/database/full-text-search#proximity) [Negation: !](https://supabase.com/docs/guides/database/full-text-search#negation) [Resources](https://supabase.com/docs/guides/database/full-text-search#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings