# How to delete a role in Postgres

Last edited: 2/21/2025

* * *

[Quote from Postgres docs:](https://www.postgresql.org/docs/current/sql-droprole.html#:~:text=A%20role%20cannot%20be%20removed,been%20granted%20on%20other%20objects.)

> A role cannot be removed if it is still referenced in any database of the cluster; an error will be raised if so. Before dropping the role, you must drop all the objects it owns (or reassign their ownership) and revoke any privileges the role has been granted on other objects.

First make sure that Postgres has ownership over the role:

`
GRANT <role> TO "postgres";
`

Then you must reassign any objects owned by role:

`
REASSIGN OWNED BY <role> TO postgres;
`

Once ownership is transferred, you can run the following query:

`
DROP OWNED BY <role>;
`

[DROP OWNED BY](https://www.postgresql.org/docs/current/sql-drop-owned.html) does delete all objects owned by the role, which should be none. However, it also revokes the role's privileges. Once this is done, you should be able to run:

`
DROP role <role>;
`

If you encounter any issues, create a [support ticket](https://supabase.com/dashboard/support/new)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings