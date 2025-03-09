Database

# Roles, superuser access and unsupported operations

* * *

Supabase provides the default `postgres` role to all instances deployed. Superuser access is not given as it allows destructive operations to be performed on the database.

To ensure you are not impacted by this, additional privileges are granted to the `postgres` user to allow it to run some operations that are normally restricted to superusers.

However, this does mean that some operations, that typically require `superuser` privileges, are not available on Supabase. These are documented below:

## Unsupported operations [\#](https://supabase.com/docs/guides/database/postgres/roles-superuser\#unsupported-operations)

- `CREATE SUBSCRIPTION`
- `CREATE EVENT TRIGGER`
- `COPY ... FROM PROGRAM`
- `ALTER USER ... WITH SUPERUSER`

### Is this helpful?

NoYes

### On this page

[Unsupported operations](https://supabase.com/docs/guides/database/postgres/roles-superuser#unsupported-operations)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings