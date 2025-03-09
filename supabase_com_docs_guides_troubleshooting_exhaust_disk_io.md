# High Disk I/O

Last edited: 2/28/2025

* * *

## Understanding disk IO and disk IO budget [\#](https://supabase.com/docs/guides/troubleshooting/exhaust-disk-io\#understanding-disk-io-and-disk-io-budget)

Disk IO refers to two metrics: throughput in Megabits per Second and IOPS which are Input/Output Operations per Second. Depending on the compute add-on of your instance you will have [different baseline performances](https://supabase.com/docs/guides/platform/compute-add-ons#compute-size).

Smaller compute instances can burst and exceed their baseline performance for a short quota of time every day. This is represented as your Disk IO Budget and once your Disk IO Budget is consumed, your instance reverts back to its baseline performance. Learn more about [choosing the right compute instance for consistent disk performance](https://supabase.com/docs/guides/platform/compute-add-ons#choosing-the-right-compute-instance-for-consistent-disk-performance).

## Depleting your disk IO budget [\#](https://supabase.com/docs/guides/troubleshooting/exhaust-disk-io\#depleting-your-disk-io-budget)

Running out of Disk IO Budget means that your instance is using more disk than its compute add-on can handle and essentially gets throttled. This could have a wide range of implications:

- Response times on requests can increase noticeably
- CPU usage rises noticeably due to IO wait
- Disruption of [daily backup](https://supabase.com/docs/guides/platform/backups#daily-backups) routines
- Disruption of internal Postgres processes such as [autovacuuming](https://supabase.com/docs/guides/platform/database-size#vacuum-operations)
- Your instance may become unresponsive

## Monitor your disk IO budget [\#](https://supabase.com/docs/guides/troubleshooting/exhaust-disk-io\#monitor-your-disk-io-budget)

To check your Disk IO Budget on the Supabase Platform, head over to [Database Health in the Reports section](https://supabase.com/dashboard/project/_/reports/database).

It is also possible to monitor your resources and set up alerts using Prometheus/Grafana. With Grafana you will be able to pinpoint potential causes and see more fine-grained metrics like how much of your RAM is used for caching and your Swap usage. Read the [Metrics Guide](https://supabase.com/docs/guides/platform/metrics) to learn more.

## Common reasons for high disk IO usage [\#](https://supabase.com/docs/guides/troubleshooting/exhaust-disk-io\#common-reasons-for-high-disk-io-usage)

Most operations on your Supabase project require disk IO in some form. Hence, there can be many reasons for high disk IO usage. Here are some common ones:

- **High Memory Usage:** Every Supabase project has 1GB of disk allocated for swapping. When your memory usage is high, the operating system might frequently move parts of the memory back and forth of the swap space on the disk.
- **Low Cache Usage:** If your cache hit rate is low, many of your database requests might go straight to the disk. Go to the [Cache Hit Rate Guide](https://supabase.com/docs/guides/platform/performance#hit-rate) to learn more.
- **Query performance:** Queries that take a long time to complete (>1 second) could be using your disk inefficiently. Check our guide on [examining query performance](https://supabase.com/docs/guides/platform/performance#examining-query-performance).
- **High popularity:** Congrats! Your side project turned out to be a real success and is getting more requests than it can handle.

## How to fix [\#](https://supabase.com/docs/guides/troubleshooting/exhaust-disk-io\#how-to-fix)

1. **Upgrade your compute:** You can get a Compute Add-on for your project. Larger compute options (4XL and above) have more consistent disk performance. See your [upgrade options](https://supabase.com/dashboard/project/_/settings/compute-and-disk) by selecting your project. Do reference the [different baseline performances](https://supabase.com/docs/guides/platform/compute-add-ons#compute-size) that come with larger Compute Add-ons.
2. **Optimize performance:** Get more out of your instance's resources by optimizing your usage. Have a look at our [performance tuning guide](https://supabase.com/docs/guides/platform/performance#examining-query-performance) and our [production readiness guide](https://supabase.com/docs/guides/platform/going-into-prod#performance).

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings