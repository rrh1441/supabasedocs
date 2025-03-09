Edge Functions

# Deploy to Production

## Deploy your Edge Functions to your remote Supabase Project.

* * *

Once you have developed your Edge Functions locally, you can deploy them to your Supabase project.

## Login to the CLI [\#](https://supabase.com/docs/guides/functions/deploy\#login-to-the-cli)

Log in to the Supabase CLI if necessary:

`
supabase login
`

##### CLI not installed?

See the [CLI Docs](https://supabase.com/docs/guides/cli) to learn how to install the Supabase CLI on your local machine.

## Get your project ID [\#](https://supabase.com/docs/guides/functions/deploy\#get-your-project-id)

Get the project ID associated with your function by running:

`
supabase projects list
`

##### Need a new project?

If you haven't yet created a Supabase project, you can do so by visiting [database.new](https://database.new/).

## Link your local project [\#](https://supabase.com/docs/guides/functions/deploy\#link-your-local-project)

[Link](https://supabase.com/docs/reference/cli/usage#supabase-link) your local project to your remote Supabase project using the ID you just retrieved:

`
supabase link --project-ref your-project-id
`

## Deploy your Edge Functions [\#](https://supabase.com/docs/guides/functions/deploy\#deploy-your-edge-functions)

##### Docker required

Since Supabase CLI version 1.123.4, you must have [Docker Desktop](https://docs.docker.com/desktop/) installed to deploy Edge Functions.

You can deploy all of your Edge Functions with a single command:

`
supabase functions deploy
`

You can deploy individual Edge Functions by specifying the name of the function in the deploy command:

`
supabase functions deploy hello-world
`

By default, Edge Functions require a valid JWT in the authorization header. If you want to use Edge Functions without Authorization checks (commonly used for Stripe webhooks), you can pass the `--no-verify-jwt` flag when deploying your Edge Functions.

`
supabase functions deploy hello-world --no-verify-jwt
`

Be careful when using this flag, as it will allow anyone to invoke your Edge Function without a valid JWT. The Supabase client libraries automatically handle authorization.

## Invoking remote functions [\#](https://supabase.com/docs/guides/functions/deploy\#invoking-remote-functions)

You can now invoke your Edge Function using the project's `ANON_KEY`, which can be found in the [API settings](https://supabase.com/dashboard/project/_/settings/api) of the Supabase Dashboard.

cURL

JavaScript

`
curl --request POST 'https://<project_id>.supabase.co/functions/v1/hello-world' \
  --header 'Authorization: Bearer ANON_KEY' \
  --header 'Content-Type: application/json' \
  --data '{ "name":"Functions" }'
`

You should receive the response `{ "message":"Hello Functions!" }`.

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2F5OWH9c4u68M%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Login to the CLI](https://supabase.com/docs/guides/functions/deploy#login-to-the-cli) [Get your project ID](https://supabase.com/docs/guides/functions/deploy#get-your-project-id) [Link your local project](https://supabase.com/docs/guides/functions/deploy#link-your-local-project) [Deploy your Edge Functions](https://supabase.com/docs/guides/functions/deploy#deploy-your-edge-functions) [Invoking remote functions](https://supabase.com/docs/guides/functions/deploy#invoking-remote-functions)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings