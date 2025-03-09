Database

# pg\_jsonschema: JSON Schema Validation

* * *

[JSON Schema](https://json-schema.org/) is a language for annotating and validating JSON documents. [`pg_jsonschema`](https://github.com/supabase/pg_jsonschema) is a Postgres extension that adds the ability to validate PostgreSQL's built-in `json` and `jsonb` data types against JSON Schema documents.

## Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/pg_jsonschema\#enable-the-extension)

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Extensions** in the sidebar.
3. Search for `pg_jsonschema` and enable the extension.

## Functions [\#](https://supabase.com/docs/guides/database/extensions/pg_jsonschema\#functions)

- [`json_matches_schema(schema json, instance json)`](https://github.com/supabase/pg_jsonschema#api): Checks if a `json` _instance_ conforms to a JSON Schema _schema_.
- [`jsonb_matches_schema(schema json, instance jsonb)`](https://github.com/supabase/pg_jsonschema#api): Checks if a `jsonb` _instance_ conforms to a JSON Schema _schema_.

## Usage [\#](https://supabase.com/docs/guides/database/extensions/pg_jsonschema\#usage)

Since `pg_jsonschema` exposes its utilities as functions, we can execute them with a select statement:

`
select
extensions.json_matches_schema(
    schema := '{"type": "object"}',
    instance := '{}'
);
`

`pg_jsonschema` is generally used in tandem with a [check constraint](https://www.postgresql.org/docs/current/ddl-constraints.html) as a way to constrain the contents of a json/b column to match a JSON Schema.

`
create table customer(
    id serial primary key,
    ...
    metadata json,
    check (
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
    )
);
-- Example: Valid Payload
insert into customer(metadata)
values ('{"tags": ["vip", "darkmode-ui"]}');
-- Result:
--   INSERT 0 1
-- Example: Invalid Payload
insert into customer(metadata)
values ('{"tags": [1, 3]}');
-- Result:
--   ERROR:  new row for relation "customer" violates check constraint "customer_metadata_check"
--   DETAIL:  Failing row contains (2, {"tags": [1, 3]}).
`

## Resources [\#](https://supabase.com/docs/guides/database/extensions/pg_jsonschema\#resources)

- Official [`pg_jsonschema` documentation](https://github.com/supabase/pg_jsonschema)

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2FamJo48ChLGs%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Enable the extension](https://supabase.com/docs/guides/database/extensions/pg_jsonschema#enable-the-extension) [Functions](https://supabase.com/docs/guides/database/extensions/pg_jsonschema#functions) [Usage](https://supabase.com/docs/guides/database/extensions/pg_jsonschema#usage) [Resources](https://supabase.com/docs/guides/database/extensions/pg_jsonschema#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings