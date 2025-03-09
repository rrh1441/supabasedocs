Database

# pg\_repack: Physical storage optimization and maintenance

* * *

[pg\_repack](https://github.com/reorg/pg_repack) is a Postgres extension to remove bloat from tables and indexes, and optionally restore the physical order of clustered indexes. Unlike CLUSTER and VACUUM FULL, pg\_repack runs "online" and does not hold a exclusive locks on the processed tables that could prevent ongoing database operations. pg\_repack's efficiency is comparable to using CLUSTER directly.

pg\_repack provides the following methods to optimize physical storage:

- Online CLUSTER: ordering table data by cluster index in a non-blocking way
- Ordering table data by specified columns
- Online VACUUM FULL: packing rows only in a non-blocking way
- Rebuild or relocate only the indexes of a table

pg\_repack has 2 components, the database extension and a client-side CLI to control it.

## Requirements [\#](https://supabase.com/docs/guides/database/extensions/pg_repack\#requirements)

- A target table must have a PRIMARY KEY, or a UNIQUE total index on a NOT NULL column.
- Performing a full-table repack requires free disk space about twice as large as the target table and its indexes.

pg\_repack requires the Postgres superuser role by default. That role is not available to users on the Supabase platform. To avoid that requirement, use the `-k` or `--no-superuser-check` flags on every `pg_repack` CLI command.

The first version of pg\_repack with full support for non-superuser repacking is 1.5.2. You can check the version installed on your Supabase instance using

`
select default_version
from pg_available_extensions
where name = 'pg_repack';
`

If pg\_repack is not present, or the version is < 1.5.2, [upgrade to the latest version](https://supabase.com/docs/guides/platform/upgrading) of Supabase to gain access.

## Usage [\#](https://supabase.com/docs/guides/database/extensions/pg_repack\#usage)

### Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/pg_repack\#enable-the-extension)

Get started with pg\_repack by enabling the extension in the Supabase Dashboard.

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Extensions** in the sidebar.
3. Search for "pg\_repack" and enable the extension.

### Install the CLI [\#](https://supabase.com/docs/guides/database/extensions/pg_repack\#install-the-cli)

Select an option from the pg\_repack docs to [install the client CLI](https://reorg.github.io/pg_repack/#download).

### Syntax [\#](https://supabase.com/docs/guides/database/extensions/pg_repack\#syntax)

All pg\_repack commands should include the `-k` flag to skip the client-side superuser check.

`
pg_repack -k [OPTION]... [DBNAME]
`

## Example [\#](https://supabase.com/docs/guides/database/extensions/pg_repack\#example)

Perform an online `VACUUM FULL` on the tables `public.foo` and `public.bar` in the database `postgres`:

`
pg_repack -k -h db.<PROJECT_REF>.supabase.co -p 5432 -U postgres -d postgres --no-order --table public.foo --table public.bar
`

See the [official pg\_repack documentation](https://reorg.github.io/pg_repack/) for the full list of options.

## Limitations [\#](https://supabase.com/docs/guides/database/extensions/pg_repack\#limitations)

- pg\_repack cannot reorganize temporary tables.
- pg\_repack cannot cluster tables by GiST indexes.
- You cannot perform DDL commands of the target tables except VACUUM or ANALYZE while pg\_repack is working.
pg\_repack holds an ACCESS SHARE lock on the target table to enforce this restriction.

## Resources [\#](https://supabase.com/docs/guides/database/extensions/pg_repack\#resources)

- [Official pg\_repack documentation](https://reorg.github.io/pg_repack/)

### Is this helpful?

NoYes

### On this page

[Requirements](https://supabase.com/docs/guides/database/extensions/pg_repack#requirements) [Usage](https://supabase.com/docs/guides/database/extensions/pg_repack#usage) [Enable the extension](https://supabase.com/docs/guides/database/extensions/pg_repack#enable-the-extension) [Install the CLI](https://supabase.com/docs/guides/database/extensions/pg_repack#install-the-cli) [Syntax](https://supabase.com/docs/guides/database/extensions/pg_repack#syntax) [Example](https://supabase.com/docs/guides/database/extensions/pg_repack#example) [Limitations](https://supabase.com/docs/guides/database/extensions/pg_repack#limitations) [Resources](https://supabase.com/docs/guides/database/extensions/pg_repack#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings