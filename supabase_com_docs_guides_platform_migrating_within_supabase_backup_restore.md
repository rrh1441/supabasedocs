Platform

# Backup and Restore using the CLI

## Learn how to backup and restore projects using the Supabase CLI

* * *

## Backup database using the CLI [\#](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore\#backup-database-using-the-cli)

1

### Install the Supabase CLI

Install the [Supabase CLI](https://supabase.com/docs/guides/local-development/cli/getting-started).

2

### Install Docker Desktop

Install [Docker Desktop](https://www.docker.com/) for your platform.

3

### Get the new database connection string

Go to the [project page](https://supabase.com/dashboard/project/_/) and click the " **Connect**" button at the top of the page for the connection string.

Use the Session pooler connection string by default. If your ISP supports IPv6, use the direct connection string.

Session pooler connection string:

`
postgresql://postgres.[PROJECT-REF]:[YOUR-PASSWORD]@aws-0-us-east-1.pooler.supabase.com:5432/postgres
`

Direct connection string:

`
postgresql://postgres.[PROJECT-REF]:[YOUR-PASSWORD]@db.[PROJECT-REF].supabase.com:5432/postgres
`

4

### Get the database password

Reset the password in the [Database Settings](https://supabase.com/dashboard/project/_/settings/database).

Replace `[YOUR-PASSWORD]` in the connection string with the database password.

5

### Backup database

Run these commands after replacing `[CONNECTION_STRING]` with your connection string from the previous steps:

`
supabase db dump --db-url [CONNECTION_STRING] -f roles.sql --role-only
`

`
supabase db dump --db-url [CONNECTION_STRING] -f schema.sql
`

`
supabase db dump --db-url [CONNECTION_STRING] -f data.sql --use-copy --data-only
`

## Before you begin [\#](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore\#before-you-begin)

Install Postgres and psql

## Restore backup using CLI [\#](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore\#restore-backup-using-cli)

1

### Create project

Create a [new project](https://database.new/)

2

### Configure newly created project

In the new project:

- If Webhooks were used in the old database, enable [Database Webhooks](https://supabase.com/dashboard/project/_/database/hooks).
- If any non-default extensions were used in the old database, enable the [Extensions](https://supabase.com/dashboard/project/_/database/extensions).
- If Replication for Realtime was used in the old database, enable [Publication](https://supabase.com/dashboard/project/_/database/publications) on the tables necessary

3

### Get the new database connection string

Go to the [project page](https://supabase.com/dashboard/project/_/) and click the " **Connect**" button at the top of the page for the connection string.

Use the Session pooler connection string by default. If your ISP supports IPv6, use the direct connection string.

Session pooler connection string:

`
postgresql://postgres.[PROJECT-REF]:[YOUR-PASSWORD]@aws-0-us-east-1.pooler.supabase.com:5432/postgres
`

Direct connection string:

`
postgresql://postgres.[PROJECT-REF]:[YOUR-PASSWORD]@db.[PROJECT-REF].supabase.com:5432/postgres
`

4

### Get the database password

Reset the password in the [Database Settings](https://supabase.com/dashboard/project/_/settings/database).

Replace `[YOUR-PASSWORD]` in the connection string with the database password.

5

### Restore your Project with the CLI

Column encryption disabledColumn encryption enabled

Run these commands after replacing `[CONNECTION_STRING]` with your connection string from the previous steps:

`
psql \
  --single-transaction \
  --variable ON_ERROR_STOP=1 \
  --file roles.sql \
  --file schema.sql \
  --command 'SET session_replication_role = replica' \
  --file data.sql \
  --dbname [CONNECTION_STRING]
`

## Important project restoration notes [\#](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore\#important-project-restoration-notes)

### Troubleshooting notes [\#](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore\#troubleshooting-notes)

- Setting the `session_replication_role` to `replica` disables all triggers so that columns are not double encrypted.
- If you have created any [custom roles](https://supabase.com/dashboard/project/_/database/roles) with `login` attribute, you have to manually set their passwords in the new project.
- If you run into any permission errors related to `supabase_admin` during restore, edit the `schema.sql` file and comment out any lines containing `ALTER ... OWNER TO "supabase_admin"`.

### Preserving migration history [\#](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore\#preserving-migration-history)

If you were using Supabase CLI for managing migrations on your old database and would like to preserve the migration history in your newly restored project, you need to insert the migration records separately using the following commands.

`
supabase db dump --db-url "$OLD_DB_URL" -f history_schema.sql --schema supabase_migrations
supabase db dump --db-url "$OLD_DB_URL" -f history_data.sql --use-copy --data-only --schema supabase_migrations
psql \
  --single-transaction \
  --variable ON_ERROR_STOP=1 \
  --file history_schema.sql \
  --file history_data.sql \
  --dbname "$NEW_DB_URL"
`

### Schema changes to `auth` and `storage` [\#](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore\#schema-changes-to-auth-and-storage)

If you have modified the `auth` and `storage` schemas in your old project, such as adding triggers or Row Level Security(RLS) policies, you have to restore them separately. The Supabase CLI can help you diff the changes to these schemas using the following commands.

`
supabase link --project-ref "$OLD_PROJECT_REF"
supabase db diff --linked --schema auth,storage > changes.sql
`

### Migrate storage objects [\#](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore\#migrate-storage-objects)

The new project has the old project's Storage buckets, but the Storage objects need to be migrated manually. Use this script to move storage objects from one project to another.

``
// npm install @supabase/supabase-js@1
const { createClient } = require('@supabase/supabase-js')
const OLD_PROJECT_URL = 'https://xxx.supabase.co'
const OLD_PROJECT_SERVICE_KEY = 'old-project-service-key-xxx'
const NEW_PROJECT_URL = 'https://yyy.supabase.co'
const NEW_PROJECT_SERVICE_KEY = 'new-project-service-key-yyy'
;(async () => {
const oldSupabaseRestClient = createClient(OLD_PROJECT_URL, OLD_PROJECT_SERVICE_KEY, {
    db: {
      schema: 'storage',
    },
})
const oldSupabaseClient = createClient(OLD_PROJECT_URL, OLD_PROJECT_SERVICE_KEY)
const newSupabaseClient = createClient(NEW_PROJECT_URL, NEW_PROJECT_SERVICE_KEY)
// make sure you update max_rows in postgrest settings if you have a lot of objects
// or paginate here
const { data: oldObjects, error } = await oldSupabaseRestClient.from('objects').select()
if (error) {
    console.log('error getting objects from old bucket')
    throw error
}
for (const objectData of oldObjects) {
    console.log(`moving ${objectData.id}`)
    try {
      const { data, error: downloadObjectError } = await oldSupabaseClient.storage
        .from(objectData.bucket_id)
        .download(objectData.name)
      if (downloadObjectError) {
        throw downloadObjectError
      }
      const { _, error: uploadObjectError } = await newSupabaseClient.storage
        .from(objectData.bucket_id)
        .upload(objectData.name, data, {
          upsert: true,
          contentType: objectData.metadata.mimetype,
          cacheControl: objectData.metadata.cacheControl,
        })
      if (uploadObjectError) {
        throw uploadObjectError
      }
    } catch (err) {
      console.log('error moving ', objectData)
      console.log(err)
    }
}
})()
``

### Is this helpful?

NoYes

### On this page

[Backup database using the CLI](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore#backup-database-using-the-cli) [Before you begin](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore#before-you-begin) [Restore backup using CLI](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore#restore-backup-using-cli) [Important project restoration notes](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore#important-project-restoration-notes) [Troubleshooting notes](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore#troubleshooting-notes) [Preserving migration history](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore#preserving-migration-history) [Schema changes to auth and storage](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore#schema-changes-to-auth-and-storage) [Migrate storage objects](https://supabase.com/docs/guides/platform/migrating-within-supabase/backup-restore#migrate-storage-objects)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings