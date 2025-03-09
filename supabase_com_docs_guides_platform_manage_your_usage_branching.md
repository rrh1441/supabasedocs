Platform

# Manage Branching usage

* * *

## What you are charged for [\#](https://supabase.com/docs/guides/platform/manage-your-usage/branching\#what-you-are-charged-for)

Each [Preview branch](https://supabase.com/docs/guides/deployment/branching) is a separate environment with all Supabase services (Database, Auth, Storage, etc.). You're charged for usage within that environment—such as [Compute](https://supabase.com/docs/guides/platform/manage-your-usage/compute), [Disk Size](https://supabase.com/docs/guides/platform/manage-your-usage/disk-size), [Egress](https://supabase.com/docs/guides/platform/manage-your-usage/egress), and [Storage](https://supabase.com/docs/guides/platform/manage-your-usage/storage-size)—just like the project you branched from.

Usage by Preview branches counts toward your subscription plan's quota.

## How charges are calculated [\#](https://supabase.com/docs/guides/platform/manage-your-usage/branching\#how-charges-are-calculated)

Refer to individual [usage items](https://supabase.com/docs/guides/platform/manage-your-usage) for details on how charges are calculated. Branching charges are the sum of all these items.

### Usage on your invoice [\#](https://supabase.com/docs/guides/platform/manage-your-usage/branching\#usage-on-your-invoice)

Compute incurred by Preview branches is shown as "Branching Compute Hours" on your invoice. Other usage items are not shown separately for branches and are rolled up into the project.

## Pricing [\#](https://supabase.com/docs/guides/platform/manage-your-usage/branching\#pricing)

There is no fixed fee for a Preview branch. You only pay for the usage it incurs. With Compute costs of $0.01344 per hour, a branch running on Micro Compute size starts at $0.32 per day.

## Billing examples [\#](https://supabase.com/docs/guides/platform/manage-your-usage/branching\#billing-examples)

The project has a Preview branch "XYZ", that runs for 30 hours, incurring Compute and Egress costs. Disk Size usage remains within the 8 GB included in the subscription plan, so no additional charges apply.

| Line Item | Costs |
| --- | --- |
| Pro Plan | $25 |
|  |  |
| Compute Hours Small Project 1 | $15 |
| Egress Project 1 | $7 |
| Disk Size Project 1 | $3 |
|  |  |
| Compute Hours Micro Branch XYZ | $0.4 |
| Egress Branch XYZ | $1 |
| Disk Size Branch XYZ | $0 |
|  |  |
| **Subtotal** | **$51.4** |
| Compute Credits | -$10 |
| **Total** | **$41.4** |

## View usage [\#](https://supabase.com/docs/guides/platform/manage-your-usage/branching\#view-usage)

You can view Branching usage on the [organization's usage page](https://supabase.com/dashboard/org/_/usage). The page shows the usage of all projects by default. To view the usage for a specific project, select it from the dropdown. You can also select a different time period.

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

In the Usage Summary section, you can see how many hours your Preview branches existed during the selected time period. Hover over "Branches Compute Hours" for a detailed breakdown.

![Usage summary Branches Compute Hours](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-summary-branch-hours--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

## Optimize usage [\#](https://supabase.com/docs/guides/platform/manage-your-usage/branching\#optimize-usage)

- Merge Preview branches as soon as they are ready
- Delete Preview branches that are no longer in use
- Check whether your [persistent branches](https://supabase.com/docs/guides/deployment/branching#persistent-branches) need to be defined as persistent, or if they can be ephemeral instead. Persistent branches will remain active even after the underlying PR is closed.

## FAQ [\#](https://supabase.com/docs/guides/platform/manage-your-usage/branching\#faq)

### Do Compute Credits apply to Branching Compute? [\#](https://supabase.com/docs/guides/platform/manage-your-usage/branching\#do-compute-credits-apply-to-branching-compute)

No, Compute Credits do not apply to Branching Compute.

### Is this helpful?

NoYes

### On this page

[What you are charged for](https://supabase.com/docs/guides/platform/manage-your-usage/branching#what-you-are-charged-for) [How charges are calculated](https://supabase.com/docs/guides/platform/manage-your-usage/branching#how-charges-are-calculated) [Usage on your invoice](https://supabase.com/docs/guides/platform/manage-your-usage/branching#usage-on-your-invoice) [Pricing](https://supabase.com/docs/guides/platform/manage-your-usage/branching#pricing) [Billing examples](https://supabase.com/docs/guides/platform/manage-your-usage/branching#billing-examples) [View usage](https://supabase.com/docs/guides/platform/manage-your-usage/branching#view-usage) [Optimize usage](https://supabase.com/docs/guides/platform/manage-your-usage/branching#optimize-usage) [FAQ](https://supabase.com/docs/guides/platform/manage-your-usage/branching#faq) [Do Compute Credits apply to Branching Compute?](https://supabase.com/docs/guides/platform/manage-your-usage/branching#do-compute-credits-apply-to-branching-compute)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings


![Usage summary Branches Compute Hours](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-summary-branch-hours--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)