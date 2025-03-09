Edge Functions

# CORS (Cross-Origin Resource Sharing) support for Invoking from the browser

* * *

To invoke edge functions from the browser, you need to handle [CORS Preflight](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request) requests.

See the [example on GitHub](https://github.com/supabase/supabase/blob/master/examples/edge-functions/supabase/functions/browser-with-cors/index.ts).

### Recommended setup [\#](https://supabase.com/docs/guides/functions/cors\#recommended-setup)

We recommend adding a `cors.ts` file within a [`_shared` folder](https://supabase.com/docs/guides/functions/quickstart#organizing-your-edge-functions) which makes it easy to reuse the CORS headers across functions:

cors.ts

`
export const corsHeaders = {
'Access-Control-Allow-Origin': '*',
'Access-Control-Allow-Headers': 'authorization, x-client-info, apikey, content-type',
}
`

You can then import and use the CORS headers within your functions:

index.ts

``
import { corsHeaders } from '../_shared/cors.ts'
console.log(`Function "browser-with-cors" up and running!`)
Deno.serve(async (req) => {
// This is needed if you're planning to invoke your function from a browser.
if (req.method === 'OPTIONS') {
    return new Response('ok', { headers: corsHeaders })
}
try {
    const { name } = await req.json()
    const data = {
      message: `Hello ${name}!`,
    }
    return new Response(JSON.stringify(data), {
      headers: { ...corsHeaders, 'Content-Type': 'application/json' },
      status: 200,
    })
} catch (error) {
    return new Response(JSON.stringify({ error: error.message }), {
      headers: { ...corsHeaders, 'Content-Type': 'application/json' },
      status: 400,
    })
}
})
``

### Is this helpful?

NoYes

### On this page

[Recommended setup](https://supabase.com/docs/guides/functions/cors#recommended-setup)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings