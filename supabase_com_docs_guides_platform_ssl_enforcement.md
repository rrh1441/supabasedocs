Platform

# Postgres SSL Enforcement

* * *

Your Supabase project supports connecting to the Postgres DB without SSL enabled to maximize client compatibility. For increased security, you can prevent clients from connecting if they're not using SSL.

Disabling SSL enforcement only applies to connections to Postgres and Supavisor ("Connection Pooler"); all HTTP APIs offered by Supabase (e.g., PostgREST, Storage, Auth) automatically enforce SSL on all incoming connections.

Projects need to be at least on Postgres 13.3.0 to enable SSL enforcement. You can find the Postgres version of your project in the [Infrastructure Settings page](https://supabase.com/dashboard/project/_/settings/infrastructure). If your project is on an older version, you will need to
[upgrade](https://supabase.com/docs/guides/platform/migrating-and-upgrading-projects#upgrade-your-project) to use this feature.

## Manage SSL enforcement via the dashboard [\#](https://supabase.com/docs/guides/platform/ssl-enforcement\#manage-ssl-enforcement-via-the-dashboard)

SSL enforcement can be configured via the "Enforce SSL on incoming connections" setting under the SSL Configuration section in [Database Settings page](https://supabase.com/dashboard/project/_/settings/database) of the dashboard.

## Manage SSL enforcement via the CLI [\#](https://supabase.com/docs/guides/platform/ssl-enforcement\#manage-ssl-enforcement-via-the-cli)

To get started:

1. [Install](https://supabase.com/docs/guides/cli) the Supabase CLI 1.37.0+.
2. [Log in](https://supabase.com/docs/guides/getting-started/local-development#log-in-to-the-supabase-cli) to your Supabase account using the CLI.
3. Ensure that you have [Owner or Admin permissions](https://supabase.com/docs/guides/platform/access-control#manage-team-members) for the project that you are enabling SSL enforcement.

### Check enforcement status [\#](https://supabase.com/docs/guides/platform/ssl-enforcement\#check-enforcement-status)

You can use the `get` subcommand of the CLI to check whether SSL is currently being enforced:

`
supabase ssl-enforcement --project-ref {ref} get --experimental
`

Response if SSL is being enforced:

`
SSL is being enforced.
`

Response if SSL is not being enforced:

`
SSL is *NOT* being enforced.
`

### Update enforcement [\#](https://supabase.com/docs/guides/platform/ssl-enforcement\#update-enforcement)

The `update` subcommand is used to change the SSL enforcement status for your project:

`
supabase ssl-enforcement --project-ref {ref} update --enable-db-ssl-enforcement --experimental
`

Similarly, to disable SSL enforcement:

`
supabase ssl-enforcement --project-ref {ref} update --disable-db-ssl-enforcement --experimental
`

### A note about Postgres SSL modes [\#](https://supabase.com/docs/guides/platform/ssl-enforcement\#a-note-about-postgres-ssl-modes)

Postgres supports [multiple SSL modes](https://www.postgresql.org/docs/current/libpq-ssl.html#LIBPQ-SSL-PROTECTION) on the client side. These modes provide different levels of protection. Depending on your needs, it is important to verify that the SSL mode in use is performing the required level of enforcement and verification of SSL connections.

The strongest mode offered by Postgres is `verify-full` and this is the mode you most likely want to use when SSL enforcement is enabled. To use `verify-full` you will need to download the Supabase CA certificate for your database. The certificate is available through the dashboard under the SSL Configuration section in the [Database Settings page](https://supabase.com/dashboard/project/_/settings/database).

Once the CA certificate has been downloaded, add it to the certificate authority list used by Postgres.

`
cat {location of downloaded prod-ca-2021.crt} >> ~/.postgres/root.crt
`

With the CA certificate added to the trusted certificate authorities list, use `psql` or your client library to connect to Supabase:

`
psql "postgresql://aws-0-eu-central-1.pooler.supabase.com:6543/postgres?sslmode=verify-full" -U postgres.<user>
`

### Is this helpful?

NoYes

### On this page

[Manage SSL enforcement via the dashboard](https://supabase.com/docs/guides/platform/ssl-enforcement#manage-ssl-enforcement-via-the-dashboard) [Manage SSL enforcement via the CLI](https://supabase.com/docs/guides/platform/ssl-enforcement#manage-ssl-enforcement-via-the-cli) [Check enforcement status](https://supabase.com/docs/guides/platform/ssl-enforcement#check-enforcement-status) [Update enforcement](https://supabase.com/docs/guides/platform/ssl-enforcement#update-enforcement) [A note about Postgres SSL modes](https://supabase.com/docs/guides/platform/ssl-enforcement#a-note-about-postgres-ssl-modes)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings