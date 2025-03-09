# Error: prepared statement "XXX" already exists

Last edited: 1/17/2025

* * *

This error occurs when you are trying to connect to the database using PgBouncer. PgBouncer does not support prepared statements. If you have prepared statements in use, you will need to use direct connections - port 5432.

There is a special parameter in the query string for Prisma to work with PgBouncer
[https://www.prisma.io/docs/guides/performance-and-optimization/connection-management/configure-pg-bouncer](https://www.prisma.io/docs/guides/performance-and-optimization/connection-management/configure-pg-bouncer)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings