Cron

# Install

* * *

Install the Supabase Cron Postgres Module to begin scheduling recurring Jobs.

DashboardSQL

1. Go to the [Cron Postgres Module](https://supabase.com/dashboard/project/_/integrations/cron/overview) under Integrations in the Dashboard.
2. Enable the `pg_cron` extension.

## Uninstall [\#](https://supabase.com/docs/guides/cron/install\#uninstall)

Uninstall Supabase Cron by disabling the `pg_cron` extension:

`
drop extension if exists pg_cron;
`

Disabling the `pg_cron` extension will permanently delete all Jobs.

### Is this helpful?

NoYes

### On this page

[Uninstall](https://supabase.com/docs/guides/cron/install#uninstall)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings