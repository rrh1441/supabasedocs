Getting Started

# Build a User Management App with Svelte

* * *

This tutorial demonstrates how to build a basic user management app. The app authenticates and identifies the user, stores their profile information in the database, and allows the user to log in, update their profile details, and upload a profile photo. The app uses:

- [Supabase Database](https://supabase.com/docs/guides/database) \- a Postgres database for storing your user data and [Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) so data is protected and users can only access their own information.
- [Supabase Auth](https://supabase.com/docs/guides/auth) \- allow users to sign up and log in.
- [Supabase Storage](https://supabase.com/docs/guides/storage) \- users can upload a profile photo.

![Supabase User Management example](https://supabase.com/docs/img/user-management-demo.png)

If you get stuck while working through this guide, refer to the [full example on GitHub](https://github.com/supabase/supabase/tree/master/examples/user-management/svelte-user-management).

## Project setup [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#project-setup)

Before we start building we're going to set up our Database and API. This is as simple as starting a new Project in Supabase and then creating a "schema" inside the database.

### Create a project [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#create-a-project)

1. [Create a new project](https://supabase.com/dashboard) in the Supabase Dashboard.
2. Enter your project details.
3. Wait for the new database to launch.

### Set up the database schema [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#set-up-the-database-schema)

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

### Get the API Keys [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#get-the-api-keys)

Now that you've created some database tables, you are ready to insert data using the auto-generated API.
We just need to get the Project URL and `anon` key from the API settings.

1. Go to the [API Settings](https://supabase.com/dashboard/project/_/settings/api) page in the Dashboard.
2. Find your Project `URL`, `anon`, and `service_role` keys on this page.

## Building the app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#building-the-app)

Let's start building the Svelte app from scratch.

### Initialize a Svelte app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#initialize-a-svelte-app)

We can use the Vite Svelte TypeScript Template to initialize an app called `supabase-svelte`:

`
npm create vite@latest supabase-svelte -- --template svelte-ts
cd supabase-svelte
npm install
`

Then let's install the only additional dependency: [supabase-js](https://github.com/supabase/supabase-js)

`
npm install @supabase/supabase-js
`

And finally we want to save the environment variables in a `.env`.
All we need are the API URL and the `anon` key that you copied [earlier](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#get-the-api-keys).

.env

`
VITE_SUPABASE_URL=YOUR_SUPABASE_URL
VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
`

Now that we have the API credentials in place, let's create a helper file to initialize the Supabase client. These variables will be exposed
on the browser, and that's completely fine since we have [Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) enabled on our Database.

src/supabaseClient.ts

`
import { createClient } from '@supabase/supabase-js'
const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY
export const supabase = createClient(supabaseUrl, supabaseAnonKey)
`

### App styling (optional) [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#app-styling-optional)

An optional step is to update the CSS file `src/app.css` to make the app look nice.
You can find the full contents of this file [here](https://raw.githubusercontent.com/supabase/supabase/master/examples/user-management/svelte-user-management/src/app.css).

### Set up a login component [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#set-up-a-login-component)

Let's set up a Svelte component to manage logins and sign ups. We'll use Magic Links, so users can sign in with their email without using passwords.

src/lib/Auth.svelte

`
<script lang="ts">
import { supabase } from '../supabaseClient'
let loading = false
let email = ''
const handleLogin = async () => {
    try {
      loading = true
      const { error } = await supabase.auth.signInWithOtp({ email })
      if (error) throw error
      alert('Check your email for login link!')
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      loading = false
    }
}
</script>
<div class="row flex-center flex">
<div class="col-6 form-widget" aria-live="polite">
    <h1 class="header">Supabase + Svelte</h1>
    <p class="description">Sign in via magic link with your email below</p>
    <form class="form-widget" on:submit|preventDefault="{handleLogin}">
      <div>
        <label for="email">Email</label>
        <input
          id="email"
          class="inputField"
          type="email"
          placeholder="Your email"
          bind:value="{email}"
        />
      </div>
      <div>
        <button type="submit" class="button block" aria-live="polite" disabled="{loading}">
          <span>{loading ? 'Loading' : 'Send magic link'}</span>
        </button>
      </div>
    </form>
</div>
</div>
`

### Account page [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#account-page)

After a user is signed in we can allow them to edit their profile details and manage their account.
Let's create a new component for that called `Account.svelte`.

src/lib/Account.svelte

`
<script lang="ts">
import { onMount } from 'svelte'
import type { AuthSession } from '@supabase/supabase-js'
import { supabase } from '../supabaseClient'
export let session: AuthSession
let loading = false
let username: string | null = null
let website: string | null = null
let avatarUrl: string | null = null
onMount(() => {
    getProfile()
})
const getProfile = async () => {
    try {
      loading = true
      const { user } = session
      const { data, error, status } = await supabase
        .from('profiles')
        .select('username, website, avatar_url')
        .eq('id', user.id)
        .single()
      if (error && status !== 406) throw error
      if (data) {
        username = data.username
        website = data.website
        avatarUrl = data.avatar_url
      }
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      loading = false
    }
}
const updateProfile = async () => {
    try {
      loading = true
      const { user } = session
      const updates = {
        id: user.id,
        username,
        website,
        avatar_url: avatarUrl,
        updated_at: new Date().toISOString(),
      }
      const { error } = await supabase.from('profiles').upsert(updates)
      if (error) {
        throw error
      }
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      loading = false
    }
}
</script>
<form on:submit|preventDefault="{updateProfile}" class="form-widget">
<div>Email: {session.user.email}</div>
<div>
    <label for="username">Name</label>
    <input id="username" type="text" bind:value="{username}" />
</div>
<div>
    <label for="website">Website</label>
    <input id="website" type="text" bind:value="{website}" />
</div>
<div>
    <button type="submit" class="button primary block" disabled="{loading}">
      {loading ? 'Saving ...' : 'Update profile'}
    </button>
</div>
<button type="button" class="button block" on:click={() => supabase.auth.signOut()}> Sign Out
</button>
</form>
`

### Launch! [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#launch)

Now that we have all the components in place, let's update `App.svelte`:

src/App.svelte

`
<script lang="ts">
import { onMount } from 'svelte'
import { supabase } from './supabaseClient'
import type { AuthSession } from '@supabase/supabase-js'
import Account from './lib/Account.svelte'
import Auth from './lib/Auth.svelte'
let session: AuthSession | null
onMount(() => {
    supabase.auth.getSession().then(({ data }) => {
      session = data.session
    })
    supabase.auth.onAuthStateChange((_event, _session) => {
      session = _session
    })
})
</script>
<div class="container" style="padding: 50px 0 100px 0">
{#if !session}
<Auth />
{:else}
<Account {session} />
{/if}
</div>
`

Once that's done, run this in a terminal window:

`
npm run dev
`

And then open the browser to [localhost:5173](http://localhost:5173/) and you should see the completed app.

Svelte uses Vite and the default port is `5173`, Supabase uses `port 3000`. To change the redirection port for Supabase go to: `Authentication > Settings` and change the `Site Url` to `http://localhost:5173/`

![Supabase Svelte](https://supabase.com/docs/img/supabase-svelte-demo.png)

## Bonus: Profile photos [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#bonus-profile-photos)

Every Supabase project is configured with [Storage](https://supabase.com/docs/guides/storage) for managing large files like photos and videos.

### Create an upload widget [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#create-an-upload-widget)

Let's create an avatar for the user so that they can upload a profile photo. We can start by creating a new component:

src/lib/Avatar.svelte

``
<script lang="ts">
import { createEventDispatcher } from 'svelte'
import { supabase } from '../supabaseClient'
export let size: number
export let url: string | null = null
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
      dispatch('upload')
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
<div style="width: {size}px" aria-live="polite">
{#if avatarUrl} <img src={avatarUrl} alt={avatarUrl ? 'Avatar' : 'No image'} class="avatar image"
style="height: {size}px, width: {size}px" /> {:else}
<div class="avatar no-image" style="height: {size}px, width: {size}px" />
{/if}
<div style="width: {size}px">
    <label class="button primary block" for="single">
      {uploading ? 'Uploading ...' : 'Upload avatar'}
    </label>
    <span style="display:none">
      <input
        type="file"
        id="single"
        accept="image/*"
        bind:files
        on:change={uploadAvatar}
        disabled={uploading}
      />
    </span>
</div>
</div>
``

### Add the new widget [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte\#add-the-new-widget)

And then we can add the widget to the Account page:

src/lib/Account.svelte

`
<script lang="ts">
// Import the new component
import Avatar from './Avatar.svelte'
</script>
<form on:submit|preventDefault="{updateProfile}" class="form-widget">
<!-- Add to body -->
<Avatar bind:url="{avatarUrl}" size="{150}" on:upload="{updateProfile}" />
<!-- Other form elements -->
</form>
`

At this stage you have a fully functional application!

### Is this helpful?

NoYes

### On this page

[Project setup](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#project-setup) [Create a project](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#create-a-project) [Set up the database schema](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#set-up-the-database-schema) [Get the API Keys](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#get-the-api-keys) [Building the app](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#building-the-app) [Initialize a Svelte app](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#initialize-a-svelte-app) [App styling (optional)](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#app-styling-optional) [Set up a login component](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#set-up-a-login-component) [Account page](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#account-page) [Launch!](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#launch) [Bonus: Profile photos](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#bonus-profile-photos) [Create an upload widget](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#create-an-upload-widget) [Add the new widget](https://supabase.com/docs/guides/getting-started/tutorials/with-svelte#add-the-new-widget)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings