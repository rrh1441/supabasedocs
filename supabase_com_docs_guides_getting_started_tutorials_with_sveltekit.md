Getting Started

# Build a User Management App with SvelteKit

* * *

This tutorial demonstrates how to build a basic user management app. The app authenticates and identifies the user, stores their profile information in the database, and allows the user to log in, update their profile details, and upload a profile photo. The app uses:

- [Supabase Database](https://supabase.com/docs/guides/database) \- a Postgres database for storing your user data and [Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) so data is protected and users can only access their own information.
- [Supabase Auth](https://supabase.com/docs/guides/auth) \- allow users to sign up and log in.
- [Supabase Storage](https://supabase.com/docs/guides/storage) \- users can upload a profile photo.

![Supabase User Management example](https://supabase.com/docs/img/user-management-demo.png)

If you get stuck while working through this guide, refer to the [full example on GitHub](https://github.com/supabase/supabase/tree/master/examples/user-management/sveltekit-user-management).

## Project setup [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#project-setup)

Before we start building we're going to set up our Database and API. This is as simple as starting a new Project in Supabase and then creating a "schema" inside the database.

### Create a project [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#create-a-project)

1. [Create a new project](https://supabase.com/dashboard) in the Supabase Dashboard.
2. Enter your project details.
3. Wait for the new database to launch.

### Set up the database schema [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#set-up-the-database-schema)

Now we are going to set up the database schema. We can use the "User Management Starter" quickstart in the SQL Editor, or you can just copy/paste the SQL from below and run it yourself.

DashboardSQL

1. Go to the [SQL Editor](https://supabase.com/dashboard/project/_/sql) page in the Dashboard.
2. Click **User Management Starter**.
3. Click **Run**.

You can easily pull the database schema down to your local project by running the `db pull` command. Read the [local development docs](https://supabase.com/docs/guides/cli/local-development#link-your-project) for detailed instructions.

`
supabase link --project-ref <project-id>
# You can get <project-id> from your project's dashboard URL: https://supabase.com/dashboard/project/<project-id>
supabase db pull
`

### Get the API Keys [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#get-the-api-keys)

Now that you've created some database tables, you are ready to insert data using the auto-generated API.
We just need to get the Project URL and `anon` key from the API settings.

1. Go to the [API Settings](https://supabase.com/dashboard/project/_/settings/api) page in the Dashboard.
2. Find your Project `URL`, `anon`, and `service_role` keys on this page.

## Building the app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#building-the-app)

Let's start building the Svelte app from scratch.

### Initialize a Svelte app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#initialize-a-svelte-app)

We can use the [SvelteKit Skeleton Project](https://kit.svelte.dev/docs) to initialize an app called `supabase-sveltekit` (for this tutorial we will be using TypeScript):

`
npm create svelte@latest supabase-sveltekit
cd supabase-sveltekit
npm install
`

Then install the Supabase client library: [supabase-js](https://github.com/supabase/supabase-js)

`
npm install @supabase/supabase-js
`

And finally we want to save the environment variables in a `.env`.
All we need are the `SUPABASE_URL` and the `SUPABASE_KEY` key that you copied [earlier](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#get-the-api-keys).

.env

`
PUBLIC_SUPABASE_URL="YOUR_SUPABASE_URL"
PUBLIC_SUPABASE_ANON_KEY="YOUR_SUPABASE_KEY"
`

Optionally, add `src/styles.css` with the [CSS from the example](https://raw.githubusercontent.com/supabase/supabase/master/examples/user-management/sveltekit-user-management/src/styles.css).

### Creating a Supabase client for SSR [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#creating-a-supabase-client-for-ssr)

The `ssr` package configures Supabase to use Cookies, which is required for server-side languages and frameworks.

Install the Supabase packages:

`
npm install @supabase/ssr @supabase/supabase-js
`

Creating a Supabase client with the `ssr` package automatically configures it to use Cookies. This means your user's session is available throughout the entire SvelteKit stack - page, layout, server, hooks.

Add the code below to your `src/hooks.server.ts` to initialize the client on the server:

src/hooks.server.ts

``
// src/hooks.server.ts
import { PUBLIC_SUPABASE_URL, PUBLIC_SUPABASE_ANON_KEY } from '$env/static/public'
import { createServerClient } from '@supabase/ssr'
import type { Handle } from '@sveltejs/kit'
export const handle: Handle = async ({ event, resolve }) => {
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
      return name === 'content-range' || name === 'x-supabase-api-version'
    },
})
}
``

Note that `auth.getSession` reads the auth token and the unencoded session data from the local storage medium. It _doesn't_ send a request back to the Supabase Auth server unless the local session is expired.

You should **never** trust the unencoded session data if you're writing server code, since it could be tampered with by the sender. If you need verified, trustworthy user data, call `auth.getUser` instead, which always makes a request to the Auth server to fetch trusted data.

If you are using TypeScript the compiler might complain about `event.locals.supabase` and `event.locals.safeGetSession`, this can be fixed by updating your `src/app.d.ts` with the content below:

src/app.d.ts

`
// src/app.d.ts
import { SupabaseClient, Session } from '@supabase/supabase-js'
declare global {
namespace App {
    interface Locals {
      supabase: SupabaseClient
      safeGetSession(): Promise<{ session: Session | null; user: User | null }>
    }
    interface PageData {
      session: Session | null
      user: User | null
    }
    // interface Error {}
    // interface Platform {}
}
}
`

Create a new `src/routes/+layout.server.ts` file to handle the session on the server-side.

src/routes/+layout.server.ts

`
// src/routes/+layout.server.ts
import type { LayoutServerLoad } from './$types'
export const load: LayoutServerLoad = async ({ locals: { safeGetSession }, cookies }) => {
const { session, user } = await safeGetSession()
return {
    session,
    user,
    cookies: cookies.getAll(),
}
}
`

Start your dev server ( `npm run dev`) in order to generate the `./$types` files we are referencing in our project.

Create a new `src/routes/+layout.ts` file to handle the session and the `supabase` object on the client-side.

src/routes/+layout.ts

``
// src/routes/+layout.ts
import { createBrowserClient, createServerClient, isBrowser } from '@supabase/ssr'
import { PUBLIC_SUPABASE_ANON_KEY, PUBLIC_SUPABASE_URL } from '$env/static/public'
import type { LayoutLoad } from './$types'
export const load: LayoutLoad = async ({ fetch, data, depends }) => {
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
return { supabase, session }
}
``

Update your `src/routes/+layout.svelte`:

src/routes/+layout.svelte

`
<!-- src/routes/+layout.svelte -->
<script lang="ts">
	import '../styles.css'
	import { invalidate } from '$app/navigation'
	import { onMount } from 'svelte'
	export let data
	let { supabase, session } = data
	$: ({ supabase, session } = data)
	onMount(() => {
		const { data } = supabase.auth.onAuthStateChange((event, newSession) => {
			if (newSession?.expires_at !== session?.expires_at) {
				invalidate('supabase:auth')
			}
		})
		return () => data.subscription.unsubscribe()
	})
</script>
<svelte:head>
	<title>User Management</title>
</svelte:head>
<div class="container" style="padding: 50px 0 100px 0">
	<slot />
</div>
`

## Set up a login page [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#set-up-a-login-page)

Create a magic link login/signup page for your application:

src/routes/+page.svelte

`
<!-- src/routes/+page.svelte -->
<script lang="ts">
	import { enhance } from '$app/forms'
	import type { ActionData, SubmitFunction } from './$types.js'
	export let form: ActionData;
	let loading = false
	const handleSubmit: SubmitFunction = () => {
		loading = true
		return async ({ update }) => {
			update()
			loading = false
		}
	}
</script>
<svelte:head>
	<title>User Management</title>
</svelte:head>
<form class="row flex flex-center" method="POST" use:enhance={handleSubmit}>
	<div class="col-6 form-widget">
		<h1 class="header">Supabase + SvelteKit</h1>
		<p class="description">Sign in via magic link with your email below</p>
		{#if form?.message !== undefined}
		<div class="success {form?.success ? '' : 'fail'}">
			{form?.message}
		</div>
		{/if}
		<div>
			<label for="email">Email address</label>
			<input
				id="email"
				name="email"
				class="inputField"
				type="email"
				placeholder="Your email"
				value={form?.email ?? ''}
			/>
		</div>
		{#if form?.errors?.email}
		<span class="flex items-center text-sm error">
			{form?.errors?.email}
		</span>
		{/if}
		<div>
			<button class="button primary block">
				{ loading ? 'Loading' : 'Send magic link' }
			</button>
		</div>
	</div>
</form>
`

Create a `src/routes/+page.server.ts` file that will handle our magic link form when submitted.

src/routes/+page.server.ts

``
// src/routes/+page.server.ts
import { fail, redirect } from '@sveltejs/kit'
import type { Actions, PageServerLoad } from './$types'
export const load: PageServerLoad = async ({ url, locals: { safeGetSession } }) => {
const { session } = await safeGetSession()
// if the user is already logged in return them to the account page
if (session) {
    redirect(303, '/account')
}
return { url: url.origin }
}
export const actions: Actions = {
default: async (event) => {
    const {
      url,
      request,
      locals: { supabase },
    } = event
    const formData = await request.formData()
    const email = formData.get('email') as string
    const validEmail = /^[\w-\.+]+@([\w-]+\.)+[\w-]{2,8}$/.test(email)
    if (!validEmail) {
      return fail(400, { errors: { email: 'Please enter a valid email address' }, email })
    }
    const { error } = await supabase.auth.signInWithOtp({ email })
    if (error) {
      return fail(400, {
        success: false,
        email,
        message: `There was an issue, Please contact support.`,
      })
    }
    return {
      success: true,
      message: 'Please check your email for a magic link to log into the website.',
    }
},
}
``

### Email template [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#email-template)

Change the email template to support a server-side authentication flow.

Before we proceed, let's change the email template to support sending a token hash:

- Go to the [Auth templates](https://supabase.com/dashboard/project/_/auth/templates) page in your dashboard.
- Select `Confirm signup` template.
- Change `{{ .ConfirmationURL }}` to `{{ .SiteURL }}/auth/confirm?token_hash={{ .TokenHash }}&type=email`.
- Repeat the previous step for `Magic link` template.

Did you know? You could also customize emails sent out to new users, including the email's looks, content, and query parameters. Check out the [settings of your project](https://supabase.com/dashboard/project/_/auth/templates).

### Confirmation endpoint [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#confirmation-endpoint)

As we are working in a server-side rendering (SSR) environment, it is necessary to create a server endpoint responsible for exchanging the `token_hash` for a session.

In the following code snippet, we perform the following steps:

- Retrieve the `token_hash` sent back from the Supabase Auth server using the `token_hash` query parameter.
- Exchange this `token_hash` for a session, which we store in storage (in this case, cookies).
- Finally, the user is redirected to the `account` page or the `error` page.

src/routes/auth/confirm/+server.ts

``
// src/routes/auth/confirm/+server.ts
import type { EmailOtpType } from '@supabase/supabase-js'
import { redirect } from '@sveltejs/kit'
import type { RequestHandler } from './$types'
export const GET: RequestHandler = async ({ url, locals: { supabase } }) => {
const token_hash = url.searchParams.get('token_hash')
const type = url.searchParams.get('type') as EmailOtpType | null
const next = url.searchParams.get('next') ?? '/account'
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

### Authentication error page [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#authentication-error-page)

If there is an error with confirming the token you will be redirect to this error page.

src/routes/auth/error/+page.svelte

`
<p>Login error</p>
`

### Account page [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#account-page)

After a user is signed in, they need to be able to edit their profile details and manage their account.
Create a new `src/routes/account/+page.svelte` file with the content below.

src/routes/account/+page.svelte

`
<!-- src/routes/account/+page.svelte -->
<script lang="ts">
	import { enhance } from '$app/forms';
	import type { SubmitFunction } from '@sveltejs/kit';
	export let data
	export let form
	let { session, supabase, profile } = data
	$: ({ session, supabase, profile } = data)
	let profileForm: HTMLFormElement
	let loading = false
	let fullName: string = profile?.full_name ?? ''
	let username: string = profile?.username ?? ''
	let website: string = profile?.website ?? ''
	let avatarUrl: string = profile?.avatar_url ?? ''
	const handleSubmit: SubmitFunction = () => {
		loading = true
		return async () => {
			loading = false
		}
	}
	const handleSignOut: SubmitFunction = () => {
		loading = true
		return async ({ update }) => {
			loading = false
			update()
		}
	}
</script>
<div class="form-widget">
	<form
		class="form-widget"
		method="post"
		action="?/update"
		use:enhance={handleSubmit}
		bind:this={profileForm}
	>
		<div>
			<label for="email">Email</label>
			<input id="email" type="text" value={session.user.email} disabled />
		</div>
		<div>
			<label for="fullName">Full Name</label>
			<input id="fullName" name="fullName" type="text" value={form?.fullName ?? fullName} />
		</div>
		<div>
			<label for="username">Username</label>
			<input id="username" name="username" type="text" value={form?.username ?? username} />
		</div>
		<div>
			<label for="website">Website</label>
			<input id="website" name="website" type="url" value={form?.website ?? website} />
		</div>
		<div>
			<input
				type="submit"
				class="button block primary"
				value={loading ? 'Loading...' : 'Update'}
				disabled={loading}
			/>
		</div>
	</form>
	<form method="post" action="?/signout" use:enhance={handleSignOut}>
		<div>
			<button class="button block" disabled={loading}>Sign Out</button>
		</div>
	</form>
</div>
`

Now create the associated `src/routes/account/+page.server.ts` file that will handle loading our data from the server through the `load` function
and handle all our form actions through the `actions` object.

``
import { fail, redirect } from '@sveltejs/kit'
import type { Actions, PageServerLoad } from './$types'
export const load: PageServerLoad = async ({ locals: { supabase, safeGetSession } }) => {
const { session } = await safeGetSession()
if (!session) {
    redirect(303, '/')
}
const { data: profile } = await supabase
    .from('profiles')
    .select(`username, full_name, website, avatar_url`)
    .eq('id', session.user.id)
    .single()
return { session, profile }
}
export const actions: Actions = {
update: async ({ request, locals: { supabase, safeGetSession } }) => {
    const formData = await request.formData()
    const fullName = formData.get('fullName') as string
    const username = formData.get('username') as string
    const website = formData.get('website') as string
    const avatarUrl = formData.get('avatarUrl') as string
    const { session } = await safeGetSession()
    const { error } = await supabase.from('profiles').upsert({
      id: session?.user.id,
      full_name: fullName,
      username,
      website,
      avatar_url: avatarUrl,
      updated_at: new Date(),
    })
    if (error) {
      return fail(500, {
        fullName,
        username,
        website,
        avatarUrl,
      })
    }
    return {
      fullName,
      username,
      website,
      avatarUrl,
    }
},
signout: async ({ locals: { supabase, safeGetSession } }) => {
    const { session } = await safeGetSession()
    if (session) {
      await supabase.auth.signOut()
      redirect(303, '/')
    }
},
}
``

### Launch! [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#launch)

Now that we have all the pages in place, run this in a terminal window:

`
npm run dev
`

And then open the browser to [localhost:5173](http://localhost:5173/) and you should see the completed app.

![Supabase Svelte](https://supabase.com/docs/img/supabase-svelte-demo.png)

## Bonus: Profile photos [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#bonus-profile-photos)

Every Supabase project is configured with [Storage](https://supabase.com/docs/guides/storage) for managing large files like photos and videos.

### Create an upload widget [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#create-an-upload-widget)

Let's create an avatar for the user so that they can upload a profile photo. We can start by creating a new component called `Avatar.svelte` in the `src/routes/account` directory:

src/routes/account/Avatar.svelte

``
<!-- src/routes/account/Avatar.svelte -->
<script lang="ts">
	import type { SupabaseClient } from '@supabase/supabase-js'
	import { createEventDispatcher } from 'svelte'
	export let size = 10
	export let url: string
	export let supabase: SupabaseClient
	let avatarUrl: string | null = null
	let uploading = false
	let files: FileList
	const dispatch = createEventDispatcher()
	const downloadImage = async (path: string) => {
		try {
			const { data, error } = await supabase.storage.from('avatars').download(path)
			if (error) {
				throw error
			}
			const url = URL.createObjectURL(data)
			avatarUrl = url
		} catch (error) {
			if (error instanceof Error) {
				console.log('Error downloading image: ', error.message)
			}
		}
	}
	const uploadAvatar = async () => {
		try {
			uploading = true
			if (!files || files.length === 0) {
				throw new Error('You must select an image to upload.')
			}
			const file = files[0]
			const fileExt = file.name.split('.').pop()
			const filePath = `${Math.random()}.${fileExt}`
			const { error } = await supabase.storage.from('avatars').upload(filePath, file)
			if (error) {
				throw error
			}
			url = filePath
			setTimeout(() => {
				dispatch('upload')
			}, 100)
		} catch (error) {
			if (error instanceof Error) {
				alert(error.message)
			}
		} finally {
			uploading = false
		}
	}
	$: if (url) downloadImage(url)
</script>
<div>
	{#if avatarUrl}
		<img
			src={avatarUrl}
			alt={avatarUrl ? 'Avatar' : 'No image'}
			class="avatar image"
			style="height: {size}em; width: {size}em;"
		/>
	{:else}
		<div class="avatar no-image" style="height: {size}em; width: {size}em;" />
	{/if}
	<input type="hidden" name="avatarUrl" value={url} />
	<div style="width: {size}em;">
		<label class="button primary block" for="single">
			{uploading ? 'Uploading ...' : 'Upload'}
		</label>
		<input
			style="visibility: hidden; position:absolute;"
			type="file"
			id="single"
			accept="image/*"
			bind:files
			on:change={uploadAvatar}
			disabled={uploading}
		/>
	</div>
</div>
``

### Add the new widget [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit\#add-the-new-widget)

And then we can add the widget to the Account page:

src/routes/account/+page.svelte

`
<!-- src/routes/account/+page.svelte -->
<script lang="ts">
// Import the new component
import Avatar from './Avatar.svelte'
</script>
<div class="form-widget">
<form
    class="form-widget"
    method="post"
    action="?/update"
    use:enhance={handleSubmit}
    bind:this={profileForm}
>
    <!-- Add to body -->
    <Avatar
        {supabase}
        bind:url={avatarUrl}
        size={10}
        on:upload={() => {
          profileForm.requestSubmit();
        }}
      />
    <!-- Other form elements -->
</form>
</div>
`

At this stage you have a fully functional application!

### Is this helpful?

NoYes

### On this page

[Project setup](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#project-setup) [Create a project](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#create-a-project) [Set up the database schema](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#set-up-the-database-schema) [Get the API Keys](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#get-the-api-keys) [Building the app](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#building-the-app) [Initialize a Svelte app](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#initialize-a-svelte-app) [Creating a Supabase client for SSR](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#creating-a-supabase-client-for-ssr) [Set up a login page](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#set-up-a-login-page) [Email template](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#email-template) [Confirmation endpoint](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#confirmation-endpoint) [Authentication error page](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#authentication-error-page) [Account page](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#account-page) [Launch!](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#launch) [Bonus: Profile photos](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#bonus-profile-photos) [Create an upload widget](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#create-an-upload-widget) [Add the new widget](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit#add-the-new-widget)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings