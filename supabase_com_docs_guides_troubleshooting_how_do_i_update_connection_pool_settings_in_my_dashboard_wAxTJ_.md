# How do I update connection pool settings in my dashboard?

Last edited: 2/21/2025

* * *

Common questions about updating settings for PgBouncer or Supavisor:

- How to tell which connection pooler you're using:
The PgBouncer connection string looks like: `postgres://postgres:[YOUR-PASSWORD]@db.xxxxxxxxxx.supabase.co:6543/postgres`

The Supavisor connection string looks like: `postgres://postgres.xxxxxxxxx:[YOUR-PASSWORD]@aws-0-us-west-1.pooler.supabase.com:6543/postgres`

The subdomain will vary depending on the region a project is deployed in. The project reference is to be included in the username following a `.`. If the username is `postgres` the username you use for Supavisor is `postgres.[PROJECT_REF]`.

- How to update the size of the connection pool to the database:
You can set the `Max Client Connections` field in your database settings here:

[https://supabase.com/dashboard/project/\_/settings/database](https://supabase.com/dashboard/project/_/settings/database)

- How to change the client connection limit:
You can set the `Default Pool Size` field in your database settings:

[https://supabase.com/dashboard/project/\_/settings/database](https://supabase.com/dashboard/project/_/settings/database)

- How to use `session` mode:
With Supavisor you can automatically use `session` mode by using the connection string with port `5432` in it.

You can also set the pooler port 6543 to use `session` mode in the database settings:

[https://supabase.com/dashboard/project/\_/settings/database](https://supabase.com/dashboard/project/_/settings/database)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings