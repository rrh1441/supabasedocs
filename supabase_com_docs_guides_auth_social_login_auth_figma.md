Auth

# Login with Figma

* * *

To enable Figma Auth for your project, you need to set up a Figma OAuth application and add the application credentials to your Supabase Dashboard.

## Overview [\#](https://supabase.com/docs/guides/auth/social-login/auth-figma\#overview)

Setting up Figma logins for your application consists of 3 parts:

- Create and configure a Figma App on the [Figma Developers page](https://www.figma.com/developers).
- Add your Figma `client_id` and `client_secret` to your [Supabase Project](https://app.supabase.com/).
- Add the login code to your [Supabase JS Client App](https://github.com/supabase/supabase-js).

## Access the Figma Developers page [\#](https://supabase.com/docs/guides/auth/social-login/auth-figma\#access-the-figma-developers-page)

- Go to the [Figma Developers page](https://www.figma.com/developers)
- Click on `My apps` at the top right
- Log in (if necessary)

![Figma Developers page](https://supabase.com/docs/img/guides/auth-figma/figma_developers_page.png)

## Find your callback URL [\#](https://supabase.com/docs/guides/auth/social-login/auth-figma\#find-your-callback-url)

The next step requires a callback URL, which looks like this: `https://<project-ref>.supabase.co/auth/v1/callback`

- Go to your [Supabase Project Dashboard](https://supabase.com/dashboard)
- Click on the `Authentication` icon in the left sidebar
- Click on [`Providers`](https://supabase.com/dashboard/project/_/auth/providers) under the Configuration section
- Click on **Figma** from the accordion list to expand and you'll find your **Callback URL**, you can click `Copy` to copy it to the clipboard

For testing OAuth locally with the Supabase CLI please see the [local development docs](https://supabase.com/docs/guides/cli/local-development#use-auth-locally).

## Create a Figma OAuth app [\#](https://supabase.com/docs/guides/auth/social-login/auth-figma\#create-a-figma-oauth-app)

- Enter your `App name`, `Website URL` and upload your app logo
- Click on `Add callback`
- Add your `Callback URL`
- Click on `Save`

![Create Figma app](https://supabase.com/docs/img/guides/auth-figma/figma_create_app.png)

- Copy and save your newly-generated `Client ID`
- Copy and save your newly-generated `Client Secret`

![Get Figma app credentials](https://supabase.com/docs/img/guides/auth-figma/figma_app_credentials.png)

## Enter your Figma credentials into your Supabase project [\#](https://supabase.com/docs/guides/auth/social-login/auth-figma\#enter-your-figma-credentials-into-your-supabase-project)

- Go to your [Supabase Project Dashboard](https://supabase.com/dashboard)
- In the left sidebar, click the `Authentication` icon (near the top)
- Click on [`Providers`](https://supabase.com/dashboard/project/_/auth/providers) under the Configuration section
- Click on **Figma** from the accordion list to expand and turn **Figma Enabled** to ON
- Enter your **Figma Client ID** and **Figma Client Secret** saved in the previous step
- Click `Save`

## Add login code to your client app [\#](https://supabase.com/docs/guides/auth/social-login/auth-figma\#add-login-code-to-your-client-app)

JavaScriptFlutterKotlin

Make sure you're using the right `supabase` client in the following code.

If you're not using Server-Side Rendering or cookie-based Auth, you can directly use the `createClient` from `@supabase/supabase-js`. If you're using Server-Side Rendering, see the [Server-Side Auth guide](https://supabase.com/docs/guides/auth/server-side/creating-a-client) for instructions on creating your Supabase client.

When your user signs in, call [`signInWithOAuth()`](https://supabase.com/docs/reference/javascript/auth-signinwithoauth) with `figma` as the `provider`:

`
async function signInWithFigma() {
const { data, error } = await supabase.auth.signInWithOAuth({
    provider: 'figma',
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

## Resources [\#](https://supabase.com/docs/guides/auth/social-login/auth-figma\#resources)

- [Supabase - Get started for free](https://supabase.com/)
- [Supabase JS Client](https://github.com/supabase/supabase-js)
- [Figma Developers page](https://www.figma.com/developers)

### Is this helpful?

NoYes

### On this page

[Overview](https://supabase.com/docs/guides/auth/social-login/auth-figma#overview) [Access the Figma Developers page](https://supabase.com/docs/guides/auth/social-login/auth-figma#access-the-figma-developers-page) [Find your callback URL](https://supabase.com/docs/guides/auth/social-login/auth-figma#find-your-callback-url) [Create a Figma OAuth app](https://supabase.com/docs/guides/auth/social-login/auth-figma#create-a-figma-oauth-app) [Enter your Figma credentials into your Supabase project](https://supabase.com/docs/guides/auth/social-login/auth-figma#enter-your-figma-credentials-into-your-supabase-project) [Add login code to your client app](https://supabase.com/docs/guides/auth/social-login/auth-figma#add-login-code-to-your-client-app) [Resources](https://supabase.com/docs/guides/auth/social-login/auth-figma#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings