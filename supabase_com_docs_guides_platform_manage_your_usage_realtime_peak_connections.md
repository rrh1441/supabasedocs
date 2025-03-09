Platform

# Manage Realtime Peak Connections usage

* * *

## What you are charged for [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections\#what-you-are-charged-for)

Realtime Peak Connections are measured by tracking the highest number of concurrent connections for each project during the billing cycle. Regardless of fluctuations, only the peak count per project is used for billing, and the totals from all projects are summed. Only successful connections are counted, connection attempts are not included.

### Example [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections\#example)

For simplicity, this example assumes a billing cycle of only three days.

| Project | Peak Connections Day 1 | Peak Connections Day 2 | Peak Connections Day 3 |
| --- | --- | --- | --- |
| Project A | 80 | 100 | 90 |
| Project B | 120 | 110 | 150 |

**Total billed connections:** 100 (Project A) + 150 (Project B) = **250 connections**

## How charges are calculated [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections\#how-charges-are-calculated)

Realtime Peak Connections are billed using Package pricing, with each package representing 1,000 peak connections. If your usage falls between two packages, you are billed for the next whole package.

### Example [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections\#example)

For simplicity, let's assume a package size of 1,000 and a charge of $10 per package with no quota.

| Peak Connections | Packages Billed | Costs |
| --- | --- | --- |
| 999 | 1 | $10 |
| 1,000 | 1 | $10 |
| 1,001 | 2 | $20 |
| 1,500 | 2 | $20 |

### Usage on your invoice [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections\#usage-on-your-invoice)

Usage is shown as "Realtime Peak Connections" on your invoice.

## Pricing [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections\#pricing)

$10 per 1,000 peak connections. You are only charged for usage exceeding your subscription plan's quota.

| Plan | Quota | Over-Usage |
| --- | --- | --- |
| Free | 200 | - |
| Pro | 500 | $10 per 1,000 peak connections |
| Team | 500 | $10 per 1,000 peak connections |
| Enterprise | Custom | Custom |

## Billing examples [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections\#billing-examples)

### Within quota [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections\#within-quota)

The organization's connections are within the quota, so no charges apply.

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
| Compute Hours Micro | 744 hours | $10 |
| Realtime Peak Connections | 350 connections | $0 |
| **Subtotal** |  | **$35** |
| Compute Credits |  | -$10 |
| **Total** |  | **$25** |

### Exceeding quota [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections\#exceeding-quota)

The organization's connections exceed the quota by 1,200, incurring charges for this additional usage.

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
| Compute Hours Micro | 744 hours | $10 |
| Realtime Peak Connections | 1,700 connections | $20 |
| **Subtotal** |  | **$45** |
| Compute Credits |  | -$10 |
| **Total** |  | **$35** |

## View usage [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections\#view-usage)

You can view Realtime Peak Connections usage on the [organization's usage page](https://supabase.com/dashboard/org/_/usage). The page shows the usage of all projects by default. To view the usage for a specific project, select it from the dropdown. You can also select a different time period.

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

In the Realtime Peak Connections section, you can see the usage for the selected time period.

![Usage page Realtime Peak Connections section](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-realtime-peak-connections--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[What you are charged for](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections#what-you-are-charged-for) [Example](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections#example) [How charges are calculated](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections#how-charges-are-calculated) [Example](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections#example) [Usage on your invoice](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections#usage-on-your-invoice) [Pricing](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections#pricing) [Billing examples](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections#billing-examples) [Within quota](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections#within-quota) [Exceeding quota](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections#exceeding-quota) [View usage](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-peak-connections#view-usage)

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Usage page Realtime Peak Connections section](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-realtime-peak-connections--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings