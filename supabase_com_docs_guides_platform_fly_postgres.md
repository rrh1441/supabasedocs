Platform

# Fly Postgres

* * *

Fly Postgres databases are deployed on the Fly.io edge network. Fly Postgres is supported in [every region](https://fly.io/docs/reference/regions/) where Fly.io operates.

Fly Postgres is being deprecated on March 14, 2025. Head over to the [announcement](https://github.com/orgs/supabase/discussions/33413) for more information.

## Quickstart [\#](https://supabase.com/docs/guides/platform/fly-postgres\#quickstart)

1. Authenticate via the CLI: `flyctl auth login`
2. Access the Supabase dashboard by running `flyctl extensions supabase dashboard <app-name>`

The full list of CLI commands for Fly Postgres are documented in the [Fly docs](https://fly.io/docs/flyctl/extensions-supabase/).

## Connecting to your database [\#](https://supabase.com/docs/guides/platform/fly-postgres\#connecting-to-your-database)

If your network supports IPv6, connect directly to your Fly Postgres database, as its domain name resolves to an IPv6 address which can be directly used from within your Fly applications.

If your network only supports IPv4, use Supavisor instead of connecting to the database directly. Supavisor's domain name resolves to an IPv4 address, allowing networks without IPv6 support to connect.

To find your database's connection strings, visit the Supabase [database settings page](https://supabase.com/dashboard/project/_/settings/database).

## Studio support [\#](https://supabase.com/docs/guides/platform/fly-postgres\#studio-support)

Access the Supabase studio by running the following command: `flyctl extensions supabase dashboard <app-name>`. This command authenticates with Fly via OAuth and then logs you into the Supabase dashboard.

In the dashboard, you gain access to several powerful tools, including:

- SQL editor: Run SQL queries against your database.
- Table editor: Create, edit and delete tables and columns.
- Log explorer: Inspect real-time logs for your database.
- Postgres upgrades: Upgrade your Fly Postgres instance to the latest version.

## Permissions [\#](https://supabase.com/docs/guides/platform/fly-postgres\#permissions)

Supabase and Fly organizations have a direct one-to-one relationship. When you launch your first Fly Postgres database, it triggers the automatic creation of a corresponding Supabase organization if one does not already exist.

User accounts on Supabase are also created on demand. Every Fly user gets a unique Supabase account, and this account is separate from any Supabase accounts you might already have.

Upon launching a Fly Postgres database, the initiating user is granted the owner role within the new Supabase organization. All subsequent users are added with the `developer` role. Roles can be adjusted in the Supabase dashboard as required.

## Limitations [\#](https://supabase.com/docs/guides/platform/fly-postgres\#limitations)

When using Fly Postgres, be aware of the current restrictions:

- Direct database connections are only supported via IPv6. Read more in the [connecting to your database](https://supabase.com/docs/guides/platform/fly-postgres#connecting-to-your-database) section.
- [Network restrictions](https://supabase.com/docs/guides/platform/network-restrictions) are not supported
- Currently, only the database, Supavisor, and PostgREST are supported. Support for additional Supabase products such as Realtime, Storage, and Auth is planned.
- The [pg\_cron](https://supabase.com/docs/guides/database/extensions/pg_cron) extension is not fully supported for Fly projects. Fly projects shut down after 15 minutes of inactivity, but transparently start up when an external request is received. However, this does not apply to pg\_cron jobs, which aren’t triggered by external requests. ` pg_cron` jobs don’t run when the database is shut down due to inactivity.

### Is this helpful?

NoYes

### On this page

[Quickstart](https://supabase.com/docs/guides/platform/fly-postgres#quickstart) [Connecting to your database](https://supabase.com/docs/guides/platform/fly-postgres#connecting-to-your-database) [Studio support](https://supabase.com/docs/guides/platform/fly-postgres#studio-support) [Permissions](https://supabase.com/docs/guides/platform/fly-postgres#permissions) [Limitations](https://supabase.com/docs/guides/platform/fly-postgres#limitations)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings