Auth

# Amazon Cognito (Amplify)

## Use Amazon Cognito via Amplify or standalone with your Supabase project

* * *

Amazon Cognito User Pools (via AWS Amplify or on its own) can be used as a third-party authentication provider alongside Supabase Auth, or standalone, with your Supabase project.

## Getting started [\#](https://supabase.com/docs/guides/auth/third-party/aws-cognito\#getting-started)

1. First you need to add an integration to connect your Supabase project with your Amazon Cognito User Pool. You will need the pool's ID and region.
2. Add a new Third-party Auth integration in your project's [Authentication settings](https://supabase.com/dashboard/project/_/settings/auth) or configure it in the CLI.
3. Assign the `role: 'authenticated'` custom claim to all JWTs by using a Pre-Token Generation Trigger.
4. Finally setup the Supabase client in your application.

## Setup the Supabase client library [\#](https://supabase.com/docs/guides/auth/third-party/aws-cognito\#setup-the-supabase-client-library)

TypeScript (Amplify)Swift (iOS)FlutterKotlin

`
import { fetchAuthSession, Hub } from 'aws-amplify/auth'
const supabase = createClient('https://<supabase-project>.supabase.co', 'SUPABASE_ANON_KEY', {
accessToken: async () => {
    const tokens = await fetchAuthSession()
    // Alternatively you can use tokens?.idToken instead.
    return tokens?.accessToken
},
})
// if you're using Realtime you also need to set up a listener for Cognito auth changes
Hub.listen('auth', () => {
fetchAuthSession().then((tokens) => supabase.realtime.setAuth(tokens?.accessToken))
})
`

## Add a new Third-Party Auth integration to your project [\#](https://supabase.com/docs/guides/auth/third-party/aws-cognito\#add-a-new-third-party-auth-integration-to-your-project)

In the dashboard navigate to your project's [Authentication settings](https://supabase.com/dashboard/project/_/settings/auth) and find the Third-Party Auth section to add a new integration.

In the CLI add the following config to your `supabase/config.toml` file:

`
[auth.third_party.aws_cognito]
enabled = true
user_pool_id = "<id>"
user_pool_region = "<region>"
`

## Use a pre-token generation trigger to assign the authenticated role [\#](https://supabase.com/docs/guides/auth/third-party/aws-cognito\#use-a-pre-token-generation-trigger-to-assign-the-authenticated-role)

Your Supabase project inspects the `role` claim present in all JWTs sent to it, to assign the correct Postgres role when using the Data API, Storage or Realtime authorization.

By default, Amazon Cognito JWTs (both ID token and access tokens) do not contain a `role` claim in them. If you were to send such a JWT to your Supabase project, the `anon` role would be assigned when executing the Postgres query. Most of your app's logic will be accessible by the `authenticated` role.

A recommended approach to do this is to configure a [Pre-Token Generation Trigger](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-pre-token-generation.html) either `V1_0` (ID token only) or `V2_0` (both access and ID token). To do this you will need to create a new Lambda function (in any language and runtime) and assign it to the [Amazon Cognito User Pool's Lambda Triggers configuration](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html). For example, the Lambda function should look similar to this:

Node.js

`
export const handler = async (event) => {
event.response = {
    claimsOverrideDetails: {
      claimsToAddOrOverride: {
        role: 'authenticated',
      },
    },
}
return event
}
`

### Is this helpful?

NoYes

### On this page

[Getting started](https://supabase.com/docs/guides/auth/third-party/aws-cognito#getting-started) [Setup the Supabase client library](https://supabase.com/docs/guides/auth/third-party/aws-cognito#setup-the-supabase-client-library) [Add a new Third-Party Auth integration to your project](https://supabase.com/docs/guides/auth/third-party/aws-cognito#add-a-new-third-party-auth-integration-to-your-project) [Use a pre-token generation trigger to assign the authenticated role](https://supabase.com/docs/guides/auth/third-party/aws-cognito#use-a-pre-token-generation-trigger-to-assign-the-authenticated-role)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings