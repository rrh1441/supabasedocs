# An "invalid response was received from the upstream server" error when querying auth

Last edited: 2/21/2025

* * *

If you are observing an "invalid response was received from the upstream server" error when making requests to Supabase Auth, it could mean that the respective service is down. One way to confirm this is to visit the [logs explorer](https://supabase.com/dashboard/project/_/logs/explorer) and look at the auth logs to see if there are any errors with the following lines:

- `running db migrations: error executing migrations/20221208132122_backfill_email_last_sign_in_at.up.sql`

We're currently investigating an issue where the tables responsible for keeping track of migrations ran by Auth ( `auth.schema_migrations`) are not being restored properly, which leads to the service(s) retrying those migrations. In such cases, migrations which are not idempotent will run into issues.

We've documented some of the migrations that run into this issue and their corresponding fix here:

### Auth: `operator does not exist: uuid = text` [\#](https://supabase.com/docs/guides/troubleshooting/an-invalid-response-was-received-from-the-upstream-server-error-when-querying-auth-RI4Vl-\#auth-operator-does-not-exist-uuid--text)

Temporary fix: Run `insert into auth.schema_migrations values ('20221208132122');` via the [SQL editor](https://supabase.com/dashboard/project/_/sql/new) to fix the issue.

If the migration error you're seeing looks different, reach out to [supabase.help](https://supabase.help/) for assistance.

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings