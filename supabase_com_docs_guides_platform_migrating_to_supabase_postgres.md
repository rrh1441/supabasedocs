Platform

# Migrate from Postgres to Supabase

## Migrate your existing Postgres database to Supabase.

* * *

This is a guide for migrating your Postgres database to [Supabase](https://supabase.com/).
Supabase is a robust and open-source platform. Supabase provide all the backend features developers need to build a product: a Postgres database, authentication, instant APIs, edge functions, realtime subscriptions, and storage. Postgres is the core of Supabase—for example, you can use row-level security and there are more than 40 Postgres extensions available.

This guide demonstrates how to migrate your Postgres database to Supabase to get the most out of Postgres while gaining access to all the features you need to build a project.

## Retrieve your Postgres database credentials [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/postgres\#retrieve-credentials)

1. Log in to your provider to get the connection details for your Postgres database.
2. Click on **PSQL Command** and edit it adding the content after `PSQL_COMMAND=`.

Example:

`
%env PSQL_COMMAND=PGPASSWORD=RgaMDfTS_password_FTPa7 psql -h dpg-a_server_in.oregon-postgres.provider.com -U my_db_pxl0_user my_db_pxl0
`

## Retrieve your Supabase connection string [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/postgres\#retrieve-supabase-connection-string)

1. If you're new to Supabase, [create a project](https://supabase.com/dashboard).
Make a note of your password, you will need this later. If you forget it, you can [reset it here](https://supabase.com/dashboard/project/_/settings/database).

2. Go to the [Database settings](https://supabase.com/dashboard/project/_/settings/database) for your project in the Supabase Dashboard.

3. Under **Connection string**, make sure `Use connection pooling` is enabled. Copy the URI and replace the password placeholder with your database password.


![Finding Supabase host address](https://supabase.com/docs/img/guides/resources/migrating-to-supabase/postgres/database-settings-host.png)

## Migrate the database [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/postgres\#migrate-the-database)

The fastest way to migrate your database is with the Supabase migration tool on [Google Colab](https://colab.research.google.com/github/mansueli/Supa-Migrate/blob/main/Migrate_Postgres_Supabase.ipynb). Alternatively, you can use the [pg\_dump](https://www.postgresql.org/docs/current/app-pgdump.html) and [psql](https://www.postgresql.org/docs/current/app-psql.html) command line tools, which are included in a full Postgres installation.

Migrate using ColabMigrate using CLI tools

1. Set the environment variables ( `PSQL_COMMAND`, `SUPABASE_HOST`, `SUPABASE_PASSWORD`) in the Colab notebook.
2. Run the first two steps in [the notebook](https://colab.research.google.com/github/mansueli/Supa-Migrate/blob/main/Migrate_Postgres_Supabase.ipynb) in order. The first sets the variables and the second installs PSQL and the migration script.
3. Run the third step to start the migration. This will take a few minutes.

- If you're planning to migrate a database larger than 6 GB, we recommend [upgrading to at least a Large compute add-on](https://supabase.com/docs/guides/platform/compute-add-ons). This will ensure you have the necessary resources to handle the migration efficiently.

- For databases smaller than 150 GB, you can increase the size of the disk on paid projects by navigating to [Database Settings](https://supabase.com/dashboard/project/_/settings/database).

- If you're dealing with a database larger than 150 GB, we strongly advise you to [contact our support team](https://supabase.com/dashboard/support/new) for assistance in provisioning the required resources and ensuring a smooth migration process.


## Enterprise [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/postgres\#enterprise)

[Contact us](https://forms.supabase.com/enterprise) if you need more help migrating your project.

### Is this helpful?

NoYes

### On this page

[Retrieve your Postgres database credentials](https://supabase.com/docs/guides/platform/migrating-to-supabase/postgres#retrieve-credentials) [Retrieve your Supabase connection string](https://supabase.com/docs/guides/platform/migrating-to-supabase/postgres#retrieve-supabase-connection-string) [Migrate the database](https://supabase.com/docs/guides/platform/migrating-to-supabase/postgres#migrate-the-database) [Enterprise](https://supabase.com/docs/guides/platform/migrating-to-supabase/postgres#enterprise)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings