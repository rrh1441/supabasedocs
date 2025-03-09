Database

# Managing JSON and unstructured data

## Using the JSON data type in Postgres.

* * *

Postgres supports storing and querying unstructured data.

## JSON vs JSONB [\#](https://supabase.com/docs/guides/database/json\#json-vs-jsonb)

Postgres supports two types of JSON columns: `json` (stored as a string) and `jsonb` (stored as a binary). The recommended type is `jsonb` for almost all cases.

- `json` stores an exact copy of the input text. Database functions must reparse the content on each execution.
- `jsonb` stores database in a decomposed binary format. While this makes it slightly slower to input due to added conversion overhead, it is significantly faster to process, since no reparsing is needed.

## When to use JSON/JSONB [\#](https://supabase.com/docs/guides/database/json\#when-to-use-jsonjsonb)

Generally you should use a `jsonb` column when you have data that is unstructured or has a variable schema. For example, if you wanted to store responses for various webhooks, you might not know the format of the response when creating the table. Instead, you could store the `payload` as a `jsonb` object in a single column.

Don't go overboard with `json/jsonb` columns. They are a useful tool, but most of the benefits of a relational database come from the ability to query and join structured data, and the referential integrity that brings.

## Create JSONB columns [\#](https://supabase.com/docs/guides/database/json\#create-jsonb-columns)

`json/jsonb` is just another "data type" for Postgres columns. You can create a `jsonb` column in the same way you would create a `text` or `int` column:

SQLDashboard

`
create table books (
id serial primary key,
title text,
author text,
metadata jsonb
);
`

## Inserting JSON data [\#](https://supabase.com/docs/guides/database/json\#inserting-json-data)

You can insert JSON data in the same way that you insert any other data. The data must be valid JSON.

SQLDashboardJavaScriptDartSwiftKotlinPython

`
insert into books
(title, author, metadata)
values
(
    'The Poky Little Puppy',
    'Janette Sebring Lowrey',
    '{"description":"Puppy is slower than other, bigger animals.","price":5.95,"ages":[3,6]}'
),
(
    'The Tale of Peter Rabbit',
    'Beatrix Potter',
    '{"description":"Rabbit eats some vegetables.","price":4.49,"ages":[2,5]}'
),
(
    'Tootle',
    'Gertrude Crampton',
    '{"description":"Little toy train has big dreams.","price":3.99,"ages":[2,5]}'
),
(
    'Green Eggs and Ham',
    'Dr. Seuss',
    '{"description":"Sam has changing food preferences and eats unusually colored food.","price":7.49,"ages":[4,8]}'
),
(
    'Harry Potter and the Goblet of Fire',
    'J.K. Rowling',
    '{"description":"Fourth year of school starts, big drama ensues.","price":24.95,"ages":[10,99]}'
);
`

## Query JSON data [\#](https://supabase.com/docs/guides/database/json\#query-json-data)

Querying JSON data is similar to querying other data, with a few other features to access nested values.

Postgres support a range of [JSON functions and operators](https://www.postgresql.org/docs/current/functions-json.html). For example, the `->` operator returns values as `jsonb` data. If you want the data returned as `text`, use the `->>` operator.

SQLJavaScriptSwiftKotlinPythonResult

`
select
title,
metadata ->> 'description' as description, -- returned as text
metadata -> 'price' as price,
metadata -> 'ages' -> 0 as low_age,
metadata -> 'ages' -> 1 as high_age
from books;
`

## Validating JSON data [\#](https://supabase.com/docs/guides/database/json\#validating-json-data)

Supabase provides the [`pg_jsonschema` extension](https://supabase.com/docs/guides/database/extensions/pg_jsonschema) that adds the ability to validate `json` and `jsonb` data types against [JSON Schema](https://json-schema.org/) documents.

Once you have enabled the extension, you can add a "check constraint" to your table to validate the JSON data:

`
create table customers (
id serial primary key,
metadata json
);
alter table customers
add constraint check_metadata check (
json_matches_schema(
    '{
        "type": "object",
        "properties": {
            "tags": {
                "type": "array",
                "items": {
                    "type": "string",
                    "maxLength": 16
                }
            }
        }
    }',
    metadata
)
);
`

## Resources [\#](https://supabase.com/docs/guides/database/json\#resources)

- [Postgres: JSON Functions and Operators](https://www.postgresql.org/docs/current/functions-json.html)
- [Postgres JSON types](https://www.postgresql.org/docs/current/datatype-json.html)

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2FnxeUiRz4G-M%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[JSON vs JSONB](https://supabase.com/docs/guides/database/json#json-vs-jsonb) [When to use JSON/JSONB](https://supabase.com/docs/guides/database/json#when-to-use-jsonjsonb) [Create JSONB columns](https://supabase.com/docs/guides/database/json#create-jsonb-columns) [Inserting JSON data](https://supabase.com/docs/guides/database/json#inserting-json-data) [Query JSON data](https://supabase.com/docs/guides/database/json#query-json-data) [Validating JSON data](https://supabase.com/docs/guides/database/json#validating-json-data) [Resources](https://supabase.com/docs/guides/database/json#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings