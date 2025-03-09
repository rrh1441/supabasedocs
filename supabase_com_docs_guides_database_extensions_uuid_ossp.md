Database

# uuid-ossp: Unique Identifiers

* * *

The `uuid-ossp` extension can be used to generate a `UUID`.

## Overview [\#](https://supabase.com/docs/guides/database/extensions/uuid-ossp\#overview)

A `UUID` is a "Universally Unique Identifier" and it is, for practical purposes, unique.
This makes them particularly well suited as Primary Keys. It is occasionally referred to as a `GUID`, which stands for "Globally Unique Identifier".

## Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/uuid-ossp\#enable-the-extension)

**Note**:
Currently `uuid-ossp` extension is enabled by default and cannot be disabled.

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Extensions** in the sidebar.
3. Search for `uuid-ossp` and enable the extension.

## The `uuid` type [\#](https://supabase.com/docs/guides/database/extensions/uuid-ossp\#the-uuid-type)

Once the extension is enabled, you now have access to a `uuid` type.

## `uuid_generate_v1()` [\#](https://supabase.com/docs/guides/database/extensions/uuid-ossp\#uuidgeneratev1)

Creates a UUID value based on the combination of computer’s MAC address, current timestamp, and a random value.

UUIDv1 leaks identifiable details, which might make it unsuitable for certain security-sensitive applications.

## `uuid_generate_v4()` [\#](https://supabase.com/docs/guides/database/extensions/uuid-ossp\#uuidgeneratev4)

Creates UUID values based solely on random numbers. You can also use Postgres's built-in [`gen_random_uuid()`](https://www.postgresql.org/docs/current/functions-uuid.html) function to generate a UUIDv4.

## Examples [\#](https://supabase.com/docs/guides/database/extensions/uuid-ossp\#examples)

### Within a query [\#](https://supabase.com/docs/guides/database/extensions/uuid-ossp\#within-a-query)

`
select uuid_generate_v4();
`

### As a primary key [\#](https://supabase.com/docs/guides/database/extensions/uuid-ossp\#as-a-primary-key)

Automatically create a unique, random ID in a table:

`
create table contacts (
id uuid default uuid_generate_v4(),
first_name text,
last_name text,
primary key (id)
);
`

## Resources [\#](https://supabase.com/docs/guides/database/extensions/uuid-ossp\#resources)

- [Choosing a Postgres Primary Key](https://supabase.com/blog/choosing-a-postgres-primary-key)
- [The Basics Of Postgres `UUID` Data Type](https://www.postgresqltutorial.com/postgresql-uuid/)

### Is this helpful?

NoYes

### On this page

[Overview](https://supabase.com/docs/guides/database/extensions/uuid-ossp#overview) [Enable the extension](https://supabase.com/docs/guides/database/extensions/uuid-ossp#enable-the-extension) [The uuid type](https://supabase.com/docs/guides/database/extensions/uuid-ossp#the-uuid-type) [uuid\_generate\_v1()](https://supabase.com/docs/guides/database/extensions/uuid-ossp#uuidgeneratev1) [uuid\_generate\_v4()](https://supabase.com/docs/guides/database/extensions/uuid-ossp#uuidgeneratev4) [Examples](https://supabase.com/docs/guides/database/extensions/uuid-ossp#examples) [Within a query](https://supabase.com/docs/guides/database/extensions/uuid-ossp#within-a-query) [As a primary key](https://supabase.com/docs/guides/database/extensions/uuid-ossp#as-a-primary-key) [Resources](https://supabase.com/docs/guides/database/extensions/uuid-ossp#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings