# High CPU usage

Last edited: 2/3/2025

* * *

Learn what high CPU usage could mean for your Supabase instance and what could have caused it.

## The danger of high CPU usage [\#](https://supabase.com/docs/guides/troubleshooting/high-cpu-usage\#the-danger-of-high-cpu-usage)

Every Supabase project runs in its dedicated virtual machine. Your instance will have a different set of hardware provisioned depending on your [compute add-on](https://supabase.com/docs/guides/platform/compute-add-ons). Your hardware may not be suitable for the intended workload and may experience high CPU usage.

High CPU usage could come with a range of issues:

- slower queries
- disruption of daily backup routines
- in rare cases, your instance may become unresponsive

Moreover, your instance might not be able to handle future traffic spikes if it already has a high CPU usage.

## Monitor your CPU [\#](https://supabase.com/docs/guides/troubleshooting/high-cpu-usage\#monitor-your-cpu)

You can check your CPU usage directly on the Supabase Platform. For this go to database health in the reports section or [click here](https://supabase.com/dashboard/project/_/reports/database) and select your project.

![CPU usage reported on Supabase dashboard](https://supabase.com/docs/img/guides/platform/exhaust-cpu-report.png)

It is also possible to monitor your resources and set up alerts using Prometheus/Grafana. You can find a guide for this [here](https://supabase.com/docs/guides/platform/metrics).

## Common reasons for high CPU usage [\#](https://supabase.com/docs/guides/troubleshooting/high-cpu-usage\#common-reasons-for-high-cpu-usage)

Everything you do with your Supabase project requires compute. Hence, there can be many reasons for high CPU usage. Here are some common ones:

- **Query performance:** Queries that take a long time to complete (>1 second) as well as excessive amounts of querying can put a strain on the CPU. Check our guide on [examining query performance](https://supabase.com/docs/guides/platform/performance#examining-query-performance).
- **Missing indexes:** Your database might have to scan through a large amount of data to find the information it needs. Creating indexes helps your database find data faster. Learn more about indexes [here](https://supabase.com/docs/guides/database/postgres/indexes).
- **Unsuitable compute:** The compute size of your Supabase project might not be suitable for your application as you might have more traffic or run resource-intensive operations.

## Solving high CPU usage [\#](https://supabase.com/docs/guides/troubleshooting/high-cpu-usage\#solving-high-cpu-usage)

There are two ways to solve high CPU:

1. **Optimize performance:** Get more out of your instance's resources by optimizing your usage. Have a look at our [performance tuning guide](https://supabase.com/docs/guides/platform/performance#examining-query-performance) and our [production readiness guide](https://supabase.com/docs/guides/platform/going-into-prod#performance).
2. **Upgrade your compute:** You can get a Compute Add-on for your project. Follow [this link](https://supabase.com/dashboard/project/_/settings/compute-and-disk) and select your project to see your upgrade options.

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings