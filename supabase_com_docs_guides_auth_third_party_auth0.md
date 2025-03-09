Auth

# Auth0

## Use Auth0 with your Supabase project

* * *

Auth0 can be used as a third-party authentication provider alongside Supabase Auth, or standalone, with your Supabase project.

## Getting started [\#](https://supabase.com/docs/guides/auth/third-party/auth0\#getting-started)

1. First you need to add an integration to connect your Supabase project with your Auth0 tenant. You will need your tenant ID (and in some cases region ID).
2. Add a new Third-party Auth integration in your project's [Authentication settings](https://supabase.com/dashboard/project/_/settings/auth).
3. Assign the `role: 'authenticated'` custom claim to all JWTs by using an Auth0 Action.
4. Finally setup the Supabase client in your application.

## Setup the Supabase client library [\#](https://supabase.com/docs/guides/auth/third-party/auth0\#setup-the-supabase-client-library)

TypeScriptSwift (iOS)FlutterKotlin

`
import { createClient } from '@supabase/supabase-js'
import Auth0Client from '@auth0/auth0-spa-js'
const auth0 = new Auth0Client({
domain: '<AUTH0_DOMAIN>',
clientId: '<AUTH0_CLIENT_ID>',
authorizationParams: {
    redirect_uri: '<MY_CALLBACK_URL>',
},
})
const supabase = createClient('https://<supabase-project>.supabase.co', 'SUPABASE_ANON_KEY', {
accessToken: async () => {
    const accessToken = await auth0.getTokenSilently()
    // Alternatively you can use (await auth0.getIdTokenClaims()).__raw to
    // use an ID token instead.
    return accessToken
},
})
`

## Add a new Third-Party Auth integration to your project [\#](https://supabase.com/docs/guides/auth/third-party/auth0\#add-a-new-third-party-auth-integration-to-your-project)

In the dashboard navigate to your project's [Authentication settings](https://supabase.com/dashboard/project/_/settings/auth) and find the Third-Party Auth section to add a new integration.

In the CLI add the following config to your `supabase/config.toml` file:

`
[auth.third_party.auth0]
enabled = true
tenant = "<id>"
tenant_region = "<region>" # if your tenant has a region
`

## Use an Auth0 Action to assign the authenticated role [\#](https://supabase.com/docs/guides/auth/third-party/auth0\#use-an-auth0-action-to-assign-the-authenticated-role)

Your Supabase project inspects the `role` claim present in all JWTs sent to it, to assign the correct Postgres role when using the Data API, Storage or Realtime authorization.

By default, Auth0 JWTs (both access token and ID token) do not contain a `role` claim in them. If you were to send such a JWT to your Supabase project, the `anon` role would be assigned when executing the Postgres query. Most of your app's logic will be accessible by the `authenticated` role.

A recommended approach to do this is to configure the [`onExecutePostLogin` Auth0 Action](https://auth0.com/docs/secure/tokens/json-web-tokens/create-custom-claims#create-custom-claims) which will add the custom claim:

`
exports.onExecutePostLogin = async (event, api) => {
api.accessToken.setCustomClaim('role', 'authenticated')
}
`

## Limitations [\#](https://supabase.com/docs/guides/auth/third-party/auth0\#limitations)

At this time, Auth0 tenants with the following [signing algorithms](https://auth0.com/docs/get-started/applications/signing-algorithms) are not supported:

- HS256 (HMAC with SHA-256) -- also known as symmetric JWTs
- PS256 (RSA-PSS with SHA-256)

### Is this helpful?

NoYes

### On this page

[Getting started](https://supabase.com/docs/guides/auth/third-party/auth0#getting-started) [Setup the Supabase client library](https://supabase.com/docs/guides/auth/third-party/auth0#setup-the-supabase-client-library) [Add a new Third-Party Auth integration to your project](https://supabase.com/docs/guides/auth/third-party/auth0#add-a-new-third-party-auth-integration-to-your-project) [Use an Auth0 Action to assign the authenticated role](https://supabase.com/docs/guides/auth/third-party/auth0#use-an-auth0-action-to-assign-the-authenticated-role) [Limitations](https://supabase.com/docs/guides/auth/third-party/auth0#limitations)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings