Platform

# Migrate from Heroku to Supabase

## Migrate your Heroku Postgres database to Supabase.

* * *

Supabase is one of the best [free alternatives to Heroku Postgres](https://supabase.com/alternatives/supabase-vs-heroku-postgres). This guide shows how to migrate your Heroku Postgres database to Supabase. This migration requires the [pg\_dump](https://www.postgresql.org/docs/current/app-pgdump.html) and [psql](https://www.postgresql.org/docs/current/app-psql.html) CLI tools, which are installed automatically as part of the complete Postgres installation package.

Alternatively, use the [Heroku to Supabase migration tool](https://migrate.supabase.com/) to migrate in just a few clicks.

## Quick demo [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku\#quick-demo)

Migrate Your Heroku Database to Supabase - YouTube

Supabase

45.5K subscribers

[Migrate Your Heroku Database to Supabase](https://www.youtube.com/watch?v=xsRhPMphtZ4)

Supabase

Search

Info

Shopping

Tap to unmute

If playback doesn't begin shortly, try restarting your device.

You're signed out

Videos you watch may be added to the TV's watch history and influence TV recommendations. To avoid this, cancel and sign in to YouTube on your computer.

CancelConfirm

Share

Include playlist

An error occurred while retrieving sharing information. Please try again later.

Watch later

Share

Copy link

Watch on

0:00

/ •Live

•

[Watch on YouTube](https://www.youtube.com/watch?v=xsRhPMphtZ4 "Watch on YouTube")

## Retrieve your Heroku database credentials [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku\#retrieve-heroku-credentials)

1. Log in to your [Heroku account](https://heroku.com/) and select the project you want to migrate.
2. Click **Resources** in the menu and select your **Heroku Postgres** database.
3. Click **Settings** in the menu.
4. Click **View Credentials** and save the following information:
   - Host ( `$HEROKU_HOST`)
   - Database ( `$HEROKU_DATABASE`)
   - User ( `$HEROKU_USER`)
   - Password ( `$HEROKU_PASSWORD`)

## Retrieve your Supabase connection string [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku\#retrieve-supabase-connection-string)

1. If you're new to Supabase, [create a project](https://supabase.com/dashboard).
2. Go to the [Database settings](https://supabase.com/dashboard/project/_/settings/database) for your project in the Supabase Dashboard.
3. Under **Connection string**, make sure `Use connection pooling` is enabled. Copy the URI and replace the password placeholder with your database password.

## Export your Heroku database to a file [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku\#export-heroku-database)

Use `pg_dump` with your Heroku credentials to export your Heroku database to a file (e.g., `heroku_dump.sql`).

`
pg_dump --clean --if-exists --quote-all-identifiers \
 -h $HEROKU_HOST -U $HEROKU_USER -d $HEROKU_DATABASE \
 --no-owner --no-privileges > heroku_dump.sql
`

## Import the database to your Supabase project [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku\#import-database-to-supabase)

Use `psql` to import the Heroku database file to your Supabase project.

`
psql -d "$YOUR_CONNECTION_STRING" -f heroku_dump.sql
`

## Additional options [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku\#additional-options)

- To only migrate a single database schema, add the `--schema=PATTERN` parameter to your `pg_dump` command.
- To exclude a schema: `--exclude-schema=PATTERN`.
- To only migrate a single table: `--table=PATTERN`.
- To exclude a table: `--exclude-table=PATTERN`.

Run `pg_dump --help` for a full list of options.

- If you're planning to migrate a database larger than 6 GB, we recommend [upgrading to at least a Large compute add-on](https://supabase.com/docs/guides/platform/compute-add-ons). This will ensure you have the necessary resources to handle the migration efficiently.

- For databases smaller than 150 GB, you can increase the size of the disk on paid projects by navigating to [Database Settings](https://supabase.com/dashboard/project/_/settings/database).

- If you're dealing with a database larger than 150 GB, we strongly advise you to [contact our support team](https://supabase.com/dashboard/support/new) for assistance in provisioning the required resources and ensuring a smooth migration process.


## Enterprise [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku\#enterprise)

[Contact us](https://forms.supabase.com/enterprise) if you need more help migrating your project.

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2FxsRhPMphtZ4%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Quick demo](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku#quick-demo) [Retrieve your Heroku database credentials](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku#retrieve-heroku-credentials) [Retrieve your Supabase connection string](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku#retrieve-supabase-connection-string) [Export your Heroku database to a file](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku#export-heroku-database) [Import the database to your Supabase project](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku#import-database-to-supabase) [Additional options](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku#additional-options) [Enterprise](https://supabase.com/docs/guides/platform/migrating-to-supabase/heroku#enterprise)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings