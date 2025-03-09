Home

# Maturity Model

* * *

Supabase is great for building something very fast _and_ for scaling up. However, it's important to note that as your application matures and your team expands, the practices you use for managing an application in production should not be the same as the practices you used for prototyping.

## Prototyping [\#](https://supabase.com/docs/guides/deployment/maturity-model\#prototyping)

The Dashboard is a quick and easy tool for building applications while you are prototyping. That said, we strongly recommend using [Migrations](https://supabase.com/docs/guides/deployment/database-migrations) to manage your database changes. You can use our CLI to [capture any changes](https://supabase.com/docs/reference/cli/supabase-db-diff) you have made on the Dashboard so that you can commit them a version control system, like git.

## Collaborating [\#](https://supabase.com/docs/guides/deployment/maturity-model\#collaborating)

As soon as you start collaborating with team members, all project changes should be in version control. At this point we strongly recommend moving away from using the Dashboard for schema changes. Use migrations to manage your database, and check them into your version control system to track every change.

Resources:

- [Database migrations](https://supabase.com/docs/guides/deployment/database-migrations)
- [Managing access on the Dashboard](https://supabase.com/docs/guides/platform/access-control)
- [PGAudit for Postgres](https://supabase.com/docs/guides/database/extensions/pgaudit)

## In production [\#](https://supabase.com/docs/guides/deployment/maturity-model\#in-production)

Once your application is live, you should never change your database using the Dashboard - everything should be done with [Migrations](https://supabase.com/docs/guides/cli/managing-environments#create-a-new-migration). Some other important things to consider at this point include:

- The Dashboard has various [access levels](https://supabase.com/docs/guides/platform/access-control) that can prevent changes being made via the UI.
- Design a [safe workflow](https://supabase.com/docs/guides/platform/shared-responsibility-model#you-decide-your-own-workflow) for managing your database. We strongly recommend running [multiple environments](https://supabase.com/docs/guides/cli/managing-environments) as part of your development workflow ( `local` -\> `staging` -\> `prod`).
- Do not share any production passwords with your team, _especially_ your `postgres` password. All changes should be made via version-controlled migrations which run via a bastion host or a CI platform (like [GitHub Actions](https://supabase.com/docs/guides/cli/managing-environments#configure-github-actions). If you use GitHub Actions, use [approval workflows](https://docs.github.com/en/actions/managing-workflow-runs/reviewing-deployments) to prevent any migrations being run accidentally.
- Restrict production access to your database using [Network Restrictions](https://supabase.com/docs/guides/platform/network-restrictions).
- As your database to grows, we strongly recommend moving to [Point-in-Time Recovery](https://supabase.com/docs/guides/platform/backups#point-in-time-recovery). This is safer and has less impact on your database performance during maintenance windows.
- Read the [Production Checklist](https://supabase.com/docs/guides/platform/going-into-prod) and familiarize your team with the [Shared Responsibilities](https://supabase.com/docs/guides/platform/shared-responsibility-model) between your organization and Supabase.

Resources:

- [Database migrations](https://supabase.com/docs/guides/deployment/database-migrations)
- [Managing access on the Dashboard](https://supabase.com/docs/guides/platform/access-control)
- [PGAudit for Postgres](https://supabase.com/docs/guides/database/extensions/pgaudit)
- [Managing environments](https://supabase.com/docs/guides/cli/managing-environments)

## Enterprise [\#](https://supabase.com/docs/guides/deployment/maturity-model\#enterprise)

For a more secure setup, consider running your workload across several organizations. It's a common pattern to have a Production organization which is restricted to only those team members who are qualified to have direct access to production databases.

Reach out to [growth](https://forms.supabase.com/enterprise) if you need help designing a secure development workflow for your organization.

### Is this helpful?

NoYes

### On this page

[Prototyping](https://supabase.com/docs/guides/deployment/maturity-model#prototyping) [Collaborating](https://supabase.com/docs/guides/deployment/maturity-model#collaborating) [In production](https://supabase.com/docs/guides/deployment/maturity-model#in-production) [Enterprise](https://supabase.com/docs/guides/deployment/maturity-model#enterprise)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings