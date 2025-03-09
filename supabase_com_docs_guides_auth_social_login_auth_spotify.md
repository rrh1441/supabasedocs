Auth

# Login with Spotify

* * *

To enable Spotify Auth for your project, you need to set up a Spotify OAuth application and add the application credentials to your Supabase Dashboard.

## Overview [\#](https://supabase.com/docs/guides/auth/social-login/auth-spotify\#overview)

Setting up Spotify logins for your application consists of 3 parts:

- Create and configure a Spotify Project and App on the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard/).
- Add your Spotify `API Key` and `API Secret Key` to your [Supabase Project](https://supabase.com/dashboard).
- Add the login code to your [Supabase JS Client App](https://github.com/supabase/supabase-js).

## Access your Spotify Developer account [\#](https://supabase.com/docs/guides/auth/social-login/auth-spotify\#access-your-spotify-developer-account)

- Log into [Spotify](https://spotify.com/)
- Access the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard)

![Spotify Developer Portal.](https://supabase.com/docs/img/guides/auth-spotify/spotify-portal.png)

## Find your callback URL [\#](https://supabase.com/docs/guides/auth/social-login/auth-spotify\#find-your-callback-url)

The next step requires a callback URL, which looks like this: `https://<project-ref>.supabase.co/auth/v1/callback`

- Go to your [Supabase Project Dashboard](https://supabase.com/dashboard)
- Click on the `Authentication` icon in the left sidebar
- Click on [`Providers`](https://supabase.com/dashboard/project/_/auth/providers) under the Configuration section
- Click on **Spotify** from the accordion list to expand and you'll find your **Callback URL**, you can click `Copy` to copy it to the clipboard

For testing OAuth locally with the Supabase CLI please see the [local development docs](https://supabase.com/docs/guides/cli/local-development#use-auth-locally).

## Create a Spotify OAuth app [\#](https://supabase.com/docs/guides/auth/social-login/auth-spotify\#create-a-spotify-oauth-app)

- Log into [Spotify](https://spotify.com/).
- Go to the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard)
- Click `Create an App`
- Type your `App name`
- Type your `App description`
- Check the box to agree with the `Developer TOS and Branding Guidelines`
- Click `Create`
- Save your `Client ID`
- Save your `Client Secret`
- Click `Edit Settings`

Under `Redirect URIs`:

- Paste your Supabase Callback URL in the box
- Click `Add`
- Click `Save` at the bottom

## Enter your Spotify credentials into your Supabase project [\#](https://supabase.com/docs/guides/auth/social-login/auth-spotify\#enter-your-spotify-credentials-into-your-supabase-project)

- Go to your [Supabase Project Dashboard](https://supabase.com/dashboard)
- In the left sidebar, click the `Authentication` icon (near the top)
- Click on [`Providers`](https://supabase.com/dashboard/project/_/auth/providers) under the Configuration section
- Click on **Spotify** from the accordion list to expand and turn **Spotify Enabled** to ON
- Enter your **Spotify Client ID** and **Spotify Client Secret** saved in the previous step
- Click `Save`

## Add login code to your client app [\#](https://supabase.com/docs/guides/auth/social-login/auth-spotify\#add-login-code-to-your-client-app)

The following outlines the steps to sign in using Spotify with Supabase Auth.

1. Call the signin method from the client library.
2. The user is redirected to the Spotify login page.
3. After completing the sign-in process, the user will be redirected to your app with an error that says the email address needs to be confirmed. Simultaneously the user receives a confirmation email from Supabase Auth.
4. The user clicks the confirmation link in the email.
5. The user is brought back to the app and is now signed in.

JavaScriptFlutterKotlin

Make sure you're using the right `supabase` client in the following code.

If you're not using Server-Side Rendering or cookie-based Auth, you can directly use the `createClient` from `@supabase/supabase-js`. If you're using Server-Side Rendering, see the [Server-Side Auth guide](https://supabase.com/docs/guides/auth/server-side/creating-a-client) for instructions on creating your Supabase client.

When your user signs in, call [`signInWithOAuth()`](https://supabase.com/docs/reference/javascript/auth-signinwithoauth) with `spotify` as the `provider`:

`
async function signInWithSpotify() {
const { data, error } = await supabase.auth.signInWithOAuth({
    provider: 'spotify',
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

## Resources [\#](https://supabase.com/docs/guides/auth/social-login/auth-spotify\#resources)

- [Supabase - Get started for free](https://supabase.com/)
- [Supabase JS Client](https://github.com/supabase/supabase-js)
- [Spotify Developer Dashboard](https://developer.spotify.com/dashboard/)

### Is this helpful?

NoYes

### On this page

[Overview](https://supabase.com/docs/guides/auth/social-login/auth-spotify#overview) [Access your Spotify Developer account](https://supabase.com/docs/guides/auth/social-login/auth-spotify#access-your-spotify-developer-account) [Find your callback URL](https://supabase.com/docs/guides/auth/social-login/auth-spotify#find-your-callback-url) [Create a Spotify OAuth app](https://supabase.com/docs/guides/auth/social-login/auth-spotify#create-a-spotify-oauth-app) [Enter your Spotify credentials into your Supabase project](https://supabase.com/docs/guides/auth/social-login/auth-spotify#enter-your-spotify-credentials-into-your-supabase-project) [Add login code to your client app](https://supabase.com/docs/guides/auth/social-login/auth-spotify#add-login-code-to-your-client-app) [Resources](https://supabase.com/docs/guides/auth/social-login/auth-spotify#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings