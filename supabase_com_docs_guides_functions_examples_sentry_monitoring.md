Edge Functions

# Monitoring with Sentry

* * *

Add the [Sentry Deno SDK](https://docs.sentry.io/platforms/javascript/guides/deno/) to your Supabase Edge Functions to track exceptions and get notified of errors or performance issues.

### Prerequisites [\#](https://supabase.com/docs/guides/functions/examples/sentry-monitoring\#prerequisites)

- [Create a Sentry account](https://sentry.io/signup/).
- Make sure you have the latest version of the [Supabase CLI](https://supabase.com/docs/guides/cli#installation) installed.

### 1\. Create Supabase function [\#](https://supabase.com/docs/guides/functions/examples/sentry-monitoring\#1-create-supabase-function)

Create a new function locally:

`
supabase functions new sentryfied
`

### 2\. Add the Sentry Deno SDK [\#](https://supabase.com/docs/guides/functions/examples/sentry-monitoring\#2-add-the-sentry-deno-sdk)

Handle exceptions within your function and send them to Sentry.

``
import * as Sentry from 'https://deno.land/x/sentry/index.mjs'
Sentry.init({
// https://docs.sentry.io/product/sentry-basics/concepts/dsn-explainer/#where-to-find-your-dsn
dsn: SENTRY_DSN,
defaultIntegrations: false,
// Performance Monitoring
tracesSampleRate: 1.0,
// Set sampling rate for profiling - this is relative to tracesSampleRate
profilesSampleRate: 1.0,
})
// Set region and execution_id as custom tags
Sentry.setTag('region', Deno.env.get('SB_REGION'))
Sentry.setTag('execution_id', Deno.env.get('SB_EXECUTION_ID'))
Deno.serve(async (req) => {
try {
    const { name } = await req.json()
    // This will throw, as `name` in our example call will be `undefined`
    const data = {
      message: `Hello ${name}!`,
    }
    return new Response(JSON.stringify(data), { headers: { 'Content-Type': 'application/json' } })
} catch (e) {
    Sentry.captureException(e)
    return new Response(JSON.stringify({ msg: 'error' }), {
      status: 500,
      headers: { 'Content-Type': 'application/json' },
    })
}
})
``

### 3\. Deploy and test [\#](https://supabase.com/docs/guides/functions/examples/sentry-monitoring\#3-deploy-and-test)

Run function locally:

`
supabase start
supabase functions serve --no-verify-jwt
`

Test it: [http://localhost:54321/functions/v1/sentryfied](http://localhost:54321/functions/v1/sentryfied)

Deploy function to Supabase:

`
supabase functions deploy sentryfied --no-verify-jwt
`

### 4\. Try it yourself [\#](https://supabase.com/docs/guides/functions/examples/sentry-monitoring\#4-try-it-yourself)

Find the complete example on [GitHub](https://github.com/supabase/supabase/tree/master/examples/edge-functions/supabase/functions/sentryfied/index.ts).

## Working with scopes [\#](https://supabase.com/docs/guides/functions/examples/sentry-monitoring\#working-with-scopes)

Sentry Deno SDK currently do not support `Deno.serve` instrumentation, which means that there is no scope separation between requests.
Because of that, when the Edge Functions runtime is reused between multiple requests, all globally captured breadcrumbs and contextual data
will be shared, which is not the desired behavior. To work around this, all default integrations in the example code above are disabled,
and you should be relying on [`withScope`](https://docs.sentry.io/platforms/javascript/enriching-events/scopes/#using-withscope) to encapsulate
all Sentry SDK API calls, or [pass context directly](https://docs.sentry.io/platforms/javascript/enriching-events/context/#passing-context-directly)
to the `captureException` or `captureMessage` calls.

### Is this helpful?

NoYes

### On this page

[Prerequisites](https://supabase.com/docs/guides/functions/examples/sentry-monitoring#prerequisites) [1\. Create Supabase function](https://supabase.com/docs/guides/functions/examples/sentry-monitoring#1-create-supabase-function) [2\. Add the Sentry Deno SDK](https://supabase.com/docs/guides/functions/examples/sentry-monitoring#2-add-the-sentry-deno-sdk) [3\. Deploy and test](https://supabase.com/docs/guides/functions/examples/sentry-monitoring#3-deploy-and-test) [4\. Try it yourself](https://supabase.com/docs/guides/functions/examples/sentry-monitoring#4-try-it-yourself) [Working with scopes](https://supabase.com/docs/guides/functions/examples/sentry-monitoring#working-with-scopes)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings