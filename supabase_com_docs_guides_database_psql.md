Database

# Connecting with PSQL

* * *

[`psql`](https://www.postgresql.org/docs/current/app-psql.html) is a command-line tool that comes with Postgres.

## Connecting with SSL [\#](https://supabase.com/docs/guides/database/psql\#connecting-with-ssl)

You should connect to your database using SSL wherever possible, to prevent snooping and man-in-the-middle attacks.

You can obtain your connection info and Server root certificate from your application's dashboard:

![Connection Info and Certificate.](https://supabase.com/docs/img/database/database-settings-ssl.png)

Download your [SSL certificate](https://supabase.com/docs/guides/database/psql#connecting-with-ssl) to `/path/to/prod-supabase.cer`.

Find your connection settings. Go to your [`Database Settings`](https://supabase.com/dashboard/project/_/settings/database) and make sure `Use connection pooling` is checked. Change the connection mode to `Session`, and copy the parameters into the connection string:

`
psql "sslmode=verify-full sslrootcert=/path/to/prod-supabase.cer host=[CLOUD_PROVIDER]-0-[REGION].pooler.supabase.com dbname=postgres user=postgres.[PROJECT_REF]"
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings