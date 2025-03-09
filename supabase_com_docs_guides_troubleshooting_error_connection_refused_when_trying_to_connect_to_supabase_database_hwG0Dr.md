# Error: "Connection refused" when trying to connect to Supabase database

Last edited: 1/18/2025

* * *

If you're not able to connect to the Supabase database and see the error `connect ECONNREFUSED 1.2.3.4:5432` or `psql: error: connection to server at "db.xxxxxxxxxxxxxxxxxxxx.supabase.co" (1.2.3.4), port 5432 failed: Connection refused Is the server running on that host and accepting TCP/IP connections?`, this could be because there are banned IPs on your project caused by Fail2ban as it kicks in when attempting 2 wrong passwords in a row.

These bans will clear after 30mins but you can unban the IPs using the Supabase CLI [https://supabase.com/docs/guides/cli](https://supabase.com/docs/guides/cli) following the commands below.

How to list the banned IPs:

`
% supabase network-bans get --project-ref <project_reference_id> --experimental
`

How to unban the IPs:

`
% supabase network-bans remove --db-unban-ip <ip_address> --project-ref <project_reference_id> --experimental
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings