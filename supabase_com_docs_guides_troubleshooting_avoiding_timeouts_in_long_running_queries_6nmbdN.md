# Avoiding timeouts in long running queries

Last edited: 2/21/2025

* * *

This page covers long-running queries via the Dashboard or external interfaces. For Supabase Client API timeout errors, check this [guide](https://github.com/orgs/supabase/discussions/14256) instead.

Certain queries, like indexing a table or changing a column's data type, are inherently time-consuming. The [Dashboard's SQL Editor](https://supabase.com/dashboard/project/_/sql/) has a 1-minute timeout limit.

To execute long-running queries, follow the below steps.

### Install an external SQL client [\#](https://supabase.com/docs/guides/troubleshooting/avoiding-timeouts-in-long-running-queries-6nmbdN\#install-an-external-sql-client)

The guide focuses on [psql](https://supabase.com/docs/guides/database/psql) but you can use any Postgres client.

Some other options include:

- [pgAdmin](https://supabase.com/docs/guides/database/pgadmin)
- [DBeaver](https://supabase.com/docs/guides/database/dbeaver)

You can install PSQL in [macOS](https://stackoverflow.com/a/49689589/2188186) and [Windows](https://www.postgresql.org/download/windows/) by following these links and instructions. For Linux (Debian) you can run the following:

`
sudo apt-get update
sudo apt-get install postgresql-client
`

Once installed, you can find your PSQL string in the [Database Settings](https://supabase.com/dashboard/project/_/settings/database). Make sure if you are using the pooler connection that it is in session mode (port 5432).

If you are working in an [IPv6 environment](https://github.com/orgs/supabase/discussions/27034) or have the IPv4 Add-On, it is preferable to use the direct connection.

### Increase the query timeout [\#](https://supabase.com/docs/guides/troubleshooting/avoiding-timeouts-in-long-running-queries-6nmbdN\#increase-the-query-timeout)

Then you can increase the query timeout solely for your session:

Long timeout

`
set statement_timeout = '120min';
`

Disable timeout

`
set statement_timeout = '0';
`

If your task is particularly long, you can may want to consider boosting your computing power temporarily. Compute size on Supabase is charged by the hour, so you can increase it for an hour or two, complete your task faster, then scale it back afterwards.

If you want to temporarily upgrade, you can find the add-ons for your project in your [Dashboard's Add-ons Settings.](https://supabase.green/dashboard/project/_/settings/addons)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings