# Do I need to expose "security definer" Functions in Row Level Security Policies?

Last edited: 3/7/2025

* * *

PostgREST supports 2 config parameters:

- Exposed Schemas
- Extra Search Path

![image](https://supabase.com/docs/img/troubleshooting/d756aeb0-515f-425d-b737-75a935935b73.png)

You do not need to add your "security definer" Functions to either of these if you are using them in your Policies.

PostgREST doesn’t need to know about this function on extra search path or exposed schemas, as long as you explicitly use the schema inside RLS (e.g.: `security.rls_func`).

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings