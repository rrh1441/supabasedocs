Auth

# Login with Slack

* * *

To enable Slack Auth for your project, you need to set up a Slack OAuth application and add the application credentials to your Supabase Dashboard.

## Overview [\#](https://supabase.com/docs/guides/auth/social-login/auth-slack\#overview)

We will be replacing the existing Slack provider with a new Slack (OIDC) provider. Developers with Slack OAuth Applications created prior to 24th June 2024 should create a new application and migrate their credentials from the Slack provider to the Slack (OIDC) provider. Existing OAuth Applications built with the old Slack provider will continue to work up till 10th October. You can refer to the [**list of supported scopes**](https://api.slack.com/scopes?filter=user) for the new Slack (OIDC) User.

Setting up Slack logins for your application consists of 3 parts:

- Create and configure a Slack Project and App on the [Slack Developer Dashboard](https://api.slack.com/apps).
- Add your Slack `API Key` and `API Secret Key` to your [Supabase Project](https://supabase.com/dashboard).
- Add the login code to your [Supabase JS Client App](https://github.com/supabase/supabase-js).

## Access your Slack Developer account [\#](https://supabase.com/docs/guides/auth/social-login/auth-slack\#access-your-slack-developer-account)

- Go to [api.slack.com](https://api.slack.com/apps).
- Click on `Your Apps` at the top right to log in.

![Slack Developer Portal.](https://supabase.com/docs/img/guides/auth-slack/slack-portal.png)

## Find your callback URL [\#](https://supabase.com/docs/guides/auth/social-login/auth-slack\#find-your-callback-url)

The next step requires a callback URL, which looks like this: `https://<project-ref>.supabase.co/auth/v1/callback`

- Go to your [Supabase Project Dashboard](https://supabase.com/dashboard)
- Click on the `Authentication` icon in the left sidebar
- Click on [`Providers`](https://supabase.com/dashboard/project/_/auth/providers) under the Configuration section
- Click on **Slack** from the accordion list to expand and you'll find your **Callback URL**, you can click `Copy` to copy it to the clipboard

For testing OAuth locally with the Supabase CLI please see the [local development docs](https://supabase.com/docs/guides/cli/local-development#use-auth-locally).

## Create a Slack OAuth app [\#](https://supabase.com/docs/guides/auth/social-login/auth-slack\#create-a-slack-oauth-app)

- Go to [api.slack.com](https://api.slack.com/apps).
- Click on `Create New App`

Under `Create an app...`:

- Click `From scratch`
- Type the name of your app
- Select your `Slack Workspace`
- Click `Create App`

Under `App Credentials`:

- Copy and save your newly-generated `Client ID`
- Copy and save your newly-generated `Client Secret`

Under the sidebar, select `OAuth & Permissions` and look for `Redirect URLs`:

- Click `Add New Redirect URL`
- Paste your `Callback URL` then click `Add`
- Click `Save URLs`

Under `Scopes`:

- Add the following scopes under the `User Token Scopes`: `profile`, `email`, `openid`. These scopes are the default scopes that Supabase Auth uses to request for user information. Do not add other scopes as [Sign In With Slack only supports `profile`, `email`, `openid`](https://api.slack.com/authentication/sign-in-with-slack#request).

## Enter your Slack credentials into your Supabase project [\#](https://supabase.com/docs/guides/auth/social-login/auth-slack\#enter-your-slack-credentials-into-your-supabase-project)

- Go to your [Supabase Project Dashboard](https://supabase.com/dashboard)
- In the left sidebar, click the `Authentication` icon (near the top)
- Click on [`Providers`](https://supabase.com/dashboard/project/_/auth/providers) under the Configuration section
- Click on **Slack** from the accordion list to expand and turn **Slack Enabled** to ON
- Enter your **Slack Client ID** and **Slack Client Secret** saved in the previous step
- Click `Save`

## Add login code to your client app [\#](https://supabase.com/docs/guides/auth/social-login/auth-slack\#add-login-code-to-your-client-app)

JavaScriptFlutterKotlin

Make sure you're using the right `supabase` client in the following code.

If you're not using Server-Side Rendering or cookie-based Auth, you can directly use the `createClient` from `@supabase/supabase-js`. If you're using Server-Side Rendering, see the [Server-Side Auth guide](https://supabase.com/docs/guides/auth/server-side/creating-a-client) for instructions on creating your Supabase client.

When your user signs in, call [`signInWithOAuth()`](https://supabase.com/docs/reference/javascript/auth-signinwithoauth) with `slack_oidc` as the `provider`:

`
async function signInWithSlack() {
const { data, error } = await supabase.auth.signInWithOAuth({
    provider: 'slack_oidc',
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

## Resources [\#](https://supabase.com/docs/guides/auth/social-login/auth-slack\#resources)

- [Supabase - Get started for free](https://supabase.com/)
- [Supabase JS Client](https://github.com/supabase/supabase-js)
- [Slack Developer Dashboard](https://api.slack.com/apps)

### Is this helpful?

NoYes

### On this page

[Overview](https://supabase.com/docs/guides/auth/social-login/auth-slack#overview) [Access your Slack Developer account](https://supabase.com/docs/guides/auth/social-login/auth-slack#access-your-slack-developer-account) [Find your callback URL](https://supabase.com/docs/guides/auth/social-login/auth-slack#find-your-callback-url) [Create a Slack OAuth app](https://supabase.com/docs/guides/auth/social-login/auth-slack#create-a-slack-oauth-app) [Enter your Slack credentials into your Supabase project](https://supabase.com/docs/guides/auth/social-login/auth-slack#enter-your-slack-credentials-into-your-supabase-project) [Add login code to your client app](https://supabase.com/docs/guides/auth/social-login/auth-slack#add-login-code-to-your-client-app) [Resources](https://supabase.com/docs/guides/auth/social-login/auth-slack#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings