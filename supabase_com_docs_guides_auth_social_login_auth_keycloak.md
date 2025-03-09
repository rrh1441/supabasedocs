Auth

# Login with Keycloak

* * *

To enable Keycloak Auth for your project, you need to set up an Keycloak OAuth application and add the application credentials to your Supabase Dashboard.

## Overview [\#](https://supabase.com/docs/guides/auth/social-login/auth-keycloak\#overview)

To get started with Keycloak, you can run it in a docker container with: `docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:latest start-dev`

This guide will be assuming that you are running Keycloak in a docker container as described in the command above.

Keycloak OAuth consists of five broad steps:

- Create a new client in your specified Keycloak realm.
- Obtain the `issuer` from the "OpenID Endpoint Configuration". This will be used as the `Keycloak URL`.
- Ensure that the new client has the "Client Protocol" set to `openid-connect` and the "Access Type" is set to "confidential".
- The `Client ID` of the client created will be used as the `client id`.
- Obtain the `Secret` from the credentials tab which will be used as the `client secret`.
- Add the callback URL of your application to your allowlist.

## Access your Keycloak admin console [\#](https://supabase.com/docs/guides/auth/social-login/auth-keycloak\#access-your-keycloak-admin-console)

- Login by visiting [`http://localhost:8080`](http://localhost:8080/) and clicking on "Administration Console".

## Create a Keycloak realm [\#](https://supabase.com/docs/guides/auth/social-login/auth-keycloak\#create-a-keycloak-realm)

- Once you've logged in to the Keycloak console, you can add a realm from the side panel. The default realm should be named "Master".
- After you've added a new realm, you can retrieve the `issuer` from the "OpenID Endpoint Configuration" endpoint. The `issuer` will be used as the `Keycloak URL`.
- You can find this endpoint from the realm settings under the "General Tab" or visit [`http://localhost:8080/realms/my_realm_name/.well-known/openid-configuration`](http://localhost:8080/realms/my_realm_name/.well-known/openid-configuration)

![Add a Keycloak Realm.](https://supabase.com/docs/img/guides/auth-keycloak/keycloak-create-realm.png)

## Create a Keycloak client [\#](https://supabase.com/docs/guides/auth/social-login/auth-keycloak\#create-a-keycloak-client)

The "Client ID" of the created client will serve as the `client_id` when you make API calls to authenticate the user.

![Add a Keycloak client](https://supabase.com/docs/img/guides/auth-keycloak/keycloak-add-client.png)

## Client settings [\#](https://supabase.com/docs/guides/auth/social-login/auth-keycloak\#client-settings)

After you've created the client successfully, ensure that you set the following settings:

1. The "Client Protocol" should be set to `openid-connect`.
2. The "Access Type" should be set to "confidential".
3. The "Valid Redirect URIs" should be set to: `https://<project-ref>.supabase.co/auth/v1/callback`.

![Obtain the client id, set the client protocol and access type](https://supabase.com/docs/img/guides/auth-keycloak/keycloak-client-id.png)![Set redirect uri](https://supabase.com/docs/img/guides/auth-keycloak/keycloak-redirect-uri.png)

## Obtain the client secret [\#](https://supabase.com/docs/guides/auth/social-login/auth-keycloak\#obtain-the-client-secret)

This will serve as the `client_secret` when you make API calls to authenticate the user.
Under the "Credentials" tab, the `Secret` value will be used as the `client secret`.

![Obtain the client secret](https://supabase.com/docs/img/guides/auth-keycloak/keycloak-client-secret.png)

## Add login code to your client app [\#](https://supabase.com/docs/guides/auth/social-login/auth-keycloak\#add-login-code-to-your-client-app)

Since Keycloak version 22, the `openid` scope must be passed. Add this to the [`supabase.auth.signInWithOAuth()`](https://supabase.com/docs/reference/javascript/auth-signinwithoauth) method.

JavaScriptFlutterKotlin

Make sure you're using the right `supabase` client in the following code.

If you're not using Server-Side Rendering or cookie-based Auth, you can directly use the `createClient` from `@supabase/supabase-js`. If you're using Server-Side Rendering, see the [Server-Side Auth guide](https://supabase.com/docs/guides/auth/server-side/creating-a-client) for instructions on creating your Supabase client.

When your user signs in, call [`signInWithOAuth()`](https://supabase.com/docs/reference/javascript/auth-signinwithoauth) with `keycloak` as the `provider`:

`
async function signInWithKeycloak() {
const { data, error } = await supabase.auth.signInWithOAuth({
    provider: 'keycloak',
    options: {
      scopes: 'openid',
    },
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

## Resources [\#](https://supabase.com/docs/guides/auth/social-login/auth-keycloak\#resources)

- You can find the Keycloak OpenID endpoint configuration under the realm settings.
![Keycloak OpenID Endpoint Configuration](https://supabase.com/docs/img/guides/auth-keycloak/keycloak-openid-endpoint-config.png)

### Is this helpful?

NoYes

### On this page

[Overview](https://supabase.com/docs/guides/auth/social-login/auth-keycloak#overview) [Access your Keycloak admin console](https://supabase.com/docs/guides/auth/social-login/auth-keycloak#access-your-keycloak-admin-console) [Create a Keycloak realm](https://supabase.com/docs/guides/auth/social-login/auth-keycloak#create-a-keycloak-realm) [Create a Keycloak client](https://supabase.com/docs/guides/auth/social-login/auth-keycloak#create-a-keycloak-client) [Client settings](https://supabase.com/docs/guides/auth/social-login/auth-keycloak#client-settings) [Obtain the client secret](https://supabase.com/docs/guides/auth/social-login/auth-keycloak#obtain-the-client-secret) [Add login code to your client app](https://supabase.com/docs/guides/auth/social-login/auth-keycloak#add-login-code-to-your-client-app) [Resources](https://supabase.com/docs/guides/auth/social-login/auth-keycloak#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings