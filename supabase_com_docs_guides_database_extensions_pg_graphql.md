Database

# pg\_graphql: GraphQL for PostgreSQL

* * *

[pg\_graphql](https://supabase.github.io/pg_graphql/) is Postgres extension for interacting with the database using [GraphQL](https://graphql.org/) instead of SQL.

The extension reflects a GraphQL schema from the existing SQL schema and exposes it through a SQL function, `graphql.resolve(...)`. This enables any programming language that can connect to Postgres to query the database via GraphQL with no additional servers, processes, or libraries.

The `pg_graphql` resolve method is designed to interop with [PostgREST](https://postgrest.org/en/stable/index.html), the tool that underpins the Supabase API, such that the `graphql.resolve` function can be called via RPC to safely and performantly expose the GraphQL API over HTTP/S.

For more information about how the SQL schema is reflected into a GraphQL schema, see the [pg\_graphql API docs](https://supabase.github.io/pg_graphql/api/).

## Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/pg_graphql\#enable-the-extension)

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Extensions** in the sidebar.
3. Search for "pg\_graphql" and enable the extension.

## Usage [\#](https://supabase.com/docs/guides/database/extensions/pg_graphql\#usage)

Given a table

`
create table "Blog"(
id serial primary key,
name text not null,
description text
);
insert into "Blog"(name)
values ('My Blog');
`

The reflected GraphQL schema can be queried immediately as

`
select
graphql.resolve($$
    {
      blogCollection(first: 1) {
        edges {
          node {
            id,
            name
          }
        }
      }
    }
$$);
`

returning the JSON

`
{
"data": {
    "blogCollection": {
      "edges": [\
        {\
          "node": {\
            "id": 1\
            "name": "My Blog"\
          }\
        }\
      ]
    }
}
}
`

Note that `pg_graphql` fully supports schema introspection so you can connect any GraphQL IDE or schema inspection tool to see the full set of fields and arguments available in the API.

## API [\#](https://supabase.com/docs/guides/database/extensions/pg_graphql\#api)

- [`graphql.resolve`](https://supabase.github.io/pg_graphql/sql_interface/): A SQL function for executing GraphQL queries.

## Resources [\#](https://supabase.com/docs/guides/database/extensions/pg_graphql\#resources)

- Official [`pg_graphql` documentation](https://github.com/supabase/pg_graphql)

### Is this helpful?

NoYes

### On this page

[Enable the extension](https://supabase.com/docs/guides/database/extensions/pg_graphql#enable-the-extension) [Usage](https://supabase.com/docs/guides/database/extensions/pg_graphql#usage) [API](https://supabase.com/docs/guides/database/extensions/pg_graphql#api) [Resources](https://supabase.com/docs/guides/database/extensions/pg_graphql#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings