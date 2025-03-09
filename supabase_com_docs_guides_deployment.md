Home

# Deployment

* * *

Deploying your app makes it live and accessible to users. Usually, you will deploy an app to at least two environments: a production environment for users and (one or multiple) staging or preview environments for developers.

Supabase provides several options for environment management and deployment.

## Environment management [\#](https://supabase.com/docs/guides/deployment\#environment-management)

You can maintain separate development, staging, and production environments for Supabase:

- **Development**: Develop with a local Supabase stack using the [Supabase CLI](https://supabase.com/docs/guides/local-development).
- **Staging**: Use [branching](https://supabase.com/docs/guides/deployment/branching) to create staging or preview environments. You can use persistent branches for a long-lived staging setup, or ephemeral branches for short-lived previews (which are often tied to a pull request).
- **Production**: If you have branching enabled, you can use the Supabase GitHub integration to automatically push your migration files when you merge a pull request. Alternatively, you can set up your own continuous deployment pipeline using the Supabase CLI.

##### Self-hosting

See the [self-hosting guides](https://supabase.com/docs/guides/self-hosting) for instructions on hosting your own Supabase stack.

## Deployment [\#](https://supabase.com/docs/guides/deployment\#deployment)

You can automate deployments using:

- The [Supabase GitHub integration](https://supabase.com/dashboard/project/_/settings/integrations) (with branching enabled)
- The [Supabase CLI](https://supabase.com/docs/guides/local-development) in your own continuous deployment pipeline
- The [Supabase Terraform provider](https://supabase.com/docs/guides/deployment/terraform)

### Is this helpful?

NoYes

### On this page

[Environment management](https://supabase.com/docs/guides/deployment#environment-management) [Deployment](https://supabase.com/docs/guides/deployment#deployment)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings