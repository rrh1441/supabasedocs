# Canceling statement due to "statement timeout"

Last edited: 2/4/2025

* * *

> If encountering 504 or timeout errors in the Dashboard, check out this [guide](https://github.com/orgs/supabase/discussions/21133#discussioncomment-9573776)

You can run this query to check the current settings set for your roles: `SELECT rolname, rolconfig FROM   pg_roles;`

To increase the `statement_timeout` for a specific role, you may follow the instructions [here](https://supabase.com/docs/guides/database/timeouts#changing-the-default-timeout). Note that it may require a quick reboot for the changes to take effect.

Additionally, to check how long a query is taking, you can check the Query Performance report which can give you more information on the query's performance: [https://app.supabase.com/project/\_/reports/query-performance](https://app.supabase.com/project/_/reports/query-performance). You can use the [query plan analyzer](https://www.postgresql.org/docs/current/sql-explain.html) on any expensive queries that you have identified: `explain analyze <query-statement-here>;`. For supabase-js/ PostgREST queries you can use `.explain()`.

You can also make use of Postgres logs that will give you useful information like when the query was executed: [https://app.supabase.com/project/\_/logs/postgres-logs](https://app.supabase.com/project/_/logs/postgres-logs).

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings