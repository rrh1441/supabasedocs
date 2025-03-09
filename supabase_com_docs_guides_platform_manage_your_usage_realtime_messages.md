Platform

# Manage Realtime Messages usage

* * *

## What you are charged for [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages\#what-you-are-charged-for)

You are charged for the number of messages going through Supabase Realtime throughout the billing cycle. Includes database changes, Broadcast and Presence.

**Database changes**
Each database change counts as one message per client that listens to the event. For example, if a database change occurs and 5 clients listen to that database event, it counts as 5 messages.

**Broadcast**
Each broadcast message counts as one message sent plus one message per subscribed client that receives it. For example, if you broadcast a message and 4 clients listen to it, it counts as 5 messages—1 sent and 4 received.

## How charges are calculated [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages\#how-charges-are-calculated)

Realtime Messages are billed using Package pricing, with each package representing 1 million messages. If your usage falls between two packages, you are billed for the next whole package.

### Example [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages\#example)

For simplicity, let's assume a package size of 1,000,000 and a charge of $2.50 per package without quota.

| Messages | Packages Billed | Costs |
| --- | --- | --- |
| 999,999 | 1 | $2.50 |
| 1,000,000 | 1 | $2.50 |
| 1,000,001 | 2 | $5.00 |
| 1,500,000 | 2 | $5.00 |

### Usage on your invoice [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages\#usage-on-your-invoice)

Usage is shown as "Realtime Messages" on your invoice.

## Pricing [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages\#pricing)

$2.50 per 1 million messages. You are only charged for usage exceeding your subscription plan's quota.

| Plan | Quota | Over-Usage |
| --- | --- | --- |
| Free | 2 million | - |
| Pro | 5 million | $2.50 per 1 million messages |
| Team | 5 million | $2.50 per 1 million messages |
| Enterprise | Custom | Custom |

## Billing examples [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages\#billing-examples)

### Within quota [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages\#within-quota)

The organization's Realtime messages are within the quota, so no charges apply.

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
| Compute Hours Micro | 744 hours | $10 |
| Realtime Messages | 1.8 million messages | $0 |
| **Subtotal** |  | **$35** |
| Compute Credits |  | -$10 |
| **Total** |  | **$25** |

### Exceeding quota [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages\#exceeding-quota)

The organization's Realtime messages exceed the quota by 3.5 million, incurring charges for this additional usage.

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
| Compute Hours Micro | 744 hours | $10 |
| Realtime Messages | 8.5 million messages | $10 |
| **Subtotal** |  | **$45** |
| Compute Credits |  | -$10 |
| **Total** |  | **$35** |

## View usage [\#](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages\#view-usage)

You can view Realtime Messages usage on the [organization's usage page](https://supabase.com/dashboard/org/_/usage). The page shows the usage of all projects by default. To view the usage for a specific project, select it from the dropdown. You can also select a different time period.

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

In the Realtime Messages section, you can see the usage for the selected time period.

![Usage page Realtime Messages section](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-realtime-messages--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[What you are charged for](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages#what-you-are-charged-for) [How charges are calculated](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages#how-charges-are-calculated) [Example](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages#example) [Usage on your invoice](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages#usage-on-your-invoice) [Pricing](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages#pricing) [Billing examples](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages#billing-examples) [Within quota](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages#within-quota) [Exceeding quota](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages#exceeding-quota) [View usage](https://supabase.com/docs/guides/platform/manage-your-usage/realtime-messages#view-usage)

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Usage page Realtime Messages section](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-realtime-messages--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings