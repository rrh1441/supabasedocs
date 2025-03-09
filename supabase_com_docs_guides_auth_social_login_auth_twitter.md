Auth

# Login with Twitter

* * *

To enable Twitter Auth for your project, you need to set up a Twitter OAuth application and add the application credentials in the Supabase Dashboard.

## Overview [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitter\#overview)

Setting up Twitter logins for your application consists of 3 parts:

- Create and configure a Twitter Project and App on the [Twitter Developer Dashboard](https://developer.twitter.com/en/portal/dashboard).
- Add your Twitter `API Key` and `API Secret Key` to your [Supabase Project](https://supabase.com/dashboard).
- Add the login code to your [Supabase JS Client App](https://github.com/supabase/supabase-js).

## Access your Twitter Developer account [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitter\#access-your-twitter-developer-account)

- Go to [developer.twitter.com](https://developer.twitter.com/).
- Click on `Sign in` at the top right to log in.

![Twitter Developer Portal.](https://supabase.com/docs/img/guides/auth-twitter/twitter-portal.png)

## Find your callback URL [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitter\#find-your-callback-url)

The next step requires a callback URL, which looks like this: `https://<project-ref>.supabase.co/auth/v1/callback`

- Go to your [Supabase Project Dashboard](https://supabase.com/dashboard)
- Click on the `Authentication` icon in the left sidebar
- Click on [`Providers`](https://supabase.com/dashboard/project/_/auth/providers) under the Configuration section
- Click on **Twitter** from the accordion list to expand and you'll find your **Callback URL**, you can click `Copy` to copy it to the clipboard

For testing OAuth locally with the Supabase CLI please see the [local development docs](https://supabase.com/docs/guides/cli/local-development#use-auth-locally).

## Create a Twitter OAuth app [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitter\#create-a-twitter-oauth-app)

- Click `+ Create Project`.
  - Enter your project name, click `Next`.
  - Select your use case, click `Next`.
  - Enter a description for your project, click `Next`.
  - Enter a name for your app, click `Next`.
  - Copy and save your `API Key` (this is your `client_id`).
  - Copy and save your `API Secret Key` (this is your `client_secret`).
  - Click on `App settings` to proceed to next steps.
- At the bottom, you will find `User authentication settings`. Click on `Set up`.
- Under `User authentication settings`, you can configure `App permissions`.
- Make sure you turn ON `Request email from users`.
- Select `Web App...` as the `Type of App`.
- Under `App info` configure the following.
  - Enter your `Callback URL`. Check the **Find your callback URL** section above to learn how to obtain your callback URL.
  - Enter your `Website URL` (tip: try `http://127.0.0.1:port` or `http://www.localhost:port` during development)
  - Enter your `Terms of service URL`.
  - Enter your `Privacy policy URL`.
- Click `Save`.

## Enter your Twitter credentials into your Supabase project [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitter\#enter-your-twitter-credentials-into-your-supabase-project)

- Go to your [Supabase Project Dashboard](https://supabase.com/dashboard)
- In the left sidebar, click the `Authentication` icon (near the top)
- Click on [`Providers`](https://supabase.com/dashboard/project/_/auth/providers) under the Configuration section
- Click on **Twitter** from the accordion list to expand and turn **Twitter Enabled** to ON
- Enter your **Twitter Client ID** and **Twitter Client Secret** saved in the previous step
- Click `Save`

## Add login code to your client app [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitter\#add-login-code-to-your-client-app)

JavaScriptFlutterKotlin

Make sure you're using the right `supabase` client in the following code.

If you're not using Server-Side Rendering or cookie-based Auth, you can directly use the `createClient` from `@supabase/supabase-js`. If you're using Server-Side Rendering, see the [Server-Side Auth guide](https://supabase.com/docs/guides/auth/server-side/creating-a-client) for instructions on creating your Supabase client.

When your user signs in, call [`signInWithOAuth()`](https://supabase.com/docs/reference/javascript/auth-signinwithoauth) with `twitter` as the `provider`:

`
async function signInWithTwitter() {
const { data, error } = await supabase.auth.signInWithOAuth({
    provider: 'twitter',
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

## Resources [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitter\#resources)

- [Supabase - Get started for free](https://supabase.com/)
- [Supabase JS Client](https://github.com/supabase/supabase-js)
- [Twitter Developer Dashboard](https://developer.twitter.com/en/portal/dashboard)

### Is this helpful?

NoYes

### On this page

[Overview](https://supabase.com/docs/guides/auth/social-login/auth-twitter#overview) [Access your Twitter Developer account](https://supabase.com/docs/guides/auth/social-login/auth-twitter#access-your-twitter-developer-account) [Find your callback URL](https://supabase.com/docs/guides/auth/social-login/auth-twitter#find-your-callback-url) [Create a Twitter OAuth app](https://supabase.com/docs/guides/auth/social-login/auth-twitter#create-a-twitter-oauth-app) [Enter your Twitter credentials into your Supabase project](https://supabase.com/docs/guides/auth/social-login/auth-twitter#enter-your-twitter-credentials-into-your-supabase-project) [Add login code to your client app](https://supabase.com/docs/guides/auth/social-login/auth-twitter#add-login-code-to-your-client-app) [Resources](https://supabase.com/docs/guides/auth/social-login/auth-twitter#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings