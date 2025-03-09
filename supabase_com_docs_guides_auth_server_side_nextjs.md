Auth

# Setting up Server-Side Auth for Next.js

* * *

Next.js comes in two flavors: the [App Router](https://nextjs.org/docs/app) and the [Pages Router](https://nextjs.org/docs/pages). You can set up Server-Side Auth with either strategy. You can even use both in the same application.

App RouterPages RouterHybrid router strategies

1

### Install Supabase packages

Install the `@supabase/supabase-js` package and the helper `@supabase/ssr` package.

`
npm install @supabase/supabase-js @supabase/ssr
`

2

### Set up environment variables

Create a `.env.local` file in your project root directory.

Fill in your `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY`:

###### Project URL

No project found

To get your Project URL, [log in](https://supabase.com/dashboard).

###### Anon key

No project found

To get your Anon key, [log in](https://supabase.com/dashboard).

.env.local

`
NEXT_PUBLIC_SUPABASE_URL=<your_supabase_project_url>
NEXT_PUBLIC_SUPABASE_ANON_KEY=<your_supabase_anon_key>
`

3

### Write utility functions to create Supabase clients

To access Supabase from your Next.js app, you need 2 types of Supabase clients:

1. **Client Component client** \- To access Supabase from Client Components, which run in the browser.
2. **Server Component client** \- To access Supabase from Server Components, Server Actions, and Route Handlers, which run only on the server.

Create a `utils/supabase` folder with a file for each type of client. Then copy the utility functions for each client type.

What does the \`cookies\` object do?

Do I need to create a new client for every route?

utils/supabase/client.ts

utils/supabase/server.ts

`
import { createBrowserClient } from '@supabase/ssr'
export function createClient() {
return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
)
}
`

4

### Hook up middleware

Create a `middleware.ts` file at the root of your project.

Since Server Components can't write cookies, you need middleware to refresh expired Auth tokens and store them.

The middleware is responsible for:

1. Refreshing the Auth token (by calling `supabase.auth.getUser`).
2. Passing the refreshed Auth token to Server Components, so they don't attempt to refresh the same token themselves. This is accomplished with `request.cookies.set`.
3. Passing the refreshed Auth token to the browser, so it replaces the old token. This is accomplished with `response.cookies.set`.

Copy the middleware code for your app.

Add a [matcher](https://nextjs.org/docs/app/building-your-application/routing/middleware#matching-paths) so the middleware doesn't run on routes that don't access Supabase.

Be careful when protecting pages. The server gets the user session from the cookies, which can be spoofed by anyone.

Always use `supabase.auth.getUser()` to protect pages and user data.

_Never_ trust `supabase.auth.getSession()` inside server code such as middleware. It isn't guaranteed to revalidate the Auth token.

It's safe to trust `getUser()` because it sends a request to the Supabase Auth server every time to revalidate the Auth token.

middleware.ts

utils/supabase/middleware.ts

`
import { type NextRequest } from 'next/server'
import { updateSession } from '@/utils/supabase/middleware'
export async function middleware(request: NextRequest) {
return await updateSession(request)
}
export const config = {
matcher: [\
    /*\
     * Match all request paths except for the ones starting with:\
     * - _next/static (static files)\
     * - _next/image (image optimization files)\
     * - favicon.ico (favicon file)\
     * Feel free to modify this pattern to include more paths.\
     */\
    '/((?!_next/static|_next/image|favicon.ico|.*\\.(?:svg|png|jpg|jpeg|gif|webp)$).*)',\
],
}
`

5

### Create a login page

Create a login page for your app. Use a Server Action to call the Supabase signup function.

Since Supabase is being called from an Action, use the client defined in `@/utils/supabase/server.ts`.

Note that `cookies` is called before any calls to Supabase, which opts fetch calls out of Next.js's caching. This is important for authenticated data fetches, to ensure that users get access only to their own data.

See the Next.js docs to learn more about [opting out of data caching](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#opting-out-of-data-caching).

app/login/page.tsx

app/login/actions.ts

app/error/page.tsx

`
import { login, signup } from './actions'
export default function LoginPage() {
return (
    <form>
      <label htmlFor="email">Email:</label>
      <input id="email" name="email" type="email" required />
      <label htmlFor="password">Password:</label>
      <input id="password" name="password" type="password" required />
      <button formAction={login}>Log in</button>
      <button formAction={signup}>Sign up</button>
    </form>
)
}
`

6

### Change the Auth confirmation path

If you have email confirmation turned on (the default), a new user will receive an email confirmation after signing up.

Change the email template to support a server-side authentication flow.

Go to the [Auth templates](https://supabase.com/dashboard/project/_/auth/templates) page in your dashboard. In the `Confirm signup` template, change `{{ .ConfirmationURL }}` to `{{ .SiteURL }}/auth/confirm?token_hash={{ .TokenHash }}&type=email`.

7

### Create a route handler for Auth confirmation

Create a Route Handler for `auth/confirm`. When a user clicks their confirmation email link, exchange their secure code for an Auth token.

Since this is a Router Handler, use the Supabase client from `@/utils/supabase/server.ts`.

app/auth/confirm/route.ts

`
import { type EmailOtpType } from '@supabase/supabase-js'
import { type NextRequest } from 'next/server'
import { createClient } from '@/utils/supabase/server'
import { redirect } from 'next/navigation'
export async function GET(request: NextRequest) {
const { searchParams } = new URL(request.url)
const token_hash = searchParams.get('token_hash')
const type = searchParams.get('type') as EmailOtpType | null
const next = searchParams.get('next') ?? '/'
if (token_hash && type) {
    const supabase = await createClient()
    const { error } = await supabase.auth.verifyOtp({
      type,
      token_hash,
    })
    if (!error) {
      // redirect user to specified redirect URL or root of app
      redirect(next)
    }
}
// redirect the user to an error page with some instructions
redirect('/error')
}
`

8

### Access user info from Server Component

Server Components can read cookies, so you can get the Auth status and user info.

Since you're calling Supabase from a Server Component, use the client created in `@/utils/supabase/server.ts`.

Create a `private` page that users can only access if they're logged in. The page displays their email.

Be careful when protecting pages. The server gets the user session from the cookies, which can be spoofed by anyone.

Always use `supabase.auth.getUser()` to protect pages and user data.

_Never_ trust `supabase.auth.getSession()` inside Server Components. It isn't guaranteed to revalidate the Auth token.

It's safe to trust `getUser()` because it sends a request to the Supabase Auth server every time to revalidate the Auth token.

app/private/page.tsx

`
import { redirect } from 'next/navigation'
import { createClient } from '@/utils/supabase/server'
export default async function PrivatePage() {
const supabase = await createClient()
const { data, error } = await supabase.auth.getUser()
if (error || !data?.user) {
    redirect('/login')
}
return <p>Hello {data.user.email}</p>
}
`

## Congratulations [\#](https://supabase.com/docs/guides/auth/server-side/nextjs\#congratulations)

You're done! To recap, you've successfully:

- Called Supabase from a Server Action.
- Called Supabase from a Server Component.
- Set up a Supabase client utility to call Supabase from a Client Component. You can use this if you need to call Supabase from a Client Component, for example to set up a realtime subscription.
- Set up middleware to automatically refresh the Supabase Auth session.

You can now use any Supabase features from your client or server code!

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings