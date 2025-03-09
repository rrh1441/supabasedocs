Platform

# Performance Tuning

* * *

The Supabase platform automatically optimizes your Postgres database to take advantage of the compute resources of the plan your project is on. However, these optimizations are based on assumptions about the type of workflow the project is being utilized for, and it is likely that better results can be obtained by tuning the database for your particular workflow.

## Examining query performance [\#](https://supabase.com/docs/guides/platform/performance\#examining-query-performance)

Unoptimized queries are a major cause of poor database performance. To analyze the performance of your queries, see the [Debugging and monitoring guide](https://supabase.com/docs/guides/database/inspect).

## Optimizing the number of connections [\#](https://supabase.com/docs/guides/platform/performance\#optimizing-the-number-of-connections)

The default connection limits for Postgres and Supavisor is based on your compute size. See the default connection numbers in the [Compute Add-ons](https://supabase.com/docs/guides/platform/compute-add-ons) section.

If the number of connections is insufficient, you will receive the following error upon connecting to the DB:

`
$ psql -U postgres -h ...
FATAL: remaining connection slots are reserved for non-replication superuser connections
`

In such a scenario, you can consider:

- [upgrading to a larger compute add-on](https://supabase.com/dashboard/project/_/settings/compute-and-disk)
- configuring your clients to use fewer connections
- manually configuring the database for a higher number of connections

### Configuring clients to use fewer connections [\#](https://supabase.com/docs/guides/platform/performance\#configuring-clients-to-use-fewer-connections)

You can use the [pg\_stat\_activity](https://www.postgresql.org/docs/current/monitoring-stats.html#MONITORING-PG-STAT-ACTIVITY-VIEW) view to debug which clients are holding open connections on your DB. `pg_stat_activity` only exposes information on direct connections to the database. Information on the number of connections to Supavisor is available [via the metrics endpoint](https://supabase.com/docs/guides/platform/metrics).

Depending on the clients involved, you might be able to configure them to work with fewer connections (e.g. by imposing a limit on the maximum number of connections they're allowed to use), or shift specific workloads to connect via [Supavisor](https://supabase.com/docs/guides/database/connecting-to-postgres#connection-pooler) instead. Transient workflows, which can quickly scale up and down in response to traffic (e.g. serverless functions), can especially benefit from using a connection pooler rather than connecting to the DB directly.

### Allowing higher number of connections [\#](https://supabase.com/docs/guides/platform/performance\#allowing-higher-number-of-connections)

You can configure Postgres connection limit among other parameters by using [Custom Postgres Config](https://supabase.com/docs/guides/platform/custom-postgres-config#custom-postgres-config).

### Enterprise [\#](https://supabase.com/docs/guides/platform/performance\#enterprise)

[Contact us](https://forms.supabase.com/enterprise) if you need help tuning your database for your specific workflow.

### Is this helpful?

NoYes

### On this page

[Examining query performance](https://supabase.com/docs/guides/platform/performance#examining-query-performance) [Optimizing the number of connections](https://supabase.com/docs/guides/platform/performance#optimizing-the-number-of-connections) [Configuring clients to use fewer connections](https://supabase.com/docs/guides/platform/performance#configuring-clients-to-use-fewer-connections) [Allowing higher number of connections](https://supabase.com/docs/guides/platform/performance#allowing-higher-number-of-connections) [Enterprise](https://supabase.com/docs/guides/platform/performance#enterprise)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings