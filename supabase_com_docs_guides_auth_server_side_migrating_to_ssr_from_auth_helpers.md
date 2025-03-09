Auth

# Migrating to the SSR package from Auth Helpers

* * *

The new `ssr` package takes the core concepts of the Auth Helpers and makes them available to any server language or framework. This page will guide you through migrating from the Auth Helpers package to `ssr`.

### Replacing Supabase packages [\#](https://supabase.com/docs/guides/auth/server-side/migrating-to-ssr-from-auth-helpers\#replacing-supabase-packages)

Next.jsSvelteKitRemix

`
npm uninstall @supabase/auth-helpers-nextjs
`

`
npm install @supabase/ssr
`

### Creating a client [\#](https://supabase.com/docs/guides/auth/server-side/migrating-to-ssr-from-auth-helpers\#creating-a-client)

The new `ssr` package exports two functions for creating a Supabase client. The `createBrowserClient` function is used in the client, and the `createServerClient` function is used in the server.

Check out the [Creating a client](https://supabase.com/docs/guides/auth/server-side/creating-a-client) page for examples of creating a client in your framework.

## Next steps [\#](https://supabase.com/docs/guides/auth/server-side/migrating-to-ssr-from-auth-helpers\#next-steps)

- Implement [Authentication using Email and Password](https://supabase.com/docs/guides/auth/server-side/email-based-auth-with-pkce-flow-for-ssr)
- Implement [Authentication using OAuth](https://supabase.com/docs/guides/auth/server-side/oauth-with-pkce-flow-for-ssr)
- [Learn more about SSR](https://supabase.com/docs/guides/auth/server-side-rendering)

### Is this helpful?

NoYes

### On this page

[Replacing Supabase packages](https://supabase.com/docs/guides/auth/server-side/migrating-to-ssr-from-auth-helpers#replacing-supabase-packages) [Creating a client](https://supabase.com/docs/guides/auth/server-side/migrating-to-ssr-from-auth-helpers#creating-a-client) [Next steps](https://supabase.com/docs/guides/auth/server-side/migrating-to-ssr-from-auth-helpers#next-steps)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings