Platform

# Permissions

* * *

The Supabase platform offers additional services (e.g. Storage) on top of the Postgres database that comes with each project. These services default to storing their operational data within your database, to ensure that you retain complete control over it.

However, these services assume a base level of access to their data, in order to e.g. be able to run migrations over it. Breaking these assumptions runs the risk of rendering these services inoperational for your project:

- all entities under the `storage` schema are owned by `supabase_storage_admin`
- all entities under the `auth` schema are owned by `supabase_auth_admin`

It is possible for violations of these assumptions to not cause an immediate outage, but take effect at a later time when a newer migration becomes available.

### Is this helpful?

NoYes

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings