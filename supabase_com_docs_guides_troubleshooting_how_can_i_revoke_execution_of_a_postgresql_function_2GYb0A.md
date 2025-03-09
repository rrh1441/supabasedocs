# How can I revoke execution of a PostgreSQL function?

Last edited: 1/17/2025

* * *

All functions access is PUBLIC by default, this means that any role can execute it. To revoke execution, there are 2 steps required:

- Revoke function execution ( `foo` in this case) from PUBLIC:

`
revoke execute on function foo from public;
`

- Revoke execution from a particular role ( `anon` in this case):

`
revoke execute on function foo from anon;
`

Now `anon` should get an error when trying to execute the function:

`
begin;
set local role anon;
select foo();
ERROR:  permission denied for function foo
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings