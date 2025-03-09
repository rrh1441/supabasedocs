Platform

# Manage Disk size usage

* * *

## What you are charged for [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size\#what-you-are-charged-for)

Each database has a dedicated [disk](https://supabase.com/docs/guides/platform/compute-and-disk#disk). You are charged for the provisioned disk size.

Disk size is not relevant for the Free Plan. Instead Free Plan customers are limited by [Database size](https://supabase.com/docs/guides/platform/database-size).

## How charges are calculated [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size\#how-charges-are-calculated)

Disk size is charged by Gigabyte-Hours (GB-Hrs). 1 GB-Hr represents 1 GB being provisioned for 1 hour.
For example, having 10 GB provisioned for 5 hours results in 50 GB-Hrs (10 GB × 5 hours).

### Usage on your invoice [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size\#usage-on-your-invoice)

Usage is shown as "Disk Size GB-Hrs" on your invoice.

## Pricing [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size\#pricing)

Pricing depends on the [disk type](https://supabase.com/docs/guides/platform/compute-and-disk#disk-types), with gp3 being the default disk type.

### General purpose disks (gp3) [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size\#general-purpose-disks-gp3)

$0.000171 per GB-Hr ($0.125 per GB per month). The primary database of your project gets provisioned with an 8 GB disk. You are only charged for provisioned disk size exceeding these 8 GB.

| Plan | Included Disk Size | Over-Usage per GB per month | Over-Usage per GB-Hr |
| --- | --- | --- | --- |
| Pro | 8 GB | $0.125 | $0.000171 |
| Team | 8 GB | $0.125 | $0.000171 |
| Enterprise | Custom | Custom | Custom |

Launching a Read Replica creates an additional database with its own dedicated disk. You are charged from the first byte of provisioned disk for the Read Replica. Refer to [Manage Read Replica usage](https://supabase.com/docs/guides/platform/manage-your-usage/read-replicas) for details on billing.

### High performance disks (io2) [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size\#high-performance-disks-io2)

$0.000267 per GB-Hr ($0.195 per GB per month).
Unlike general purpose disks, high performance disks are billed from the first byte of provisioned disk.

| Plan | Included Disk size | Usage per GB per month | Usage per GB-Hr |
| --- | --- | --- | --- |
| Pro | 0 GB | $0.195 | $0.000267 |
| Team | 0 GB | $0.195 | $0.000267 |
| Enterprise | Custom | Custom | Custom |

## Billing examples [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size\#billing-examples)

### Gp3 [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size\#gp3)

Project 1 and 2 don't exceed the included disk size, so no charges for Disk size apply. Project 3 exceeds the included disk size by 42 GB, incurring charges for this additional usage.

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
|  |  |  |
| Compute Hours Micro Project 1 | 744 hours | $10 |
| Disk Size Project 1 | 8 GB | $0 |
|  |  |  |
| Compute Hours Micro Project 2 | 744 hours | $10 |
| Disk Size Project 2 | 8 GB | $0 |
|  |  |  |
| Compute Hours Micro Project 3 | 744 hours | $10 |
| Disk Size Project 3 | 50 GB | $5.25 |
|  |  |  |
| **Subtotal** |  | **$50.25** |
| Compute Credits |  | -$10 |
| **Total** |  | **$40.25** |

### Io2 [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size\#io2)

This disk type is billed from the first byte of provisioned disk, meaning for 66 GB across all projects.

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
|  |  |  |
| Compute Hours Micro Project 1 | 744 hours | $10 |
| Disk Size Project 1 | 8 GB | $1.56 |
|  |  |  |
| Compute Hours Micro Project 2 | 744 hours | $10 |
| Disk Size Project 2 | 8 GB | $1.56 |
|  |  |  |
| Compute Hours Micro Project 3 | 744 hours | $10 |
| Disk Size Project 3 | 50 GB | $9.75 |
|  |  |  |
| **Subtotal** |  | **$67.87** |
| Compute Credits |  | -$10 |
| **Total** |  | **$57.87** |

## View usage [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size\#view-usage)

You can view Disk size usage on the [organization's usage page](https://supabase.com/dashboard/org/_/usage). The page shows the usage of all projects by default. To view the usage for a specific project, select it from the dropdown.

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

In the Disk size section, you can see how much disk size your projects have provisioned.

![Usage page Disk Size section](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-disk-size--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Disk size distribution [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size\#disk-size-distribution)

To see how your disk usage is distributed across Database, WAL, and System categories, refer to [Disk size distribution](https://supabase.com/docs/guides/platform/database-size#disk-size-distribution).

## Reduce Disk size [\#](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size\#reduce-disk-size)

To see how you can downsize your disk, refer to [Reducing disk size](https://supabase.com/docs/guides/platform/database-size#reducing-disk-size)

### Is this helpful?

NoYes

### On this page

[What you are charged for](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size#what-you-are-charged-for) [How charges are calculated](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size#how-charges-are-calculated) [Usage on your invoice](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size#usage-on-your-invoice) [Pricing](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size#pricing) [General purpose disks (gp3)](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size#general-purpose-disks-gp3) [High performance disks (io2)](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size#high-performance-disks-io2) [Billing examples](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size#billing-examples) [Gp3](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size#gp3) [Io2](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size#io2) [View usage](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size#view-usage) [Disk size distribution](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size#disk-size-distribution) [Reduce Disk size](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size#reduce-disk-size)

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Usage page Disk Size section](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-disk-size--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings