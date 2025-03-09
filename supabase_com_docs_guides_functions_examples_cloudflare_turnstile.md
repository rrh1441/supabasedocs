Edge Functions

# CAPTCHA support with Cloudflare Turnstile

* * *

[Cloudflare Turnstile](https://www.cloudflare.com/products/turnstile/) is a friendly, free CAPTCHA replacement, and it works seamlessly with Supabase Edge Functions to protect your forms. [View on GitHub](https://github.com/supabase/supabase/tree/master/examples/edge-functions/supabase/functions/cloudflare-turnstile).

## Setup [\#](https://supabase.com/docs/guides/functions/examples/cloudflare-turnstile\#setup)

- Follow these steps to set up a new site: [https://developers.cloudflare.com/turnstile/get-started/](https://developers.cloudflare.com/turnstile/get-started/)
- Add the Cloudflare Turnstile widget to your site: [https://developers.cloudflare.com/turnstile/get-started/client-side-rendering/](https://developers.cloudflare.com/turnstile/get-started/client-side-rendering/)

## Code [\#](https://supabase.com/docs/guides/functions/examples/cloudflare-turnstile\#code)

Create a new function in your project:

`
supabase functions new cloudflare-turnstile
`

And add the code to the `index.ts` file:

index.ts

`
import { corsHeaders } from '../_shared/cors.ts'
console.log('Hello from Cloudflare Trunstile!')
function ips(req: Request) {
return req.headers.get('x-forwarded-for')?.split(/\s*,\s*/)
}
Deno.serve(async (req) => {
// This is needed if you're planning to invoke your function from a browser.
if (req.method === 'OPTIONS') {
    return new Response('ok', { headers: corsHeaders })
}
const { token } = await req.json()
const clientIps = ips(req) || ['']
const ip = clientIps[0]
// Validate the token by calling the
// "/siteverify" API endpoint.
let formData = new FormData()
formData.append('secret', Deno.env.get('CLOUDFLARE_SECRET_KEY') ?? '')
formData.append('response', token)
formData.append('remoteip', ip)
const url = 'https://challenges.cloudflare.com/turnstile/v0/siteverify'
const result = await fetch(url, {
    body: formData,
    method: 'POST',
})
const outcome = await result.json()
console.log(outcome)
if (outcome.success) {
    return new Response('success', { headers: corsHeaders })
}
return new Response('failure', { headers: corsHeaders })
})
`

## Deploy the server-side validation Edge Functions [\#](https://supabase.com/docs/guides/functions/examples/cloudflare-turnstile\#deploy-the-server-side-validation-edge-functions)

- [https://developers.cloudflare.com/turnstile/get-started/server-side-validation/](https://developers.cloudflare.com/turnstile/get-started/server-side-validation/)

`
supabase functions deploy cloudflare-turnstile
supabase secrets set CLOUDFLARE_SECRET_KEY=your_secret_key
`

## Invoke the function from your site [\#](https://supabase.com/docs/guides/functions/examples/cloudflare-turnstile\#invoke-the-function-from-your-site)

`
const { data, error } = await supabase.functions.invoke('cloudflare-turnstile', {
body: { token },
})
`

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2FOwW0znboh60%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Setup](https://supabase.com/docs/guides/functions/examples/cloudflare-turnstile#setup) [Code](https://supabase.com/docs/guides/functions/examples/cloudflare-turnstile#code) [Deploy the server-side validation Edge Functions](https://supabase.com/docs/guides/functions/examples/cloudflare-turnstile#deploy-the-server-side-validation-edge-functions) [Invoke the function from your site](https://supabase.com/docs/guides/functions/examples/cloudflare-turnstile#invoke-the-function-from-your-site)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings