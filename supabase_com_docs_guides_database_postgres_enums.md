Database

# Managing Enums in Postgres

* * *

Enums in Postgres are a custom data type. They allow you to define a set of values (or labels) that a column can hold. They are useful when you have a fixed set of possible values for a column.

## Creating enums [\#](https://supabase.com/docs/guides/database/postgres/enums\#creating-enums)

You can define a Postgres Enum using the `create type` statement. Here's an example:

`
create type mood as enum (
'happy',
'sad',
'excited',
'calm'
);
`

In this example, we've created an Enum called "mood" with four possible values.

## When to use enums [\#](https://supabase.com/docs/guides/database/postgres/enums\#when-to-use-enums)

There is a lot of overlap between Enums and foreign keys. Both can be used to define a set of values for a column. However, there are some advantages to using Enums:

- Performance: You can query a single table instead of finding the value from a lookup table.
- Simplicity: Generally the SQL is easier to read and write.

There are also some disadvantages to using Enums:

- Limited Flexibility: Adding and removing values requires modifying the database schema (i.e.: using migrations) rather than adding data to a table.
- Maintenance Overhead: Enum types require ongoing maintenance. If your application's requirements change frequently, maintaining enums can become burdensome.

In general you should only use Enums when the list of values is small, fixed, and unlikely to change often. Things like "a list of continents" or "a list of departments" are good candidates for Enums.

## Using enums in tables [\#](https://supabase.com/docs/guides/database/postgres/enums\#using-enums-in-tables)

To use the Enum in a table, you can define a column with the Enum type. For example:

`
create table person (
id serial primary key,
name text,
current_mood mood
);
`

Here, the `current_mood` column can only have values from the "mood" Enum.

### Inserting data with enums [\#](https://supabase.com/docs/guides/database/postgres/enums\#inserting-data-with-enums)

You can insert data into a table with Enum columns by specifying one of the Enum values:

`
insert into person
(name, current_mood)
values
('Alice', 'happy');
`

### Querying data with enums [\#](https://supabase.com/docs/guides/database/postgres/enums\#querying-data-with-enums)

When querying data, you can filter and compare Enum values as usual:

`
select *
from person
where current_mood = 'sad';
`

## Managing enums [\#](https://supabase.com/docs/guides/database/postgres/enums\#managing-enums)

You can manage your Enums using the `alter type` statement. Here are some examples:

### Updating enum values [\#](https://supabase.com/docs/guides/database/postgres/enums\#updating-enum-values)

You can update the value of an Enum column:

`
update person
set current_mood = 'excited'
where name = 'Alice';
`

### Adding enum values [\#](https://supabase.com/docs/guides/database/postgres/enums\#adding-enum-values)

To add new values to an existing Postgres Enum, you can use the `ALTER TYPE` statement. Here's how you can do it:

Let's say you have an existing Enum called `mood`, and you want to add a new value, `content`:

`
alter type mood add value 'content';
`

### Removing enum values [\#](https://supabase.com/docs/guides/database/postgres/enums\#removing-enum-values)

Even though it is possible, it is unsafe to remove enum values once they have been created. It's better to leave the enum value in place.

Read the [Postgres mailing list](https://www.postgresql.org/message-id/21012.1459434338%40sss.pgh.pa.us) for more information:

There is no `ALTER TYPE DELETE VALUE` in Postgres. Even if you delete every occurrence of an Enum value within a table (and vacuumed away those rows), the target value could still exist in upper index pages. If you delete the `pg_enum` entry you'll break the index.

### Getting a list of enum values [\#](https://supabase.com/docs/guides/database/postgres/enums\#getting-a-list-of-enum-values)

Check your existing Enum values by querying the enum\_range function:

`
select enum_range(null::mood);
`

## Resources [\#](https://supabase.com/docs/guides/database/postgres/enums\#resources)

- Official Postgres Docs: [Enumerated Types](https://www.postgresql.org/docs/current/datatype-enum.html)

### Is this helpful?

NoYes

### On this page

[Creating enums](https://supabase.com/docs/guides/database/postgres/enums#creating-enums) [When to use enums](https://supabase.com/docs/guides/database/postgres/enums#when-to-use-enums) [Using enums in tables](https://supabase.com/docs/guides/database/postgres/enums#using-enums-in-tables) [Inserting data with enums](https://supabase.com/docs/guides/database/postgres/enums#inserting-data-with-enums) [Querying data with enums](https://supabase.com/docs/guides/database/postgres/enums#querying-data-with-enums) [Managing enums](https://supabase.com/docs/guides/database/postgres/enums#managing-enums) [Updating enum values](https://supabase.com/docs/guides/database/postgres/enums#updating-enum-values) [Adding enum values](https://supabase.com/docs/guides/database/postgres/enums#adding-enum-values) [Removing enum values](https://supabase.com/docs/guides/database/postgres/enums#removing-enum-values) [Getting a list of enum values](https://supabase.com/docs/guides/database/postgres/enums#getting-a-list-of-enum-values) [Resources](https://supabase.com/docs/guides/database/postgres/enums#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings