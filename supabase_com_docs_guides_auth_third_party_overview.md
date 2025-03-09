Auth

# Third-party auth

## First-class support for authentication providers

* * *

Supabase has first-class support for these third-party authentication providers:

- [Firebase Auth](https://supabase.com/docs/guides/auth/third-party/firebase-auth)
- [Auth0](https://supabase.com/docs/guides/auth/third-party/auth0)
- [AWS Cognito (with or without AWS Amplify)](https://supabase.com/docs/guides/auth/third-party/aws-cognito)

You can use these providers alongside Supabase Auth, or on their own, to access the [Data API (REST and GraphQL)](https://supabase.com/docs/guides/database), [Storage](https://supabase.com/docs/guides/storage), [Realtime](https://supabase.com/docs/guides/storage) and [Functions](https://supabase.com/docs/guides/functions) from your existing apps.

If you already have production apps using one of these authentication providers, and would like to use a Supabase feature, you no longer need to migrate your users to Supabase Auth or use workarounds like translating JWTs into the Supabase Auth format and using your project's signing secret.

## How does it work? [\#](https://supabase.com/docs/guides/auth/third-party/overview\#how-does-it-work)

To use Supabase products like Data APIs for your Postgres database, Storage or Realtime, you often need to send access tokens or JWTs via the Supabase client libraries or via the REST API. Third-party auth support means that when you add a new integration with one of these providers, the API will trust JWTs issued by the provider similar to how it trusts JWTs issued by Supabase Auth.

This is made possible if the providers are using JWTs signed with asymmetric keys, which means that the Supabase APIs will be able to only verify but not create JWTs.

## Limitations [\#](https://supabase.com/docs/guides/auth/third-party/overview\#limitations)

There are some limitations you should be aware of when using third-party authentication providers with Supabase.

1. The third-party provider must use asymmetrically signed JWTs (exposed as an OIDC Issuer Discovery URL by the third-party authentication provider). Using symmetrically signed JWTs is not possible at this time.
2. The JWT signing keys from the third-party provider are stored in the configuration of your project, and are checked for changes periodically. If you are rotating your keys (when supported) allow up to 30 minutes for the change to be picked up.
3. It is not possible to disable Supabase Auth at this time.

## Pricing [\#](https://supabase.com/docs/guides/auth/third-party/overview\#pricing)

$0.00325 per Third-Party MAU. You are only charged for usage exceeding your subscription plan's quota.

For a detailed breakdown of how charges are calculated, refer to [Manage Monthly Active Third-Party Users usage](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-third-party).

### Is this helpful?

NoYes

### On this page

[How does it work?](https://supabase.com/docs/guides/auth/third-party/overview#how-does-it-work) [Limitations](https://supabase.com/docs/guides/auth/third-party/overview#limitations) [Pricing](https://supabase.com/docs/guides/auth/third-party/overview#pricing)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings