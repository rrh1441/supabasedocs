Database

# Select first row for each group in PostgreSQL

* * *

Given a table `seasons`:

| id | team | points |
| --- | :-: | --: |
| 1 | Liverpool | 82 |
| 2 | Liverpool | 84 |
| 3 | Brighton | 34 |
| 4 | Brighton | 28 |
| 5 | Liverpool | 79 |

We want to find the rows containing the maximum number of points _per team_.

The expected output we want is:

| id | team | points |
| --- | :-: | --: |
| 3 | Brighton | 34 |
| 2 | Liverpool | 84 |

From the [SQL Editor](https://supabase.com/dashboard/project/_/sql), you can run a query like:

`
select distinct
on (team) id,
team,
points
from
seasons
order BY
id,
points desc,
team;
`

The important bits here are:

- The `desc` keyword to order the `points` from highest to lowest.
- The `distinct` keyword that tells Postgres to only return a single row per team.

This query can also be executed via `psql` or any other query editor if you prefer to [connect directly to the database](https://supabase.com/docs/guides/database/connecting-to-postgres#direct-connections).

### Is this helpful?

NoYes

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings