Platform

# Manage Disk Throughput usage

* * *

## What you are charged for [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput\#what-you-are-charged-for)

Each database has a dedicated disk, and you are charged for its provisioned disk throughput. However, unless you explicitly opt in for additional throughput, no charges apply.

Refer to our [disk guide](https://supabase.com/docs/guides/platform/compute-and-disk#disk) for details on how disk throughput, disk IOPS, disk size, disk type and compute size interact, along with their limitations and constraints.

Launching a Read Replica creates an additional database with its own dedicated disk. Read Replicas inherit the primary database's disk throughput settings. You are charged for the provisioned throughput of the Read Replica.

## How charges are calculated [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput\#how-charges-are-calculated)

Disk throughput is charged by Mbps-Hrs (Mbps stands for megabits per second). 1 Mbps-Hr represents disk throughput of 1 Mbps being provisioned for 1 hour. For example, having 10 Mbps provisioned for 5 hours results in 50 Mbps-Hrs (10 Mbps × 5 hours).

### Usage on your invoice [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput\#usage-on-your-invoice)

Usage is shown as "Disk Throughput Mbps-Hrs" on your invoice.

## Pricing [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput\#pricing)

Pricing depends on the [disk type](https://supabase.com/docs/guides/platform/compute-and-disk#disk-types), with type gp3 being the default.

### General purpose disks (gp3) [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput\#general-purpose-disks-gp3)

$0.00013 per Mbps-Hr ($0.095 per Mbps per month). gp3 disks come with a baseline throughput of 125 Mbps. You are only charged for provisioned throughput exceeding these 125 Mbps.

| Plan | Included Disk Throughput | Over-Usage per Mbps per month | Over-Usage per Mbps-Hrs |
| --- | --- | --- | --- |
| Pro | 125 Mbps | $0.095 | $0.00013 |
| Team | 125 Mbps | $0.095 | $0.00013 |
| Enterprise | Custom | Custom | Custom |

### High performance disks (io2) [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput\#high-performance-disks-io2)

There are no charges. Throughput scales with IOPS at no additional cost.

## Billing examples [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput\#billing-examples)

### No additional throughput configured [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput\#no-additional-throughput-configured)

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
|  |  |  |
| Compute Hours Micro Project 1 | 744 hours | $10 |
| Disk Throughput Project 1 | 125 Mbps | $0 |
|  |  |  |
| **Subtotal** |  | **$35** |
| Compute Credits |  | -$10 |
| **Total** |  | **$25** |

### Additional throughput configured [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput\#additional-throughput-configured)

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
|  |  |  |
| Compute Hours Large Project 1 | 744 hours | $110 |
| Disk Throughput Project 1 | 200 Mbps | $7.13 |
|  |  |  |
| **Subtotal** |  | **$142.13** |
| Compute Credits |  | -$10 |
| **Total** |  | **$132.13** |

### Additional throughput configured with Read Replica [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput\#additional-throughput-configured-with-read-replica)

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
|  |  |  |
| Compute Hours Large Project 1 | 744 hours | $110 |
| Disk Throughput Project 1 | 200 Mbps | $7.13 |
|  |  |  |
| Compute Hours Large Replica | 744 hours | $110 |
| Disk Throughput Replica | 200 Mbps | $7.13 |
|  |  |  |
| **Subtotal** |  | **$259.26** |
| Compute Credits |  | -$10 |
| **Total** |  | **$249.26** |

### Is this helpful?

NoYes

### On this page

[What you are charged for](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput#what-you-are-charged-for) [How charges are calculated](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput#how-charges-are-calculated) [Usage on your invoice](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput#usage-on-your-invoice) [Pricing](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput#pricing) [General purpose disks (gp3)](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput#general-purpose-disks-gp3) [High performance disks (io2)](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput#high-performance-disks-io2) [Billing examples](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput#billing-examples) [No additional throughput configured](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput#no-additional-throughput-configured) [Additional throughput configured](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput#additional-throughput-configured) [Additional throughput configured with Read Replica](https://supabase.com/docs/guides/platform/manage-your-usage/disk-throughput#additional-throughput-configured-with-read-replica)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings