Platform

# Manage Edge Function Invocations usage

* * *

## What you are charged for [\#](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations\#what-you-are-charged-for)

You are charged for the number of times your functions get invoked, regardless of the response status code.

## How charges are calculated [\#](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations\#how-charges-are-calculated)

Edge Function Invocations are billed using Package pricing, with each package representing 1 million invocations. If your usage falls between two packages, you are billed for the next whole package.

### Example [\#](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations\#example)

For simplicity, let's assume a package size of 1 million and a charge of $2 per package without a free quota.

| Invocations | Packages Billed | Costs |
| --- | --- | --- |
| 999,999 | 1 | $2 |
| 1,000,000 | 1 | $2 |
| 1,000,001 | 2 | $4 |
| 1,500,000 | 2 | $4 |

### Usage on your invoice [\#](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations\#usage-on-your-invoice)

Usage is shown as "Function Invocations" on your invoice.

## Pricing [\#](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations\#pricing)

$2 per 1 million invocations. You are only charged for usage exceeding your subscription plan's quota.

| Plan | Quota | Over-Usage |
| --- | --- | --- |
| Free | 500,000 | - |
| Pro | 2 million | $2 per 1 million invocations |
| Team | 2 million | $2 per 1 million invocations |
| Enterprise | Custom | Custom |

## Billing examples [\#](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations\#billing-examples)

### Within quota [\#](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations\#within-quota)

The organization's function invocations are within the quota, so no charges apply.

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
| Compute Hours Micro | 744 hours | $10 |
| Function Invocations | 1,800,000 invocations | $0 |
| **Subtotal** |  | **$35** |
| Compute Credits |  | -$10 |
| **Total** |  | **$25** |

### Exceeding quota [\#](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations\#exceeding-quota)

The organization's function invocations exceed the quota by 1.4 million, incurring charges for this additional usage.

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
| Compute Hours Micro | 744 hours | $10 |
| Function Invocations | 3,400,000 invocations | $4 |
| **Subtotal** |  | **$39** |
| Compute Credits |  | -$10 |
| **Total** |  | **$29** |

## View usage [\#](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations\#view-usage)

You can view Edge Function Invocations usage on the [organization's usage page](https://supabase.com/dashboard/org/_/usage). The page shows the usage of all projects by default. To view the usage for a specific project, select it from the dropdown. You can also select a different time period.

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

In the Edge Function Invocations section, you can see how many invocations your projects have had during the selected time period.

![Usage page Edge Function Invocations section](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-function-invocations--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[What you are charged for](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations#what-you-are-charged-for) [How charges are calculated](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations#how-charges-are-calculated) [Example](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations#example) [Usage on your invoice](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations#usage-on-your-invoice) [Pricing](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations#pricing) [Billing examples](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations#billing-examples) [Within quota](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations#within-quota) [Exceeding quota](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations#exceeding-quota) [View usage](https://supabase.com/docs/guides/platform/manage-your-usage/edge-function-invocations#view-usage)

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Usage page Edge Function Invocations section](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-function-invocations--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings