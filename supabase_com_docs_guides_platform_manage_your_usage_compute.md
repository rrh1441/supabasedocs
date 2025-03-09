Platform

# Manage Compute usage

* * *

## What you are charged for [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#what-you-are-charged-for)

Each project on the Supabase platform includes a dedicated Postgres instance running on its own server. You are charged for the [Compute](https://supabase.com/docs/guides/platform/compute-and-disk#compute) resources of that server, independent of your database usage.

Paused projects do not count towards Compute usage.

## How charges are calculated [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#how-charges-are-calculated)

Compute is charged by the hour, meaning you are charged for the exact number of hours that a project is running and, therefore, incurring Compute usage. If a project runs for part of an hour, you are still charged for the full hour.

Each project you launch increases your monthly Compute costs.

### Example [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#example)

Your billing cycle runs from January 1 to January 31. On January 10 at 4:30 PM, you switch your project from the Micro Compute size to the Small Compute size. At the end of the billing cycle you are billed for 233 hours of Micro Compute size and 511 hours of Small Compute size.

| Time Window | Compute Size | Hours Billed | Description |
| --- | --- | --- | --- |
| January 1, 00:00 AM - January 10, 4:00 PM | Micro | 232 |  |
| January 10, 04:00 PM - January 10, 4:30 PM | Micro | 1 | full hour is billed |
| January 10, 04:30 PM - January 10, 5:00 PM | Small | 1 | full hour is billed |
| January 10, 05:00 PM - January 31, 23:59 PM | Small | 511 |  |

### Usage on your invoice [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#usage-on-your-invoice)

Usage is shown as "Compute Hours" on your invoice.

## Compute Credits [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#compute-credits)

Paid plans include $10 in Compute Credits, which cover one project running on the Micro/Nano Compute size or portions of other Compute sizes. Compute Credits are applied to your Compute costs and are provided to an organization each month. They reset monthly and do not accumulate.

## Pricing [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#pricing)

| Compute Size | Hourly Price USD | Monthly Price USD |
| --- | --- | --- |
| Nano[1](https://supabase.com/docs/guides/platform/manage-your-usage/compute#user-content-fn-1) | $0 | $0 |
| Micro | $0.01344 | ~$10 |
| Small | $0.0206 | ~$15 |
| Medium | $0.0822 | ~$60 |
| Large | $0.1517 | ~$110 |
| XL | $0.2877 | ~$210 |
| 2XL | $0.562 | ~$410 |
| 4XL | $1.32 | ~$960 |
| 8XL | $2.562 | ~$1,870 |
| 12XL | $3.836 | ~$2,800 |
| 16XL | $5.12 | ~$3,730 |

##### Nano Compute size in paid plan organizations

In paid organizations, Nano Compute are billed at the same price as Micro Compute. It is recommended to upgrade your Project from Nano Compute to Micro Compute when it's convenient for you. Compute sizes are not auto-upgraded because of the downtime incurred. See [Supabase Pricing](https://supabase.com/pricing) for more information. You cannot launch Nano instances on paid plans, only Micro and above - but you might have Nano instances after upgrading from Free Plan.

## Billing examples [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#billing-examples)

### One project [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#one-project)

The project runs on the same Compute size throughout the entire billing cycle.

| Line Item | Hours | Costs |
| --- | --- | --- |
| Pro Plan | - | $25 |
| Compute Hours Micro Project 1 | 744 | $10 |
| **Subtotal** |  | **$35** |
| Compute Credits |  | -$10 |
| **Total** |  | **$25** |

### Multiple projects [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#multiple-projects)

All projects run on the same Compute size throughout the entire billing cycle.

| Line Item | Hours | Costs |
| --- | --- | --- |
| Pro Plan | - | $25 |
| Compute Hours Micro Project 1 | 744 | $10 |
| Compute Hours Micro Project 2 | 744 | $10 |
| Compute Hours Micro Project 3 | 744 | $10 |
| **Subtotal** |  | **$55** |
| Compute Credits |  | -$10 |
| **Total** |  | **$45** |

### One project on different Compute sizes [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#one-project-on-different-compute-sizes)

The project's Compute size changes throughout the billing cycle.

| Line Item | Hours | Costs |
| --- | --- | --- |
| Pro Plan | - | $25 |
| Compute Hours Micro Project 1 | 233 | $3 |
| Compute Hours Small Project 1 | 511 | $11 |
| **Subtotal** |  | **$39** |
| Compute Credits |  | -$10 |
| **Total** |  | **$29** |

## View usage [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#view-usage)

You can view Compute usage on the [organization's usage page](https://supabase.com/dashboard/org/_/usage). The page shows the usage of all projects by default. To view the usage for a specific project, select it from the dropdown. You can also select a different time period.

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

In the Compute Hours section, you can see how many hours of a specific Compute size your projects have used during the selected time period. Hover over a specific date for a daily breakdown.

![Usage page Compute Hours section](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-compute--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

## Optimize usage [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#optimize-usage)

- Start out on a smaller Compute size, [create a report](https://supabase.com/dashboard/project/_/reports) on the Dashboard to monitor your CPU and memory utilization, and upgrade the Compute size as needed
- Load test your application in staging to understand your Compute requirements
- [Transfer projects](https://supabase.com/docs/guides/platform/project-transfer) to a Free Plan organization to reduce Compute usage
- Delete unused projects

## FAQ [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#faq)

### Do Compute Credits apply to line items other than Compute? [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#do-compute-credits-apply-to-line-items-other-than-compute)

No, Compute Credits apply only to Compute and do not cover other line items, including Read Replica Compute and Branching Compute.

## Footnotes [\#](https://supabase.com/docs/guides/platform/manage-your-usage/compute\#footnote-label)

1. Compute resources on the Free Plan are subject to change. [↩](https://supabase.com/docs/guides/platform/manage-your-usage/compute#user-content-fnref-1)


### Is this helpful?

NoYes

### On this page

[What you are charged for](https://supabase.com/docs/guides/platform/manage-your-usage/compute#what-you-are-charged-for) [How charges are calculated](https://supabase.com/docs/guides/platform/manage-your-usage/compute#how-charges-are-calculated) [Example](https://supabase.com/docs/guides/platform/manage-your-usage/compute#example) [Usage on your invoice](https://supabase.com/docs/guides/platform/manage-your-usage/compute#usage-on-your-invoice) [Compute Credits](https://supabase.com/docs/guides/platform/manage-your-usage/compute#compute-credits) [Pricing](https://supabase.com/docs/guides/platform/manage-your-usage/compute#pricing) [Billing examples](https://supabase.com/docs/guides/platform/manage-your-usage/compute#billing-examples) [One project](https://supabase.com/docs/guides/platform/manage-your-usage/compute#one-project) [Multiple projects](https://supabase.com/docs/guides/platform/manage-your-usage/compute#multiple-projects) [One project on different Compute sizes](https://supabase.com/docs/guides/platform/manage-your-usage/compute#one-project-on-different-compute-sizes) [View usage](https://supabase.com/docs/guides/platform/manage-your-usage/compute#view-usage) [Optimize usage](https://supabase.com/docs/guides/platform/manage-your-usage/compute#optimize-usage) [FAQ](https://supabase.com/docs/guides/platform/manage-your-usage/compute#faq) [Do Compute Credits apply to line items other than Compute?](https://supabase.com/docs/guides/platform/manage-your-usage/compute#do-compute-credits-apply-to-line-items-other-than-compute) [Footnotes](https://supabase.com/docs/guides/platform/manage-your-usage/compute#footnote-label)

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Usage page Compute Hours section](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-compute--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings