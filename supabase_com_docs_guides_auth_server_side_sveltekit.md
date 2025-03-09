Auth

# Setting up Server-Side Auth for SvelteKit

* * *

Set up Server-Side Auth to use cookie-based authentication with SvelteKit.

1

### Install Supabase packages

Install the `@supabase/supabase-js` package and the helper `@supabase/ssr` package.

`
npm install @supabase/supabase-js @supabase/ssr
`

2

### Set up environment variables

Create a `.env.local` file in your project root directory.

Fill in your `PUBLIC_SUPABASE_URL` and `PUBLIC_SUPABASE_ANON_KEY`:

###### Project URL

No project found

To get your Project URL, [log in](https://supabase.com/dashboard).

###### Anon key

No project found

To get your Anon key, [log in](https://supabase.com/dashboard).

.env.local

`
PUBLIC_SUPABASE_URL=<your_supabase_project_url>
PUBLIC_SUPABASE_ANON_KEY=<your_supabase_anon_key>
`

3

### Set up server-side hooks

Set up server-side hooks in `src/hooks.server.ts`. The hooks:

- Create a request-specific Supabase client, using the user credentials from the request cookie. This client is used for server-only code.
- Check user authentication.
- Guard protected pages.

src/hooks.server.ts

``
import { createServerClient } from '@supabase/ssr'
import { type Handle, redirect } from '@sveltejs/kit'
import { sequence } from '@sveltejs/kit/hooks'
import { PUBLIC_SUPABASE_URL, PUBLIC_SUPABASE_ANON_KEY } from '$env/static/public'
const supabase: Handle = async ({ event, resolve }) => {
/**
* Creates a Supabase client specific to this server request.
*
* The Supabase client gets the Auth token from the request cookies.
*/
event.locals.supabase = createServerClient(PUBLIC_SUPABASE_URL, PUBLIC_SUPABASE_ANON_KEY, {
    cookies: {
      getAll: () => event.cookies.getAll(),
      /**
       * SvelteKit's cookies API requires `path` to be explicitly set in
       * the cookie options. Setting `path` to `/` replicates previous/
       * standard behavior.
       */
      setAll: (cookiesToSet) => {
        cookiesToSet.forEach(({ name, value, options }) => {
          event.cookies.set(name, value, { ...options, path: '/' })
        })
      },
    },
})
/**
* Unlike `supabase.auth.getSession()`, which returns the session _without_
* validating the JWT, this function also calls `getUser()` to validate the
* JWT before returning the session.
*/
event.locals.safeGetSession = async () => {
    const {
      data: { session },
    } = await event.locals.supabase.auth.getSession()
    if (!session) {
      return { session: null, user: null }
    }
    const {
      data: { user },
      error,
    } = await event.locals.supabase.auth.getUser()
    if (error) {
      // JWT validation has failed
      return { session: null, user: null }
    }
    return { session, user }
}
return resolve(event, {
    filterSerializedResponseHeaders(name) {
      /**
       * Supabase libraries use the `content-range` and `x-supabase-api-version`
       * headers, so we need to tell SvelteKit to pass it through.
       */
      return name === 'content-range' || name === 'x-supabase-api-version'
    },
})
}
const authGuard: Handle = async ({ event, resolve }) => {
const { session, user } = await event.locals.safeGetSession()
event.locals.session = session
event.locals.user = user
if (!event.locals.session && event.url.pathname.startsWith('/private')) {
    redirect(303, '/auth')
}
if (event.locals.session && event.url.pathname === '/auth') {
    redirect(303, '/private')
}
return resolve(event)
}
export const handle: Handle = sequence(supabase, authGuard)
``

4

### Create TypeScript definitions

To prevent TypeScript errors, add type definitions for the new `event.locals` properties.

src/app.d.ts

`
import type { Session, SupabaseClient, User } from '@supabase/supabase-js'
import type { Database } from './database.types.ts' // import generated types
declare global {
namespace App {
    // interface Error {}
    interface Locals {
      supabase: SupabaseClient<Database>
      safeGetSession: () => Promise<{ session: Session | null; user: User | null }>
      session: Session | null
      user: User | null
    }
    interface PageData {
      session: Session | null
    }
    // interface PageState {}
    // interface Platform {}
}
}
export {}
`

5

### Create a Supabase client in your root layout

Create a Supabase client in your root `+layout.ts`. This client can be used to access Supabase from the client or the server. In order to get access to the Auth token on the server, use a `+layout.server.ts` file to pass in the session from `event.locals`.

src/routes/+layout.ts

src/routes/+layout.server.ts

``
import { createBrowserClient, createServerClient, isBrowser } from '@supabase/ssr'
import { PUBLIC_SUPABASE_ANON_KEY, PUBLIC_SUPABASE_URL } from '$env/static/public'
import type { LayoutLoad } from './$types'
export const load: LayoutLoad = async ({ data, depends, fetch }) => {
/**
* Declare a dependency so the layout can be invalidated, for example, on
* session refresh.
*/
depends('supabase:auth')
const supabase = isBrowser()
    ? createBrowserClient(PUBLIC_SUPABASE_URL, PUBLIC_SUPABASE_ANON_KEY, {
        global: {
          fetch,
        },
      })
    : createServerClient(PUBLIC_SUPABASE_URL, PUBLIC_SUPABASE_ANON_KEY, {
        global: {
          fetch,
        },
        cookies: {
          getAll() {
            return data.cookies
          },
        },
      })
/**
* It's fine to use `getSession` here, because on the client, `getSession` is
* safe, and on the server, it reads `session` from the `LayoutData`, which
* safely checked the session using `safeGetSession`.
*/
const {
    data: { session },
} = await supabase.auth.getSession()
const {
    data: { user },
} = await supabase.auth.getUser()
return { session, supabase, user }
}
``

6

### Listen to Auth events

Set up a listener for Auth events on the client, to handle session refreshes and signouts.

src/routes/+layout.svelte

`
<script>
import { invalidate } from '$app/navigation'
import { onMount } from 'svelte'
let { data, children } = $props()
let { session, supabase } = $derived(data)
onMount(() => {
    const { data } = supabase.auth.onAuthStateChange((_, newSession) => {
      if (newSession?.expires_at !== session?.expires_at) {
        invalidate('supabase:auth')
      }
    })
    return () => data.subscription.unsubscribe()
})
</script>
{@render children()}
`

7

### Create your first page

Create your first page. This example page calls Supabase from the server to get a list of countries from the database.

This is an example of a public page that uses publicly readable data.

To populate your database, run the [colors quickstart](https://supabase.com/dashboard/project/_/sql/quickstarts) from your dashboard.

src/routes/+page.server.ts

src/routes/+page.svelte

`
import type { PageServerLoad } from './$types'
export const load: PageServerLoad = async ({ locals: { supabase } }) => {
const { data: colors } = await supabase.from('colors').select('name').limit(5).order('name')
return { colors: colors ?? [] }
}
`

8

### Change the Auth confirmation path

If you have email confirmation turned on (the default), a new user will receive an email confirmation after signing up.

Change the email template to support a server-side authentication flow.

Go to the [Auth templates](https://supabase.com/dashboard/project/_/auth/templates) page in your dashboard. In the `Confirm signup` template, change `{{ .ConfirmationURL }}` to `{{ .SiteURL }}/auth/confirm?token_hash={{ .TokenHash }}&type=email`.

9

### Create a login page

Next, create a login page to let users sign up and log in.

src/routes/auth/+page.server.ts

src/routes/auth/+page.svelte

src/routes/auth/+layout.svelte

src/routes/auth/error/+page.svelte

`
import { redirect } from '@sveltejs/kit'
import type { Actions } from './$types'
export const actions: Actions = {
signup: async ({ request, locals: { supabase } }) => {
    const formData = await request.formData()
    const email = formData.get('email') as string
    const password = formData.get('password') as string
    const { error } = await supabase.auth.signUp({ email, password })
    if (error) {
      console.error(error)
      redirect(303, '/auth/error')
    } else {
      redirect(303, '/')
    }
},
login: async ({ request, locals: { supabase } }) => {
    const formData = await request.formData()
    const email = formData.get('email') as string
    const password = formData.get('password') as string
    const { error } = await supabase.auth.signInWithPassword({ email, password })
    if (error) {
      console.error(error)
      redirect(303, '/auth/error')
    } else {
      redirect(303, '/private')
    }
},
}
`

10

### Create the signup confirmation route

Finish the signup flow by creating the API route to handle email verification.

src/routes/auth/confirm/+server.ts

``
import type { EmailOtpType } from '@supabase/supabase-js'
import { redirect } from '@sveltejs/kit'
import type { RequestHandler } from './$types'
export const GET: RequestHandler = async ({ url, locals: { supabase } }) => {
const token_hash = url.searchParams.get('token_hash')
const type = url.searchParams.get('type') as EmailOtpType | null
const next = url.searchParams.get('next') ?? '/'
/**
* Clean up the redirect URL by deleting the Auth flow parameters.
*
* `next` is preserved for now, because it's needed in the error case.
*/
const redirectTo = new URL(url)
redirectTo.pathname = next
redirectTo.searchParams.delete('token_hash')
redirectTo.searchParams.delete('type')
if (token_hash && type) {
    const { error } = await supabase.auth.verifyOtp({ type, token_hash })
    if (!error) {
      redirectTo.searchParams.delete('next')
      redirect(303, redirectTo)
    }
}
redirectTo.pathname = '/auth/error'
redirect(303, redirectTo)
}
``

11

### Create private routes

Create private routes that can only be accessed by authenticated users. The routes in the `private` directory are protected by the route guard in `hooks.server.ts`.

To ensure that `hooks.server.ts` runs for every nested path, put a `+layout.server.ts` file in the `private` directory. This file can be empty, but must exist to protect routes that don't have their own `+layout|page.server.ts`.

src/routes/private/+layout.server.ts

src/routes/private/+layout.svelte

SQL

src/routes/private/+page.server.ts

src/routes/private/+page.svelte

``
/**
* This file is necessary to ensure protection of all routes in the `private`
* directory. It makes the routes in this directory _dynamic_ routes, which
* send a server request, and thus trigger `hooks.server.ts`.
**/
``

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings