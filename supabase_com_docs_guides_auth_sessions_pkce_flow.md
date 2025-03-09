Auth

# PKCE flow

## About authenticating with PKCE flow.

* * *

The Proof Key for Code Exchange (PKCE) flow is one of two ways that a user can authenticate and your app can receive the necessary access and refresh tokens.

The flow is an implementation detail handled for you by Supabase Auth, but understanding the difference between PKCE and [implicit flow](https://supabase.com/docs/guides/auth/sessions/implicit-flow) is important for understanding the difference between client-only and server-side auth.

## How it works [\#](https://supabase.com/docs/guides/auth/sessions/pkce-flow\#how-it-works)

After a successful verification, the user is redirected to your app with a URL that looks like this:

`
https://yourapp.com/...?code=<...>
`

The `code` parameter is commonly known as the Auth Code and can be exchanged for an access token by calling `exchangeCodeForSession(code)`.

For security purposes, the code has a validity of 5 minutes and can only be exchanged for an access token once. You will need to restart the authentication flow from scratch if you wish to obtain a new access token.

As the flow is run server side, `localStorage` may not be available. You may configure the client library to use a custom storage adapter and an alternate backing storage such as cookies by setting the `storage` option to an object with the following methods:

`
const customStorageAdapter: SupportedStorage = {
    getItem: (key) => {
    if (!supportsLocalStorage()) {
        // Configure alternate storage
        return null
    }
    return globalThis.localStorage.getItem(key)
    },
    setItem: (key, value) => {
    if (!supportsLocalStorage()) {
        // Configure alternate storage here
        return
    }
    globalThis.localStorage.setItem(key, value)
    },
    removeItem: (key) => {
    if (!supportsLocalStorage()) {
        // Configure alternate storage here
        return
    }
    globalThis.localStorage.removeItem(key)
    },
}
`

You may also configure the client library to automatically exchange it for a session after a successful redirect. This can be done by setting the `detectSessionInUrl` option to `true`.

Putting it all together, your client library initialization may look like this:

`
const supabase = createClient(
        'https://xyzcompany.supabase.co',
        'public-anon-key',
        {
        ...
        auth: {
            ...
            detectSessionInUrl: true,
            flowType: 'pkce',
            storage: customStorageAdapter,
        }
        ...
        }
)
`

## Limitations [\#](https://supabase.com/docs/guides/auth/sessions/pkce-flow\#limitations)

Behind the scenes, the code exchange requires a code verifier. Both the code in the URL and the code verifier are sent back to the Auth server for a successful exchange.

The code verifier is created and stored locally when the Auth flow is first initiated. That means the code exchange must be initiated on the same browser and device where the flow was started.

## Resources [\#](https://supabase.com/docs/guides/auth/sessions/pkce-flow\#resources)

- [OAuth 2.0 guide](https://oauth.net/2/pkce/) to PKCE flow

### Is this helpful?

NoYes

### On this page

[How it works](https://supabase.com/docs/guides/auth/sessions/pkce-flow#how-it-works) [Limitations](https://supabase.com/docs/guides/auth/sessions/pkce-flow#limitations) [Resources](https://supabase.com/docs/guides/auth/sessions/pkce-flow#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings