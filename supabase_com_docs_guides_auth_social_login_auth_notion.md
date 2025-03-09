Auth

# Login with Notion

* * *

To enable Notion Auth for your project, you need to set up a Notion Application and add the Application OAuth credentials to your Supabase Dashboard.

## Overview [\#](https://supabase.com/docs/guides/auth/social-login/auth-notion\#overview)

Setting up Notion logins for your application consists of 3 parts:

- Create and configure a Notion Application [Notion Developer Portal](https://www.notion.so/my-integrations)
- Retrieve your OAuth client ID and OAuth client secret and add them to your [Supabase Project](https://supabase.com/dashboard)
- Add the login code to your [Supabase JS Client App](https://github.com/supabase/supabase-js)

## Create your notion integration [\#](https://supabase.com/docs/guides/auth/social-login/auth-notion\#create-your-notion-integration)

- Go to [developers.notion.com](https://developers.notion.com/).

- Click "View my integrations" and login.
![notion.so](https://supabase.com/docs/img/guides/auth-notion/notion.png)

- Once logged in, go to [notion.so/my-integrations](https://notion.so/my-integrations) and create a new integration.

- When creating your integration, ensure that you select "Public integration" under "Integration type" and "Read user information including email addresses" under "Capabilities".

- You will need to add a redirect URI, see [Add the redirect URI](https://supabase.com/docs/guides/auth/social-login/auth-notion#add-the-redirect-uri)

- Once you've filled in the necessary fields, click "Submit" to finish creating the integration.


![notion.so](https://supabase.com/docs/img/guides/auth-notion/notion-developer.png)

## Add the redirect URI [\#](https://supabase.com/docs/guides/auth/social-login/auth-notion\#add-the-redirect-uri)

- After selecting "Public integration", you should see an option to add "Redirect URIs".

![notion.so](https://supabase.com/docs/img/guides/auth-notion/notion-redirect-uri.png)

The next step requires a callback URL, which looks like this: `https://<project-ref>.supabase.co/auth/v1/callback`

- Go to your [Supabase Project Dashboard](https://supabase.com/dashboard)
- Click on the `Authentication` icon in the left sidebar
- Click on [`Providers`](https://supabase.com/dashboard/project/_/auth/providers) under the Configuration section
- Click on **Notion** from the accordion list to expand and you'll find your **Callback URL**, you can click `Copy` to copy it to the clipboard

For testing OAuth locally with the Supabase CLI please see the [local development docs](https://supabase.com/docs/guides/cli/local-development#use-auth-locally).

## Add your Notion credentials into your Supabase project [\#](https://supabase.com/docs/guides/auth/social-login/auth-notion\#add-your-notion-credentials-into-your-supabase-project)

- Once you've created your notion integration, you should be able to retrieve the "OAuth client ID" and "OAuth client secret" from the "OAuth Domain and URIs" tab.

![notion.so](https://supabase.com/docs/img/guides/auth-notion/notion-creds.png)

- Go to your [Supabase Project Dashboard](https://supabase.com/dashboard)
- In the left sidebar, click the `Authentication` icon (near the top)
- Click on [`Providers`](https://supabase.com/dashboard/project/_/auth/providers) under the Configuration section
- Click on **Notion** from the accordion list to expand and turn **Notion Enabled** to ON
- Enter your **Notion Client ID** and **Notion Client Secret** saved in the previous step
- Click `Save`

## Add login code to your client app [\#](https://supabase.com/docs/guides/auth/social-login/auth-notion\#add-login-code-to-your-client-app)

JavaScriptFlutterKotlin

Make sure you're using the right `supabase` client in the following code.

If you're not using Server-Side Rendering or cookie-based Auth, you can directly use the `createClient` from `@supabase/supabase-js`. If you're using Server-Side Rendering, see the [Server-Side Auth guide](https://supabase.com/docs/guides/auth/server-side/creating-a-client) for instructions on creating your Supabase client.

When your user signs in, call [`signInWithOAuth()`](https://supabase.com/docs/reference/javascript/auth-signinwithoauth) with `notion` as the `provider`:

`
async function signInWithNotion() {
const { data, error } = await supabase.auth.signInWithOAuth({
    provider: 'notion',
})
}
`

For a PKCE flow, for example in Server-Side Auth, you need an extra step to handle the code exchange. When calling `signInWithOAuth`, provide a `redirectTo` URL which points to a callback route. This redirect URL should be added to your [redirect allow list](https://supabase.com/docs/guides/auth/redirect-urls).

ClientServer

In the browser, `signInWithOAuth` automatically redirects to the OAuth provider's authentication endpoint, which then redirects to your endpoint.

``
await supabase.auth.signInWithOAuth({
provider,
options: {
    redirectTo: `http://example.com/auth/callback`,
},
})
``

At the callback endpoint, handle the code exchange to save the user session.

Next.jsSvelteKitAstroRemixExpress

Create a new file at `app/auth/callback/route.ts` and populate with the following:

app/auth/callback/route.ts

``
import { NextResponse } from 'next/server'
// The client you created from the Server-Side Auth instructions
import { createClient } from '@/utils/supabase/server'
export async function GET(request: Request) {
const { searchParams, origin } = new URL(request.url)
const code = searchParams.get('code')
// if "next" is in param, use it as the redirect URL
const next = searchParams.get('next') ?? '/'
if (code) {
    const supabase = await createClient()
    const { error } = await supabase.auth.exchangeCodeForSession(code)
    if (!error) {
      const forwardedHost = request.headers.get('x-forwarded-host') // original origin before load balancer
      const isLocalEnv = process.env.NODE_ENV === 'development'
      if (isLocalEnv) {
        // we can be sure that there is no load balancer in between, so no need to watch for X-Forwarded-Host
        return NextResponse.redirect(`${origin}${next}`)
      } else if (forwardedHost) {
        return NextResponse.redirect(`https://${forwardedHost}${next}`)
      } else {
        return NextResponse.redirect(`${origin}${next}`)
      }
    }
}
// return the user to an error page with instructions
return NextResponse.redirect(`${origin}/auth/auth-code-error`)
}
``

JavaScriptFlutterKotlin

When your user signs out, call [signOut()](https://supabase.com/docs/reference/javascript/auth-signout) to remove them from the browser session and any objects from localStorage:

`
async function signOut() {
const { error } = await supabase.auth.signOut()
}
`

## Resources [\#](https://supabase.com/docs/guides/auth/social-login/auth-notion\#resources)

- [Supabase - Get started for free](https://supabase.com/)
- [Supabase JS Client](https://github.com/supabase/supabase-js)
- [Notion Account](https://notion.so/)
- [Notion Developer Portal](https://www.notion.so/my-integrations)

### Is this helpful?

NoYes

### On this page

[Overview](https://supabase.com/docs/guides/auth/social-login/auth-notion#overview) [Create your notion integration](https://supabase.com/docs/guides/auth/social-login/auth-notion#create-your-notion-integration) [Add the redirect URI](https://supabase.com/docs/guides/auth/social-login/auth-notion#add-the-redirect-uri) [Add your Notion credentials into your Supabase project](https://supabase.com/docs/guides/auth/social-login/auth-notion#add-your-notion-credentials-into-your-supabase-project) [Add login code to your client app](https://supabase.com/docs/guides/auth/social-login/auth-notion#add-login-code-to-your-client-app) [Resources](https://supabase.com/docs/guides/auth/social-login/auth-notion#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings