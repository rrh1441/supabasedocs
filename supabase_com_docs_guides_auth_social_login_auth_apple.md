Auth

# Login with Apple

* * *

Supabase Auth supports using [Sign in with Apple](https://developer.apple.com/sign-in-with-apple/) on the web and in native apps for iOS, macOS, watchOS or tvOS.

## Overview [\#](https://supabase.com/docs/guides/auth/social-login/auth-apple\#overview)

To support Sign in with Apple, you need to configure the [Apple provider in the Supabase dashboard](https://supabase.com/dashboard/project/_/auth/providers) for your project.

There are three general ways to use Sign in with Apple, depending on the application you're trying to build:

- Sign in on the web or in web-based apps
  - Using an OAuth flow initiated by Supabase Auth using the [Sign in with Apple REST API](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_rest_api).
  - Using [Sign in with Apple JS](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_js) directly in the browser, usually suitable for websites.
- Sign in natively inside iOS, macOS, watchOS or tvOS apps using [Apple's Authentication Services](https://developer.apple.com/documentation/authenticationservices)

In some cases you're able to use the OAuth flow within web-based native apps such as with [React Native](https://reactnative.dev/), [Expo](https://expo.dev/) or other similar frameworks. It is best practice to use native Sign in with Apple capabilities on those platforms instead.

When developing with Expo, you can test Sign in with Apple via the Expo Go app, in all other cases you will need to obtain an [Apple Developer](https://developer.apple.com/) account to enable the capability.

WebExpo React NativeFlutterSwiftKotlin

## Using the OAuth flow for web [\#](https://supabase.com/docs/guides/auth/social-login/auth-apple\#using-the-oauth-flow-for-web)

Sign in with Apple's OAuth flow is designed for web or browser based sign in methods. It can be used on web-based apps as well as websites, though some users can benefit by using Sign in with Apple JS directly.

Behind the scenes, Supabase Auth uses the [REST APIs](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_rest_api) provided by Apple.

Make sure you're using the right `supabase` client in the following code.

If you're not using Server-Side Rendering or cookie-based Auth, you can directly use the `createClient` from `@supabase/supabase-js`. If you're using Server-Side Rendering, see the [Server-Side Auth guide](https://supabase.com/docs/guides/auth/server-side/creating-a-client) for instructions on creating your Supabase client.

To initiate sign in, you can use the `signInWithOAuth()` method from the Supabase JavaScript library:

`
supabase.auth.signInWithOAuth({
provider: 'apple',
})
`

This call takes the user to Apple's consent screen. Once the flow ends, the user's profile information is exchanged and validated with Supabase Auth before it redirects back to your web application with an access and refresh token representing the user's session.

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

### Configuration [\#](https://supabase.com/docs/guides/auth/social-login/auth-apple\#configuration-web)

You will require the following information:

1. Your Apple Developer account's **Team ID**, which is an alphanumeric string of 10 characters that uniquely identifies the developer of the app. It's often accessible in the upper right-side menu on the Apple Developer Console.
2. Register email sources for _Sign in with Apple for Email Communication_ which can be found in the [Services](https://developer.apple.com/account/resources/services/list) section of the Apple Developer Console.
3. An **App ID** which uniquely identifies the app you are building. You can create a new App ID from the [Identifiers](https://developer.apple.com/account/resources/identifiers/list/bundleId) section in the Apple Developer Console (use the filter menu in the upper right side to see all App IDs). These usually are a reverse domain name string, for example `com.example.app`. Make sure you configure Sign in with Apple once you create an App ID in the Capabilities list. At this time Supabase Auth does not support Server-to-Server notification endpoints, so you should leave that setting blank. (In the past App IDs were referred to as _bundle IDs._)
4. A **Services ID** which uniquely identifies the web services provided by the app you registered in the previous step. You can create a new Services ID from the [Identifiers](https://developer.apple.com/account/resources/identifiers/list/serviceId) section in the Apple Developer Console (use the filter menu in the upper right side to see all Services IDs). These usually are a reverse domain name string, for example `com.example.app.web`.
5. Configure Website URLs for the newly created **Services ID**. The web domain you should use is the domain your Supabase project is hosted on. This is usually `<project-id>.supabase.co` while the redirect URL is `https://<project-id>.supabase.co/auth/v1/callback`.
6. Create a signing **Key** in the [Keys](https://developer.apple.com/account/resources/authkeys/list) section of the Apple Developer Console. You can use this key to generate a secret key using the tool below, which is added to your Supabase project's Auth configuration. Make sure you safely store the `AuthKey_XXXXXXXXXX.p8` file. If you ever lose access to it, or make it public accidentally, revoke it from the Apple Developer Console and create a new one immediately. You will have to generate a new secret key using this file every 6 months, so make sure you schedule a recurring meeting in your calendar!
7. Finally, add the information you configured above to the [Apple provider configuration in the Supabase dashboard](https://supabase.com/dashboard/project/_/auth/providers).

Use this tool to generate a new Apple client secret. No keys leave your browser! Be aware that this tool does not currently work in Safari, so use Firefox or a Chrome-based browser instead.

Account IDrequired

Found in the upper-right corner of Apple Developer Center.

Service IDrequired

Found under Certificates, Identifiers & Profiles in Apple Developer Center.

Key ID(optional)

If the file you select does not preserve the original name from Apple Developer Center, please enter the key ID.

Generate Secret Key

## Using sign in with Apple JS [\#](https://supabase.com/docs/guides/auth/social-login/auth-apple\#using-sign-in-with-apple-js)

[Sign in with Apple JS](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_js) is an official Apple framework for authenticating Apple users on websites. Although it can be used in web-based apps, those use cases will benefit more with the OAuth flow described above. We recommend using this method on classic websites only.

You can use the `signInWithIdToken()` method from the Supabase JavaScript library on the website to obtain an access and refresh token once the user has given consent using Sign in with Apple JS:

`
function signIn() {
const data = await AppleID.auth.signIn()
await supabase.auth.signInWithIdToken({
    provider: 'apple',
    token: data.id_token,
    nonce: '<nonce used in AppleID.auth.init>',
})
}
`

Alternatively, you can use the `AppleIDSignInOnSuccess` event with the `usePopup` option:

`
// Listen for authorization success.
document.addEventListener('AppleIDSignInOnSuccess', async (event) => {
await supabase.auth.signInWithIdToken({
    provider: 'apple',
    token: event.data.id_token,
    nonce: '<value used in appleid-signin-nonce meta tag>',
})
})
`

Make sure you request for the scope `name email` when initializing the library.

### Configuration [\#](https://supabase.com/docs/guides/auth/social-login/auth-apple\#configuration-apple-js)

To use Sign in with Apple JS you need to configure these options:

1. Have an **App ID** which uniquely identifies the app you are building. You can create a new App ID from the [Identifiers](https://developer.apple.com/account/resources/identifiers/list/bundleId) section in the Apple Developer Console (use the filter menu in the upper right side to see all App IDs). These usually are a reverse domain name string, for example `com.example.app`. Make sure you configure Sign in with Apple for the App ID you created or already have, in the Capabilities list. At this time Supabase Auth does not support Server-to-Server notification endpoints, so you should leave that setting blank. (In the past App IDs were referred to as _bundle IDs._)
2. Obtain a **Services ID** attached to the App ID that uniquely identifies the website. Use this value as the client ID when initializing Sign in with Apple JS. You can create a new Services ID from the [Identifiers](https://developer.apple.com/account/resources/identifiers/list/serviceId) section in the Apple Developer Console (use the filter menu in the upper right side to see all Services IDs). These usually are a reverse domain name string, for example `com.example.app.website`.
3. Configure Website URLs for the newly created **Services ID**. The web domain you should use is the domain your website is hosted on. The redirect URL must also point to a page on your website that will receive the callback from Apple.
4. Register the Services ID you created to your project's [Apple provider configuration in the Supabase dashboard](https://supabase.com/dashboard/project/_/auth/providers) under _Client IDs_.

If you're using Sign in with Apple JS you do not need to configure the OAuth settings.

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2F-tpcZzTdvN0%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Overview](https://supabase.com/docs/guides/auth/social-login/auth-apple#overview) [Using the OAuth flow for web](https://supabase.com/docs/guides/auth/social-login/auth-apple#using-the-oauth-flow-for-web) [Configuration](https://supabase.com/docs/guides/auth/social-login/auth-apple#configuration-web) [Using sign in with Apple JS](https://supabase.com/docs/guides/auth/social-login/auth-apple#using-sign-in-with-apple-js) [Configuration](https://supabase.com/docs/guides/auth/social-login/auth-apple#configuration-apple-js)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings