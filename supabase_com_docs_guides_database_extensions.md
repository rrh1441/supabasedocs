Database

# Postgres Extensions Overview

* * *

Extensions are exactly as they sound - they "extend" the database with functionality which isn't part of the Postgres core.
Supabase has pre-installed some of the most useful open source extensions.

### Enable and disable extensions [\#](https://supabase.com/docs/guides/database/extensions\#enable-and-disable-extensions)

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click **Extensions** in the sidebar.
3. Enable or disable an extension.

Most extensions are installed under the `extensions` schema, which is accessible to public by default. To avoid namespace pollution, we do not recommend creating other entities in the `extensions` schema.

If you need to restrict user access to tables managed by extensions, we recommend creating a separate schema for installing that specific extension.

Some extensions can only be created under a specific schema, for example, `postgis_tiger_geocoder` extension creates a schema named `tiger`. Before enabling such extensions, make sure you have not created a conflicting schema with the same name.

In addition to the pre-configured extensions, you can also install your own SQL extensions directly in the database using Supabase's SQL editor. The SQL code for the extensions, including plpgsql extensions, can be added through the SQL editor.

### Upgrade extensions [\#](https://supabase.com/docs/guides/database/extensions\#upgrade-extensions)

If a new version of an extension becomes available on Supabase, you need to initiate a software upgrade in the [Infrastructure Settings](https://supabase.com/dashboard/project/_/settings/infrastructure) to access it. Software upgrades can also be initiated by restarting your server in the [General Settings](https://supabase.com/dashboard/project/_/settings/general).

### Full list of extensions [\#](https://supabase.com/docs/guides/database/extensions\#full-list-of-extensions)

Supabase is pre-configured with over 50 extensions. You can also install your own SQL extensions directly in the database through our SQL editor.

Search extensions

### Filter

- AI
- Admin
- Audit
- Cryptography
- Data Type
- Dataset
- Geo
- Index
- Language
- Notifications
- Search
- Testing
- Time Series
- Utility

Reset

[address\_standardizer\\
\\
Used to parse an address into constituent elements. Generally used to support geocoding address normalization step.](https://postgis.net/docs/manual-2.5/Address_Standardizer.html) [address\_standardizer\_data\_us\\
\\
Address Standardizer US dataset example](https://postgis.net/docs/manual-2.5/Address_Standardizer.html) [amcheck\\
\\
Functions for verifying relation integrity](https://www.postgresql.org/docs/current/amcheck.html) [autoinc\\
\\
Functions for autoincrementing fields](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.50.6) [bloom\\
\\
Bloom access method - signature file based index](https://www.postgresql.org/docs/current/bloom.html) [btree\_gin\\
\\
Support for indexing common datatypes in GIN](https://www.postgresql.org/docs/current/btree-gin.html) [btree\_gist\\
\\
Support for indexing common datatypes in GiST](https://www.postgresql.org/docs/current/btree-gist.html) [citext\\
\\
Data type for case-insensitive character strings](https://www.postgresql.org/docs/current/citext.html) [cube\\
\\
Data type for multidimensional cubes](https://www.postgresql.org/docs/current/cube.html) [dblink\\
\\
Connect to other PostgreSQL databases from within a database](https://www.postgresql.org/docs/current/contrib-dblink-function.html) [dict\_int\\
\\
Text search dictionary template for integers](https://www.postgresql.org/docs/current/dict-int.html) [dict\_xsyn\\
\\
Text search dictionary template for extended synonym processing](https://www.postgresql.org/docs/current/dict-xsyn.html) [earthdistance\\
\\
Calculate great-circle distances on the surface of the Earth](https://www.postgresql.org/docs/current/earthdistance.html) [fuzzystrmatch\\
\\
Determine similarities and distance between strings](https://www.postgresql.org/docs/current/fuzzystrmatch.html) [hstore\\
\\
Data type for storing sets of (key, value) pairs](https://www.postgresql.org/docs/current/hstore.html) [hypopg\\
\\
Hypothetical indexes for PostgreSQL](https://supabase.com/docs/guides/database/extensions/hypopg) [http\\
\\
HTTP client for PostgreSQL, allows web page retrieval inside the database.](https://supabase.com/docs/guides/database/extensions/http) [insert\_username\\
\\
Functions for tracking who changed a table](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.50.7) [old\_snapshot\\
\\
Utilities in support of old\_snapshot\_threshold](https://www.postgresql.org/docs/current/oldsnapshot.html) [index\_advisor\\
\\
Optimize query performance with automatic index recommendation](https://supabase.com/docs/guides/database/extensions/index_advisor) [intarray\\
\\
Functions, operators, and index support for 1-D arrays of integers](https://www.postgresql.org/docs/current/intarray.html) [isn\\
\\
Data types for international product numbering standards](https://www.postgresql.org/docs/current/isn.html) [lo\\
\\
Large Object maintenance](https://www.postgresql.org/docs/current/lo.html) [ltree\\
\\
Data type for hierarchical tree-like structures](https://www.postgresql.org/docs/current/ltree.html) [moddatetime\\
\\
Functions for tracking last modification time](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.50.8) [pg\_cron\\
\\
Job scheduler for PostgreSQL](https://supabase.com/docs/guides/database/extensions/pg_cron) [pg\_freespacemap\\
\\
Examine the free space map (FSM)](https://www.postgresql.org/docs/current/pgfreespacemap.html) [pg\_graphql\\
\\
Pg\_graphql: GraphQL support](https://supabase.com/docs/guides/database/extensions/pg_graphql) [pg\_hashids\\
\\
Pg\_hashids](https://supabase.com/docs/guides/database/extensions/pg_hashids) [pg\_jsonschema\\
\\
Pg\_jsonschema](https://supabase.com/docs/guides/database/extensions/pg_jsonschema) [pg\_net\\
\\
Async HTTP](https://supabase.com/docs/guides/database/extensions/pg_net) [pg\_prewarm\\
\\
Prewarm relation data](https://www.postgresql.org/docs/current/pgprewarm.html) [pg\_stat\_statements\\
\\
Track execution statistics of all SQL statements executed](https://supabase.com/docs/guides/database/extensions/pg_stat_statements) [pg\_surgery\\
\\
Extension to perform surgery on a damaged relation](https://www.postgresql.org/docs/current/pgsurgery.html) [pg\_trgm\\
\\
Text similarity measurement and index searching based on trigrams](https://www.postgresql.org/docs/current/pgtrgm.html) [pgaudit\\
\\
Provides auditing functionality](https://supabase.com/docs/guides/database/extensions/pgaudit) [pg\_walinspect\\
\\
Functions to inspect contents of PostgreSQL Write-Ahead Log](https://www.postgresql.org/docs/current/pgwalinspect.html) [pgcrypto\\
\\
Cryptographic functions](https://www.postgresql.org/docs/current/pgcrypto.html) [pgjwt\\
\\
JSON Web Token API for Postgresql](https://supabase.com/docs/guides/database/extensions/pgjwt) [pgroonga\\
\\
Super fast and all languages supported full text search index based on Groonga](https://supabase.com/docs/guides/database/extensions/pgroonga) [pgroonga\_database\\
\\
PGroonga database management module](https://pgroonga.github.io/reference/modules/pgroonga-database.html) [pgrouting\\
\\
PgRouting Extension](https://supabase.com/docs/guides/database/extensions/pgrouting) [pgrowlocks\\
\\
Show row-level locking information](https://www.postgresql.org/docs/current/pgrowlocks.html) [pgsodium\\
\\
Postgres extension for libsodium functions](https://supabase.com/docs/guides/database/extensions/pgsodium) [pgstattuple\\
\\
Show tuple-level statistics](https://www.postgresql.org/docs/current/pgstattuple.html) [pgtap\\
\\
Unit testing for PostgreSQL](https://supabase.com/docs/guides/database/extensions/pgtap) [plcoffee\\
\\
PL/CoffeeScript (v8) trusted procedural language](https://github.com/plv8/plv8/blob/master/doc/plv8.md#coffeescript-extension) [pljava\\
\\
PL/Java procedural language (https://tada.github.io/pljava/)](https://tada.github.io/pljava/) [plls\\
\\
PL/LiveScript (v8) trusted procedural language](https://github.com/plv8/plv8/blob/master/doc/plv8.md#livescript-extension) [plpgsql\\
\\
PL/pgSQL procedural language](https://www.postgresql.org/docs/current/plpgsql.html) [plpgsql\_check\\
\\
Extended check for plpgsql functions](https://supabase.com/docs/guides/database/extensions/plpgsql_check) [plv8\\
\\
PL/JavaScript (v8) trusted procedural language](https://supabase.com/docs/guides/database/extensions/plv8) [postgis\\
\\
PostGIS geometry and geography spatial types and functions](https://supabase.com/docs/guides/database/extensions/postgis) [postgres\_fdw\\
\\
Foreign-data wrapper for remote PostgreSQL servers](https://supabase.com/docs/guides/database/extensions/postgres_fdw) [refint\\
\\
Functions for implementing referential integrity (obsolete)](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.50.5) [rum\\
\\
GIN-like index for text search](https://supabase.com/docs/guides/database/extensions/rum) [seg\\
\\
Data type for representing line segments or floating-point intervals](https://www.postgresql.org/docs/current/seg.html) [sslinfo\\
\\
Information about SSL certificates](https://www.postgresql.org/docs/current/sslinfo.html) [tablefunc\\
\\
Functions that manipulate whole tables, including crosstab](https://www.postgresql.org/docs/current/tablefunc.html) [tcn\\
\\
Triggered change notifications](https://www.postgresql.org/docs/current/tcn.html) [timescaledb\\
\\
Enables scalable inserts and complex queries for time-series data](https://supabase.com/docs/guides/database/extensions/timescaledb) [tsm\_system\_rows\\
\\
TABLESAMPLE method which accepts number of rows as a limit](https://www.postgresql.org/docs/current/tsm-system-rows.html) [tsm\_system\_time\\
\\
TABLESAMPLE method which accepts time in milliseconds as a limit](https://www.postgresql.org/docs/current/tsm-system-time.html) [unaccent\\
\\
Text search dictionary that removes accents](https://www.postgresql.org/docs/current/unaccent.html) [uuid-ossp\\
\\
Generate universally unique identifiers (UUIDs)](https://supabase.com/docs/guides/database/extensions/uuid-ossp) [vector\\
\\
Vector data type with similarity search](https://supabase.com/docs/guides/database/extensions/pgvector) [pg\_repack\\
\\
Optimize physical storage and remove bloat from tables and indexes](https://supabase.com/docs/guides/database/extensions/pg_repack) [wrappers\\
\\
Foreign data wrappers developed by Supabase](https://supabase.com/docs/guides/database/extensions/wrappers/overview)

### Is this helpful?

NoYes

### On this page

[Enable and disable extensions](https://supabase.com/docs/guides/database/extensions#enable-and-disable-extensions) [Upgrade extensions](https://supabase.com/docs/guides/database/extensions#upgrade-extensions) [Full list of extensions](https://supabase.com/docs/guides/database/extensions#full-list-of-extensions)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings