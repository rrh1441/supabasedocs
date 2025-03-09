Auth

# Login with Twitch

* * *

To enable Twitch Auth for your project, you need to set up a Twitch Application and add the Application OAuth credentials to your Supabase Dashboard.

## Overview [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitch\#overview)

Setting up Twitch logins for your application consists of 3 parts:

- Create and configure a Twitch Application [Twitch Developer Console](https://dev.twitch.tv/console)
- Add your Twitch OAuth Consumer keys to your [Supabase Project](https://supabase.com/dashboard)
- Add the login code to your [Supabase JS Client App](https://github.com/supabase/supabase-js)

## Access your Twitch Developer account [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitch\#access-your-twitch-developer-account)

- Go to [dev.twitch.tv](https://dev.twitch.tv/).
- Click on `Log in with Twitch` at the top right to log in.
- If you have not already enabled 2-Factor Authentication for your Twitch Account, you will need to do that at [Twitch Security Settings](https://www.twitch.tv/settings/security) before you can continue.

![Twitch Developer Page](https://supabase.com/docs/img/guides/auth-twitch/twitch-developer-page.png)

- Once logged in, go to the [Twitch Developer Console](https://dev.twitch.tv/console).

![Twitch Developer Console](https://supabase.com/docs/img/guides/auth-twitch/twitch-console.png)

## Find your callback URL [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitch\#find-your-callback-url)

The next step requires a callback URL, which looks like this: `https://<project-ref>.supabase.co/auth/v1/callback`

- Go to your [Supabase Project Dashboard](https://supabase.com/dashboard)
- Click on the `Authentication` icon in the left sidebar
- Click on [`Providers`](https://supabase.com/dashboard/project/_/auth/providers) under the Configuration section
- Click on **Twitch** from the accordion list to expand and you'll find your **Callback URL**, you can click `Copy` to copy it to the clipboard

For testing OAuth locally with the Supabase CLI please see the [local development docs](https://supabase.com/docs/guides/cli/local-development#use-auth-locally).

## Create a Twitch application [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitch\#create-a-twitch-application)

![Twitch Developer Console](https://supabase.com/docs/img/guides/auth-twitch/twitch-console.png)

- Click on `+ Register Your Application` at the top right.

![Register Application](https://supabase.com/docs/img/guides/auth-twitch/twitch-register-your-application.png)

- Enter the name of your application.
- Type or paste your `OAuth Redirect URL` (the callback URL from the previous step.)
- Select a category for your app.
- Check the CAPTCHA box and click `Create`.

## Retrieve your Twitch OAuth client ID and client secret [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitch\#retrieve-your-twitch-oauth-client-id-and-client-secret)

- Click `Manage` at the right of your application entry in the list.

![Twitch Applications List](https://supabase.com/docs/img/guides/auth-twitch/twitch-applications-list.png)

- Copy your Client ID.
- Click `New Secret` to create a new Client Secret.
- Copy your Client Secret.

![Get Client ID and Secret](https://supabase.com/docs/img/guides/auth-twitch/twitch-get-keys.png)

## Add your Twitch credentials into your Supabase project [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitch\#add-your-twitch-credentials-into-your-supabase-project)

- Go to your [Supabase Project Dashboard](https://supabase.com/dashboard)
- In the left sidebar, click the `Authentication` icon (near the top)
- Click on [`Providers`](https://supabase.com/dashboard/project/_/auth/providers) under the Configuration section
- Click on **Twitch** from the accordion list to expand and turn **Twitch Enabled** to ON
- Enter your **Twitch Client ID** and **Twitch Client Secret** saved in the previous step
- Click `Save`

## Add login code to your client app [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitch\#add-login-code-to-your-client-app)

JavaScriptFlutterKotlin

Make sure you're using the right `supabase` client in the following code.

If you're not using Server-Side Rendering or cookie-based Auth, you can directly use the `createClient` from `@supabase/supabase-js`. If you're using Server-Side Rendering, see the [Server-Side Auth guide](https://supabase.com/docs/guides/auth/server-side/creating-a-client) for instructions on creating your Supabase client.

When your user signs in, call [`signInWithOAuth()`](https://supabase.com/docs/reference/javascript/auth-signinwithoauth) with `twitch` as the `provider`:

`
async function signInWithTwitch() {
const { data, error } = await supabase.auth.signInWithOAuth({
    provider: 'twitch',
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

## Resources [\#](https://supabase.com/docs/guides/auth/social-login/auth-twitch\#resources)

- [Supabase - Get started for free](https://supabase.com/)
- [Supabase JS Client](https://github.com/supabase/supabase-js)
- [Twitch Account](https://twitch.tv/)
- [Twitch Developer Console](https://dev.twitch.tv/console)

### Is this helpful?

NoYes

### On this page

[Overview](https://supabase.com/docs/guides/auth/social-login/auth-twitch#overview) [Access your Twitch Developer account](https://supabase.com/docs/guides/auth/social-login/auth-twitch#access-your-twitch-developer-account) [Find your callback URL](https://supabase.com/docs/guides/auth/social-login/auth-twitch#find-your-callback-url) [Create a Twitch application](https://supabase.com/docs/guides/auth/social-login/auth-twitch#create-a-twitch-application) [Retrieve your Twitch OAuth client ID and client secret](https://supabase.com/docs/guides/auth/social-login/auth-twitch#retrieve-your-twitch-oauth-client-id-and-client-secret) [Add your Twitch credentials into your Supabase project](https://supabase.com/docs/guides/auth/social-login/auth-twitch#add-your-twitch-credentials-into-your-supabase-project) [Add login code to your client app](https://supabase.com/docs/guides/auth/social-login/auth-twitch#add-login-code-to-your-client-app) [Resources](https://supabase.com/docs/guides/auth/social-login/auth-twitch#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings