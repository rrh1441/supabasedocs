Platform

# Manage Monthly Active SSO Users usage

* * *

## What you are charged for [\#](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso\#what-you-are-charged-for)

You are charged for the number of distinct users who log in or refresh their token during the billing cycle using a SAML 2.0 compatible identity provider. Each unique user is counted only once per billing cycle, regardless of how many times they authenticate. These users are referred to as "SSO MAUs".

### Example [\#](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso\#example)

Your billing cycle runs from January 1 to January 31. Although User-1 was signed in multiple times, they are counted as a single SSO MAU for this billing cycle.

1

### Sign User-1 in on January 3

The SSO MAU count increases from 0 to 1.

`
const { data, error } = await supabase.auth.signInWithSSO({
domain: 'company.com'
})
if (data?.url) {
// redirect User-1 to the identity provider's authentication flow
window.location.href = data.url
}
`

2

### Sign User-1 out on January 4

`javascript const {error} = await supabase.auth.signOut() `

3

### Sign User-1 in again on January 17

The SSO MAU count remains 1.

`
const { data, error } = await supabase.auth.signInWithSSO({
domain: 'company.com'
})
if (data?.url) {
// redirect User-1 to the identity provider's authentication flow
window.location.href = data.url
}
`

## How charges are calculated [\#](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso\#how-charges-are-calculated)

You are charged by SSO MAU.

### Usage on your invoice [\#](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso\#usage-on-your-invoice)

Usage is shown as "Monthly Active SSO Users" on your invoice.

## Pricing [\#](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso\#pricing)

$0.015 per SSO MAU. You are only charged for usage exceeding your subscription plan's quota.

The count resets at the start of each billing cycle.

| Plan | Quota | Over-Usage |
| --- | --- | --- |
| Pro | 50 | $0.015 per SSO MAU |
| Team | 50 | $0.015 per SSO MAU |
| Enterprise | Custom | Custom |

## Billing examples [\#](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso\#billing-examples)

### Within quota [\#](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso\#within-quota)

The organization's SSO MAU usage for the billing cycle is within the quota, so no charges apply.

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
| Compute Hours Micro | 744 hours | $10 |
| Monthly Active SSO Users | 37 SSO MAU | $0 |
| **Subtotal** |  | **$35** |
| Compute Credits |  | -$10 |
| **Total** |  | **$25** |

### Exceeding quota [\#](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso\#exceeding-quota)

The organization's SSO MAU usage for the billing cycle exceeds the quota by 10, incurring charges for this additional usage.

| Line Item | Units | Costs |
| --- | --- | --- |
| Pro Plan | 1 | $25 |
| Compute Hours Micro | 744 hours | $10 |
| Monthly Active SSO Users | 60 SSO MAU | $0.15 |
| **Subtotal** |  | **$35.15** |
| Compute Credits |  | -$10 |
| **Total** |  | **$25.15** |

## View usage [\#](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso\#view-usage)

You can view Monthly Active SSO Users usage on the [organization's usage page](https://supabase.com/dashboard/org/_/usage). The page shows the usage of all projects by default. To view the usage for a specific project, select it from the dropdown. You can also select a different time period.

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

In the Monthly Active SSO Users section, you can see the usage for the selected time period.

![Usage page Monthly Active SSO Users section](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-mau-sso--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[What you are charged for](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso#what-you-are-charged-for) [Example](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso#example) [How charges are calculated](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso#how-charges-are-calculated) [Usage on your invoice](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso#usage-on-your-invoice) [Pricing](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso#pricing) [Billing examples](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso#billing-examples) [Within quota](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso#within-quota) [Exceeding quota](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso#exceeding-quota) [View usage](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso#view-usage)

![Usage page navigation bar](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fusage-navbar--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings