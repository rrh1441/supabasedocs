Auth

# Creating a Supabase client for SSR

## Configure your Supabase client to use cookies

* * *

To use Server-Side Rendering (SSR) with Supabase, you need to configure your Supabase client to use cookies. The `@supabase/ssr` package helps you do this for JavaScript/TypeScript applications.

## Install [\#](https://supabase.com/docs/guides/auth/server-side/creating-a-client\#install)

Install the `@supabase/ssr` and `@supabase/supabase-js` packages:

npmyarnpnpm

`
npm install @supabase/ssr @supabase/supabase-js
`

## Set environment variables [\#](https://supabase.com/docs/guides/auth/server-side/creating-a-client\#set-environment-variables)

In your environment variables file, set your Supabase URL and Supabase Anon Key:

###### Project URL

No project found

To get your Project URL, [log in](https://supabase.com/dashboard).

###### Anon key

No project found

To get your Anon key, [log in](https://supabase.com/dashboard).

Next.jsSvelteKitAstroRemixExpressHono

.env.local

`
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
`

## Create a client [\#](https://supabase.com/docs/guides/auth/server-side/creating-a-client\#create-a-client)

You'll need some one-time setup code to configure your Supabase client to use cookies. Once your utility code is set up, you can use your new `createClient` utility functions to get a properly configured Supabase client.

Use the browser client in code that runs on the browser, and the server client in code that runs on the server.

Next.jsSvelteKitAstroRemixExpressHono

The following code samples are for App Router. For help with Pages Router, see the [Next.js Server-Side Auth guide](https://supabase.com/docs/guides/auth/server-side/nextjs?queryGroups=router&router=pages).

Client-sideServer-sideMiddleware

## Next steps [\#](https://supabase.com/docs/guides/auth/server-side/creating-a-client\#next-steps)

- Implement [Authentication using Email and Password](https://supabase.com/docs/guides/auth/server-side/email-based-auth-with-pkce-flow-for-ssr)
- Implement [Authentication using OAuth](https://supabase.com/docs/guides/auth/server-side/oauth-with-pkce-flow-for-ssr)
- [Learn more about SSR](https://supabase.com/docs/guides/auth/server-side-rendering)

### Is this helpful?

NoYes

### On this page

[Install](https://supabase.com/docs/guides/auth/server-side/creating-a-client#install) [Set environment variables](https://supabase.com/docs/guides/auth/server-side/creating-a-client#set-environment-variables) [Create a client](https://supabase.com/docs/guides/auth/server-side/creating-a-client#create-a-client) [Next steps](https://supabase.com/docs/guides/auth/server-side/creating-a-client#next-steps)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings