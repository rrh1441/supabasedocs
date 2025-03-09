Integrations

# Vercel Marketplace

* * *

## Overview [\#](https://supabase.com/docs/guides/integrations/vercel-marketplace\#overview)

The Vercel Marketplace is a feature that allows you to manage third-party resources, such as Supabase, directly from the Vercel platform. This integration offers a seamless experience with unified billing, streamlined authentication, and easy access management for your team.

When you create an organization and projects through Vercel Marketplace, they function just like those created directly within Supabase. However, the billing is handled through your Vercel account, and you can manage your resources directly from the Vercel dashboard or CLI. Additionally, environment variables are automatically synchronized, making them immediately available for your connected projects.

For more information, see [Introducing the Vercel Marketplace](https://vercel.com/blog/introducing-the-vercel-marketplace) blog post.

Vercel Marketplace is currently in Public Alpha. If you encounter any issues or have feature requests, [contact support](https://supabase.com/dashboard/support/new).

## Quickstart [\#](https://supabase.com/docs/guides/integrations/vercel-marketplace\#quickstart)

### Via template [\#](https://supabase.com/docs/guides/integrations/vercel-marketplace\#via-template)

##### Deploy a Next.js app with Supabase Vercel Storage now

Uses the Next.js Supabase Starter Template

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fvercel%2Fnext.js%2Ftree%2Fcanary%2Fexamples%2Fhello-world)

### Via Vercel Marketplace [\#](https://supabase.com/docs/guides/integrations/vercel-marketplace\#via-vercel-marketplace)

Details coming soon..

### Connecting to Supabase project [\#](https://supabase.com/docs/guides/integrations/vercel-marketplace\#connecting-to-supabase-project)

Supabase Projects created via Vercel Marketplace are automatically synchronized with connected Vercel projects. This synchronization includes setting essential environment variables, such as:

`
POSTGRES_URL
POSTGRES_PRISMA_URL
POSTGRES_URL_NON_POOLING
POSTGRES_USER
POSTGRES_HOST
POSTGRES_PASSWORD
POSTGRES_DATABASE
SUPABASE_SERVICE_ROLE_KEY
SUPABASE_ANON_KEY
SUPABASE_URL
SUPABASE_JWT_SECRET
NEXT_PUBLIC_SUPABASE_ANON_KEY
NEXT_PUBLIC_SUPABASE_URL
`

These variables ensure your applications can connect securely to the database and interact with Supabase APIs.

## Studio support [\#](https://supabase.com/docs/guides/integrations/vercel-marketplace\#studio-support)

Accessing Supabase Studio is simple through the Vercel dashboard. You can open Supabase Studio from either the Integration installation page or the Vercel Storage page.
Depending on your entry point, you'll either land on the Supabase dashboard homepage or be redirected to the corresponding Supabase Project.

Supabase Studio provides tools such as:

- **SQL Editor:** Run SQL queries against your database.
- **Table Editor:** Create, edit, and delete tables and columns.
- **Log Explorer:** Inspect real-time logs for your database.
- **Postgres Upgrades:** Upgrade your Postgres instance to the latest version.
- **Compute Upgrades:** Scale the compute resources allocated to your database.

## Permissions [\#](https://supabase.com/docs/guides/integrations/vercel-marketplace\#permissions)

There is a direct one-to-one relationship between a Supabase Organization and a Vercel team. Installing the integration or launching your first Supabase Project through Vercel triggers the creation of a corresponding Supabase Organization if one doesn’t already exist.

When Vercel users interact with Supabase, they are automatically assigned Supabase accounts. New users get a Supabase account linked to their primary email, while existing users have their Vercel and Supabase accounts linked.

- The user who initiates the creation of a Vercel Storage database is assigned the `owner` role in the new Supabase organization.
- Subsequent users are assigned roles based on their Vercel role, such as `developer` for `member` and `owner` for `owner`.

Role management is handled directly in the Vercel dashboard, and changes are synchronized with Supabase.

Note: you can invite non-Vercel users to your Supabase Organization, but their permissions won't be synchronized with Vercel.

## Pricing [\#](https://supabase.com/docs/guides/integrations/vercel-marketplace\#pricing)

Pricing for databases created through Vercel Marketplace is identical to those created directly within Supabase. Detailed pricing information is available on the [Supabase pricing page](https://supabase.com/pricing).

The [usage page](https://supabase.com/dashboard/org/_/usage) tracks the usage of your Vercel databases, with this information sent to Vercel for billing, which appears on your Vercel invoice.

Note: Supabase Organization billing cycle is separate from Vercel's. Plan changes will reset the billing cycle to the day of the change, with the initial billing cycle starting the day you install the integration.

## Limitations [\#](https://supabase.com/docs/guides/integrations/vercel-marketplace\#limitations)

When using Vercel Marketplace, the following limitations apply:

- Projects can only be created or removed via the Vercel dashboard.
- Organizations cannot be removed manually; they are removed only if you uninstall the Vercel Marketplace Integration.
- Owners cannot be added manually within the Supabase dashboard.
- Invoices and payments must be managed through the Vercel dashboard, not the Supabase dashboard.

### Is this helpful?

NoYes

### On this page

[Overview](https://supabase.com/docs/guides/integrations/vercel-marketplace#overview) [Quickstart](https://supabase.com/docs/guides/integrations/vercel-marketplace#quickstart) [Via template](https://supabase.com/docs/guides/integrations/vercel-marketplace#via-template) [Via Vercel Marketplace](https://supabase.com/docs/guides/integrations/vercel-marketplace#via-vercel-marketplace) [Connecting to Supabase project](https://supabase.com/docs/guides/integrations/vercel-marketplace#connecting-to-supabase-project) [Studio support](https://supabase.com/docs/guides/integrations/vercel-marketplace#studio-support) [Permissions](https://supabase.com/docs/guides/integrations/vercel-marketplace#permissions) [Pricing](https://supabase.com/docs/guides/integrations/vercel-marketplace#pricing) [Limitations](https://supabase.com/docs/guides/integrations/vercel-marketplace#limitations)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings