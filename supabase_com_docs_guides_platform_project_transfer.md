Platform

# Project Transfers

* * *

You can freely transfer projects between different organizations. Head to your [projects' general settings](https://supabase.com/dashboard/project/_/settings/general) to initiate a project transfer.

![Project Transfer: General Settings](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fproject-transfer-overview--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Project Transfer: Confirmation Modal](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fproject-transfer-modal--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

Source organization - the organization the project currently belongs to
Target organization - the organization you want to move the project to

## Pre-Requirements [\#](https://supabase.com/docs/guides/platform/project-transfer\#pre-requirements)

- You need to be the owner of the source organization.
- You need to be at least a member of the target organization you want to move the project to.
- Projects with support tier add-ons cannot be transferred at this point. [Open a support ticket](https://supabase.com/dashboard/support/new?category=billing&subject=Transfer%20project).

## Usage-billing and project add-ons [\#](https://supabase.com/docs/guides/platform/project-transfer\#usage-billing-and-project-add-ons)

For usage metrics such as disk size, egress or image transformations and project add-ons such as [Compute Add-On](https://supabase.com/docs/guides/platform/compute-add-ons), [Point-In-Time-Recovery](https://supabase.com/docs/guides/platform/backups#point-in-time-recovery), [IPv4](https://supabase.com/docs/guides/platform/ipv4-address), [Log Drains](https://supabase.com/docs/guides/platform/log-drains), [Advanced MFA](https://supabase.com/docs/guides/auth/auth-mfa/phone) or a [Custom Domain](https://supabase.com/docs/guides/platform/custom-domains), the source organization will still be charged for the usage up until the transfer. The charges will be added to the invoice when the billing cycle resets.

The target organization will be charged at the end of the billing cycle for usage after the project transfer.

## Things to watch out for [\#](https://supabase.com/docs/guides/platform/project-transfer\#things-to-watch-out-for)

- Transferring a project might come with a short 1-2 minute downtime if you're moving a project from a paid to a Free Plan.
- You could lose access to certain project features depending on the plan of the target organization, i.e. moving a project from a Pro Plan to a Free Plan.
- When moving your project to a Free Plan, we also ensure you’re not exceeding your two free project limit. In these cases, it is best to upgrade your target organization to Pro Plan first.
- You could have less rights on the project depending on your role in the target organization, i.e. you were an Owner in the previous organization and only have a Read-Only role in the target organization.

## Transfer to a different region [\#](https://supabase.com/docs/guides/platform/project-transfer\#transfer-to-a-different-region)

Note that project transfers are only transferring your projects across an organization and cannot be used to transfer between different regions. To move your project to a different region, see [migrating your project](https://supabase.com/docs/guides/platform/migrating-and-upgrading-projects#migrate-your-project).

### Is this helpful?

NoYes

### On this page

[Pre-Requirements](https://supabase.com/docs/guides/platform/project-transfer#pre-requirements) [Usage-billing and project add-ons](https://supabase.com/docs/guides/platform/project-transfer#usage-billing-and-project-add-ons) [Things to watch out for](https://supabase.com/docs/guides/platform/project-transfer#things-to-watch-out-for) [Transfer to a different region](https://supabase.com/docs/guides/platform/project-transfer#transfer-to-a-different-region)

![Project Transfer: Confirmation Modal](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fproject-transfer-modal--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Project Transfer: General Settings](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fproject-transfer-overview--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings