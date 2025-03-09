Database

# pg\_hashids: Short UIDs

* * *

[pg\_hashids](https://github.com/iCyberon/pg_hashids) provides a secure way to generate short, unique, non-sequential ids from numbers. The hashes are intended to be small, easy-to-remember identifiers that can be used to obfuscate data (optionally) with a password, alphabet, and salt. For example, you may wish to hide data like user IDs, order numbers, or tracking codes in favor of `pg_hashid`'s unique identifiers.

## Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/pg_hashids\#enable-the-extension)

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Extensions** in the sidebar.
3. Search for "pg\_hashids" and enable the extension.

## Usage [\#](https://supabase.com/docs/guides/database/extensions/pg_hashids\#usage)

Suppose we have a table that stores order information, and we want to give customers a unique identifier without exposing the sequential `id` column. To do this, we can use `pg_hashid`'s `id_encode` function.

`
create table orders (
id serial primary key,
description text,
price_cents bigint
);
insert into orders (description, price_cents)
values ('a book', 9095);
select
id,
id_encode(id) as short_id,
description,
price_cents
from
orders;
id | short_id | description | price_cents
----+----------+-------------+-------------
1 | jR       | a book      |        9095
(1 row)
`

To reverse the `short_id` back into an `id`, there is an equivalent function named `id_decode`.

## Resources [\#](https://supabase.com/docs/guides/database/extensions/pg_hashids\#resources)

- Official [pg\_hashids documentation](https://github.com/iCyberon/pg_hashids)

### Is this helpful?

NoYes

### On this page

[Enable the extension](https://supabase.com/docs/guides/database/extensions/pg_hashids#enable-the-extension) [Usage](https://supabase.com/docs/guides/database/extensions/pg_hashids#usage) [Resources](https://supabase.com/docs/guides/database/extensions/pg_hashids#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings