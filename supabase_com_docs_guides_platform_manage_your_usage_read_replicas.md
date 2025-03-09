Platform

# Manage Read Replica usage

* * *

## What you are charged for [\#](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas\#what-you-are-charged-for)

Each [Read Replica](https://supabase.com/docs/guides/platform/read-replicas) is a dedicated database. You are charged for its resources: [Compute](https://supabase.com/docs/guides/platform/compute-and-disk#compute), [Disk Size](https://supabase.com/docs/guides/platform/database-size#disk-size), provisioned [Disk IOPS](https://supabase.com/docs/guides/platform/compute-and-disk#provisioned-disk-throughput-and-iops), provisioned [Disk Throughput](https://supabase.com/docs/guides/platform/compute-and-disk#provisioned-disk-throughput-and-iops), and [IPv4](https://supabase.com/docs/guides/platform/ipv4-address).

## How charges are calculated [\#](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas\#how-charges-are-calculated)

Read Replica charges are the total of the charges listed below.

**Compute**
Compute is charged by the hour, meaning you are charged for the exact number of hours that a Read Replica is running and, therefore, incurring Compute usage. If a Read Replica runs for part of an hour, you are still charged for the full hour.

Read Replicas run on the same Compute size as the primary database.

**Disk Size**
Refer to [Manage Disk Size usage](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size) for details on how charges are calculated. The disk size of a Read Replica is 1.25x the size of the primary disk to account for WAL archives. With a Read Replica you go beyond your subscription plan's quota for Disk Size.

**Provisioned Disk IOPS (optional)**
Read Replicas inherit any additional provisioned Disk IOPS from the primary database. Refer to [Manage Disk IOPS usage](https://supabase.com/docs/guides/platform/manage-your-usage/disk-iops) for details on how charges are calculated.

**Provisioned Disk Throughput (optional)**
Read Replicas inherit any additional provisioned Disk Throughput from the primary database. Refer to [Manage Disk Throughput usage](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput) for details on how charges are calculated.

**IPv4 (optional)**
If the primary database has a configured IPv4 address, its Read Replicas are also assigned one, with charges for each. Refer to [Manage IPv4 usage](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4) for details on how charges are calculated.

### Usage on your invoice [\#](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas\#usage-on-your-invoice)

Compute incurred by Read Replicas is shown as "Replica Compute Hours" on your invoice. Disk Size, Disk IOPS, Disk Throughput and IPv4 are not shown separately for Read Replicas and are rolled up into the project.

## Billing examples [\#](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas\#billing-examples)

### No additional resources configured [\#](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas\#no-additional-resources-configured)

The project has one Read Replica and no IPv4 and no additional Disk IOPS and Disk Throughput configured.

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
|  |  |  |
| Compute Hours Small Project 1 | 744 hours | $15 |
| Disk Size Project 1 | 8 GB | $0 |
|  |  |  |
| Compute Hours Small Replica | 744 hours | $15 |
| Disk Size Replica | 10 GB | $1.25 |
|  |  |  |
| **Subtotal** |  | **$56.25** |
| Compute Credits |  | -$10 |
| **Total** |  | **$46.25** |

### Additional resources configured [\#](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas\#additional-resources-configured)

The project has two Read Replicas and IPv4 and additional Disk IOPS and Disk Throughput configured.

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
|  |  |  |
| Compute Hours Large Project 1 | 744 hours | $110 |
| Disk Size Project 1 | 8 GB | $0 |
| Disk IOPS Project 1 | 3600 | $14.40 |
| Disk Throughput Project 1 | 200 Mbps | $7.13 |
| IPv4 Hours Project 1 | 744 hours | $4 |
|  |  |  |
| Compute Hours Large Replica 1 | 744 hours | $110 |
| Disk Size Replica 1 | 10 GB | $1.25 |
| Disk IOPS Replica 1 | 3600 | $14.40 |
| Disk Throughput Replica 1 | 200 Mbps | $7.13 |
| IPv4 Hours Replica 1 | 744 hours | $4 |
|  |  |  |
| Compute Hours Large Replica 2 | 744 hours | $110 |
| Disk Size Replica 2 | 10 GB | $1.25 |
| Disk IOPS Replica 2 | 3600 | $14.40 |
| Disk Throughput Replica 2 | 200 Mbps | $7.13 |
| IPv4 Hours Replica 2 | 744 hours | $4 |
|  |  |  |
| **Subtotal** |  | **$434.09** |
| Compute Credits |  | -$10 |
| **Total** |  | **$424.09** |

## FAQ [\#](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas\#faq)

### Do Compute Credits apply to Read Replica Compute? [\#](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas\#do-compute-credits-apply-to-read-replica-compute)

No, Compute Credits do not apply to Read Replica Compute.

### Is this helpful?

NoYes

### On this page

[What you are charged for](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas#what-you-are-charged-for) [How charges are calculated](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas#how-charges-are-calculated) [Usage on your invoice](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas#usage-on-your-invoice) [Billing examples](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas#billing-examples) [No additional resources configured](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas#no-additional-resources-configured) [Additional resources configured](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas#additional-resources-configured) [FAQ](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas#faq) [Do Compute Credits apply to Read Replica Compute?](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas#do-compute-credits-apply-to-read-replica-compute)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings