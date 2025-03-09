Platform

# Manage IPv4 usage

* * *

## What you are charged for [\#](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4\#what-you-are-charged-for)

You can assign a dedicated [IPv4 address](https://supabase.com/docs/guides/platform/ipv4-address) to a database by enabling the [IPv4 add-on](https://supabase.com/dashboard/project/_/settings/addons?panel=ipv4). You are charged for all IPv4 addresses configured across your databases.

If the primary database has a dedicated IPv4 address configured, its Read Replicas are also assigned one, with charges for each.

## How charges are calculated [\#](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4\#how-charges-are-calculated)

IPv4 addresses are charged by the hour, meaning you are charged for the exact number of hours that an IPv4 address is assigned to a database. If an address is assigned for part of an hour, you are still charged for the full hour.

### Example [\#](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4\#example)

Your billing cycle runs from January 1 to January 31. On January 10 at 4:30 PM, you enable the IPv4 add-on for your project. At the end of the billing cycle you are billed for 512 hours.

| Time Window | IPv4 add-on | Hours Billed | Description |
| --- | --- | --- | --- |
| January 1, 00:00 AM - January 10, 4:00 PM | Disabled | 0 |  |
| January 10, 04:00 PM - January 10, 4:30 PM | Disabled | 0 |  |
| January 10, 04:30 PM - January 10, 5:00 PM | Enabled | 1 | full hour is billed |
| January 10, 05:00 PM - January 31, 23:59 PM | Enabled | 511 |  |

### Usage on your invoice [\#](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4\#usage-on-your-invoice)

Usage is shown as "IPv4 Hours" on your invoice.

## Pricing [\#](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4\#pricing)

$0.0055 per hour ($4 per month).

## Billing examples [\#](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4\#billing-examples)

### One project [\#](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4\#one-project)

The project has the IPv4 add-on enabled throughout the entire billing cycle.

| Line Item | Hours | Costs |
| --- | --- | --- |
| Pro Plan | - | $25 |
| Compute Hours Micro Project 1 | 744 | $10 |
| IPv4 Hours | 744 | $4 |
| **Subtotal** |  | **$39** |
| Compute Credits |  | -$10 |
| **Total** |  | **$29** |

### Multiple projects [\#](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4\#multiple-projects)

All projects have the IPv4 add-on enabled throughout the entire billing cycle.

| Line Item | Hours | Costs |
| --- | --- | --- |
| Pro Plan | - | $25 |
|  |  |  |
| Compute Hours Micro Project 1 | 744 | $10 |
| IPv4 Hours Project 1 | 744 | $4 |
|  |  |  |
| Compute Hours Micro Project 2 | 744 | $10 |
| IPv4 Hours Project 2 | 744 | $4 |
|  |  |  |
| Compute Hours Micro Project 3 | 744 | $10 |
| IPv4 Hours Project 3 | 744 | $4 |
|  |  |  |
| **Subtotal** |  | **$67** |
| Compute Credits |  | -$10 |
| **Total** |  | **$57** |

### One project with Read Replicas [\#](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4\#one-project-with-read-replicas)

The project has two Read Replicas and the IPv4 add-on enabled throughout the entire billing cycle.

| Line Item | Hours | Costs |
| --- | --- | --- |
| Pro Plan | - | $25 |
|  |  |  |
| Compute Hours Small Project 1 | 744 | $15 |
| IPv4 Hours Project 1 | 744 | $4 |
|  |  |  |
| Compute Hours Small Replica 1 | 744 | $15 |
| IPv4 Hours Replica 1 | 744 | $4 |
|  |  |  |
| Compute Hours Small Replica 2 | 744 | $15 |
| IPv4 Hours Replica 2 | 744 | $4 |
|  |  |  |
| **Subtotal** |  | **$82** |
| Compute Credits |  | -$10 |
| **Total** |  | **$72** |

## Optimize usage [\#](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4\#optimize-usage)

To see whether your database actually needs a dedicated IPv4 address, refer to [When you need the IPv4 add-on](https://supabase.com/docs/guides/platform/ipv4-address#when-you-need-the-ipv4-add-on).

### Is this helpful?

NoYes

### On this page

[What you are charged for](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4#what-you-are-charged-for) [How charges are calculated](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4#how-charges-are-calculated) [Example](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4#example) [Usage on your invoice](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4#usage-on-your-invoice) [Pricing](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4#pricing) [Billing examples](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4#billing-examples) [One project](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4#one-project) [Multiple projects](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4#multiple-projects) [One project with Read Replicas](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4#one-project-with-read-replicas) [Optimize usage](https://supabase.com/docs/guides/platform/manage-your-usage/ipv4#optimize-usage)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings