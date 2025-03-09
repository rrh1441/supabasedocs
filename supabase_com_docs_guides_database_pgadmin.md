Database

# Connecting with pgAdmin

* * *

[`pgAdmin`](https://www.pgadmin.org/) is a GUI tool for managing Postgres databases. You can use it to connect to your database via SSL.

1

### Register

Register a new Postgres server.

![Register a new postgres server.](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fdatabase%2Fconnecting-to-postgres%2Fpgadmin%2Fregister-server-pgAdmin--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

2

### Name

Name your server.

![Name Postgres Server.](https://supabase.com/docs/img/guides/database/connecting-to-postgres/pgadmin/name-pg-server.png)

3

### Connect

Add the connection info. Go to your [`Database Settings`](https://supabase.com/dashboard/project/_/settings/database). Make sure `Use connection pooling` is enabled. Switch the connection mode to `Session` and copy your connection parameters. Fill in your Database password that you made when creating your project (It can be reset in Database Settings above if you don't have it).

![Add Connection Info.](https://supabase.com/docs/img/guides/database/connecting-to-postgres/pgadmin/add-pg-server-conn-info.png)

4

### SSL

Download your SSL certificate from Dashboard's [`Database Settings`](https://supabase.com/dashboard/project/_/settings/database).

In pgAdmin, navigate to the Parameters tab and select connection parameter as Root Certificate. Next navigate to the Root certificate input, it will open up a file-picker modal. Select the certificate you downloaded earlier and save the server details. pgAdmin should now be able to connect to your Postgres via SSL.

![Add Connection Info.](https://supabase.com/docs/img/guides/database/connecting-to-postgres/pgadmin/database-settings-host.png)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings