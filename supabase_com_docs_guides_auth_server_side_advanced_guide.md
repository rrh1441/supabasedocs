Auth

# Advanced guide

## Details about SSR Auth flows and implementation for advanced users.

* * *

When a user authenticates with Supabase Auth, two pieces of information are issued by the server:

1. **Access token** in the form of a JWT.
2. **Refresh token** which is a randomly generated string.

The default behavior if you're not using SSR is to store this information in local storage. Local storage isn't accessible by the server, so for SSR, the tokens instead need to be stored in a secure cookie. The cookie can then be passed back and forth between your app code in the client and your app code in the server.

If you're not using SSR, you might also be using the [implicit flow](https://supabase.com/docs/guides/auth/sessions/implicit-flow) to get the access and refresh tokens. The server can't access the tokens in this flow, so for SSR, you should change to the [PKCE flow](https://supabase.com/docs/guides/auth/sessions/pkce-flow). You can change the flow type when initiating your Supabase client if your client library provides this option.

In the `@supabase/ssr` package, Supabase clients are initiated to use the PKCE flow by default. They are also automatically configured to handle the saving and retrieval of session information in cookies.

## How it works [\#](https://supabase.com/docs/guides/auth/server-side/advanced-guide\#how-it-works)

In the PKCE flow, a redirect is made to your app, with an Auth Code contained in the URL. When you exchange this code using `exchangeCodeForSession`, you receive the session information, which contains the access and refresh tokens.

To maintain the session, these tokens must be stored in a storage medium securely shared between client and server, which is traditionally cookies. Whenever the session is refreshed, the auth and refresh tokens in the shared storage medium must be updated. Supabase client libraries provide a customizable `storage` option when a client is initiated, allowing you to change where tokens are stored.

For an implementation example, see the [@supabase/ssr](https://github.com/supabase/auth-helpers/blob/main/packages/ssr/src/index.ts) package.

## Frequently asked questions [\#](https://supabase.com/docs/guides/auth/server-side/advanced-guide\#frequently-asked-questions)

### No session on the server side with Next.js route prefetching? [\#](https://supabase.com/docs/guides/auth/server-side/advanced-guide\#no-session-on-the-server-side-with-nextjs-route-prefetching)

When you use route prefetching in Next.js using `<Link href="/...">` components or the `Router.push()` APIs can send server-side requests before the browser processes the access and refresh tokens. This means that those requests may not have any cookies set and your server code will render unauthenticated content.

To improve experience for your users, we recommend redirecting users to one specific page after sign-in that does not include any route prefetching from Next.js. Once the Supabase client library running in the browser has obtained the access and refresh tokens from the URL fragment, you can send users to any pages that use prefetching.

### How do I make the cookies `HttpOnly`? [\#](https://supabase.com/docs/guides/auth/server-side/advanced-guide\#how-do-i-make-the-cookies-httponly-)

This is not necessary. Both the access token and refresh token are designed to be passed around to different components in your application. The browser-based side of your application needs access to the refresh token to properly maintain a browser session anyway.

### My server is getting invalid refresh token errors. What's going on? [\#](https://supabase.com/docs/guides/auth/server-side/advanced-guide\#my-server-is-getting-invalid-refresh-token-errors-whats-going-on)

It is likely that the refresh token sent from the browser to your server is stale. Make sure the `onAuthStateChange` listener callback is free of bugs and is registered relatively early in your application's lifetime

When you receive this error on the server-side, try to defer rendering to the browser where the client library can access an up-to-date refresh token and present the user with a better experience.

### Should I set a shorter `Max-Age` parameter on the cookies? [\#](https://supabase.com/docs/guides/auth/server-side/advanced-guide\#should-i-set-a-shorter-max-age-parameter-on-the-cookies)

The `Max-Age` or `Expires` cookie parameters only control whether the browser sends the value to the server. Since a refresh token represents the long-lived authentication session of the user on that browser, setting a short `Max-Age` or `Expires` parameter on the cookies only results in a degraded user experience.

The only way to ensure that a user has logged out or their session has ended is to get the user's details with `getUser()`.

### What should I use for the `SameSite` property? [\#](https://supabase.com/docs/guides/auth/server-side/advanced-guide\#what-should-i-use-for-the-samesite-property)

Make sure you [understand the behavior of the property in different situations](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite) as some properties can degrade the user experience.

A good default is to use `Lax` which sends cookies when users are navigating to your site. Cookies typically require the `Secure` attribute, which only sends them over HTTPS. However, this can be a problem when developing on `localhost`.

### Can I use server-side rendering with a CDN or cache? [\#](https://supabase.com/docs/guides/auth/server-side/advanced-guide\#can-i-use-server-side-rendering-with-a-cdn-or-cache)

Yes, but you need to be careful to include at least the refresh token cookie value in the cache key. Otherwise you may be accidentally serving pages with data belonging to different users!

Also be sure you set proper cache control headers. We recommend invalidating cache keys every hour or less.

### Which authentication flows have PKCE support? [\#](https://supabase.com/docs/guides/auth/server-side/advanced-guide\#which-authentication-flows-have-pkce-support)

At present, PKCE is supported on the Magic Link, OAuth, Sign Up, and Password Recovery routes. These correspond to the `signInWithOtp`, `signInWithOAuth`, `signUp`, and `resetPasswordForEmail` methods on the Supabase client library. When using PKCE with Phone and Email OTPs, there is no behavior change with respect to the implicit flow - an access token will be returned in the body when a request is successful.

### Is this helpful?

NoYes

### On this page

[How it works](https://supabase.com/docs/guides/auth/server-side/advanced-guide#how-it-works) [Frequently asked questions](https://supabase.com/docs/guides/auth/server-side/advanced-guide#frequently-asked-questions) [No session on the server side with Next.js route prefetching?](https://supabase.com/docs/guides/auth/server-side/advanced-guide#no-session-on-the-server-side-with-nextjs-route-prefetching) [How do I make the cookies HttpOnly?](https://supabase.com/docs/guides/auth/server-side/advanced-guide#how-do-i-make-the-cookies-httponly-) [My server is getting invalid refresh token errors. What's going on?](https://supabase.com/docs/guides/auth/server-side/advanced-guide#my-server-is-getting-invalid-refresh-token-errors-whats-going-on) [Should I set a shorter Max-Age parameter on the cookies?](https://supabase.com/docs/guides/auth/server-side/advanced-guide#should-i-set-a-shorter-max-age-parameter-on-the-cookies) [What should I use for the SameSite property?](https://supabase.com/docs/guides/auth/server-side/advanced-guide#what-should-i-use-for-the-samesite-property) [Can I use server-side rendering with a CDN or cache?](https://supabase.com/docs/guides/auth/server-side/advanced-guide#can-i-use-server-side-rendering-with-a-cdn-or-cache) [Which authentication flows have PKCE support?](https://supabase.com/docs/guides/auth/server-side/advanced-guide#which-authentication-flows-have-pkce-support)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings