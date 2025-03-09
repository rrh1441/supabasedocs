Database

# postgres\_fdw

* * *

The extension enables Postgres to query tables and views on a remote Postgres server.

## Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/postgres_fdw\#enable-the-extension)

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Extensions** in the sidebar.
3. Search for "postgres\_fdw" and enable the extension.

## Create a connection to another database [\#](https://supabase.com/docs/guides/database/extensions/postgres_fdw\#create-a-connection-to-another-database)

1

### Create a foreign server

Define the remote database address

`
    create server "<foreign_server_name>"
    foreign data wrapper postgres_fdw
    options (
        host '<host>',
        port '<port>',
        dbname '<dbname>'
    );
`

2

### Create a server mapping

Set the user credentials for the remote server

`
create user mapping for "<dbname>"
server "<foreign_server_name>"
options (
    user '<db_user>',
    password '<password>'
);
`

3

### Import tables

Import tables from the foreign database

Example: Import all tables from a schema

`
import foreign schema "<foreign_schema>"
from server "<foreign_server>"
into "<host_schema>";
`

Example: Import specific tables

`
import foreign schema "<foreign_schema>"
limit to (
    "<table_name1>",
    "<table_name2>"
)
from server "<foreign_server>"
into "<host_schema>";
`

4

### Query foreign table

`
select * from "<foreign_table>"
`

### Configuring execution options [\#](https://supabase.com/docs/guides/database/extensions/postgres_fdw\#configuring-execution-options)

#### Fetch\_size [\#](https://supabase.com/docs/guides/database/extensions/postgres_fdw\#fetchsize)

Maximum rows fetched per operation. For example, fetching 200 rows with `fetch_size` set to 100 requires 2 requests.

`
alter server "<foreign_server_name>"
options (fetch_size '10000');
`

#### Batch\_size [\#](https://supabase.com/docs/guides/database/extensions/postgres_fdw\#batchsize)

Maximum rows inserted per cycle. For example, inserting 200 rows with `batch_size` set to 100 requires 2 requests.

`
alter server "<foreign_server_name>"
options (batch_size '1000');
`

#### Extensions [\#](https://supabase.com/docs/guides/database/extensions/postgres_fdw\#extensions)

Lists shared extensions. Without them, queries involving unlisted extension functions or operators may fail or omit references.

`
alter server "<foreign_server_name>"
options (extensions 'vector, postgis');
`

For more server options, check the extension's [official documentation](https://www.postgresql.org/docs/current/postgres-fdw.html#POSTGRES-FDW)

## Resources [\#](https://supabase.com/docs/guides/database/extensions/postgres_fdw\#resources)

- Official [`postgres_fdw` documentation](https://www.postgresql.org/docs/current/postgres-fdw.html#POSTGRES-FDW)

### Is this helpful?

NoYes

### On this page

[Enable the extension](https://supabase.com/docs/guides/database/extensions/postgres_fdw#enable-the-extension) [Create a connection to another database](https://supabase.com/docs/guides/database/extensions/postgres_fdw#create-a-connection-to-another-database) [Configuring execution options](https://supabase.com/docs/guides/database/extensions/postgres_fdw#configuring-execution-options) [Resources](https://supabase.com/docs/guides/database/extensions/postgres_fdw#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings