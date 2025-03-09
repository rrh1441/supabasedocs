# Database Error: remaining connection slots are reserved for non-replication superuser connections

Last edited: 1/18/2025

* * *

This error usually occurs when the database reaches the maximum number of connections allowed based on the compute add-on.

To overcome this, the connections need to be optimized as mentioned here: [https://supabase.com/docs/guides/platform/performance#optimizing-the-number-of-connections](https://supabase.com/docs/guides/platform/performance#optimizing-the-number-of-connections)

Additionally, you can try using the connection pool to help solve this issue:
[https://supabase.com/docs/guides/database/connecting-to-postgres#connection-pooler](https://supabase.com/docs/guides/database/connecting-to-postgres#connection-pooler)

If you're already using connection pooling and still hitting the maximum connections, then it is suggested to upgrade your compute add-on that allows more connections: [https://supabase.com/docs/guides/platform/compute-add-ons](https://supabase.com/docs/guides/platform/compute-add-ons)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings