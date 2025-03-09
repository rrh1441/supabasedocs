# error: no pg\_hba.conf entry for host "xx.xxx.xxx.xxx", user "postgres", database "postgres", SSL off

Last edited: 1/18/2025

* * *

This error indicates a failed authentication attempt to the database and the connection couldn't be established.

In Supabase, this is generally seen when [SSL enforcement](https://supabase.com/docs/guides/platform/ssl-enforcement) is enabled on your Supabase Project. The authentication failed because the incoming connection didn't use SSL encryption when connecting to the database.

You can ignore this message if the attempt is from an unknown user. If you want this connection attempt to be successful, you will either need to connect with SSL or disable SSL enforcement on your Supabase project.

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings