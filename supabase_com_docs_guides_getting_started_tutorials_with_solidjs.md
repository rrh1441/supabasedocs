Getting Started

# Build a User Management App with SolidJS

* * *

This tutorial demonstrates how to build a basic user management app. The app authenticates and identifies the user, stores their profile information in the database, and allows the user to log in, update their profile details, and upload a profile photo. The app uses:

- [Supabase Database](https://supabase.com/docs/guides/database) \- a Postgres database for storing your user data and [Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) so data is protected and users can only access their own information.
- [Supabase Auth](https://supabase.com/docs/guides/auth) \- allow users to sign up and log in.
- [Supabase Storage](https://supabase.com/docs/guides/storage) \- users can upload a profile photo.

![Supabase User Management example](https://supabase.com/docs/img/user-management-demo.png)

If you get stuck while working through this guide, refer to the [full example on GitHub](https://github.com/supabase/supabase/tree/master/examples/user-management/solid-user-management).

## Project setup [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#project-setup)

Before we start building we're going to set up our Database and API. This is as simple as starting a new Project in Supabase and then creating a "schema" inside the database.

### Create a project [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#create-a-project)

1. [Create a new project](https://supabase.com/dashboard) in the Supabase Dashboard.
2. Enter your project details.
3. Wait for the new database to launch.

### Set up the database schema [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#set-up-the-database-schema)

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

### Get the API Keys [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#get-the-api-keys)

Now that you've created some database tables, you are ready to insert data using the auto-generated API.
We just need to get the Project URL and `anon` key from the API settings.

1. Go to the [API Settings](https://supabase.com/dashboard/project/_/settings/api) page in the Dashboard.
2. Find your Project `URL`, `anon`, and `service_role` keys on this page.

## Building the app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#building-the-app)

Let's start building the SolidJS app from scratch.

### Initialize a SolidJS app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#initialize-a-solidjs-app)

We can use [degit](https://github.com/Rich-Harris/degit) to initialize an app called `supabase-solid`:

`
npx degit solidjs/templates/ts supabase-solid
cd supabase-solid
`

Then let's install the only additional dependency: [supabase-js](https://github.com/supabase/supabase-js)

`
npm install @supabase/supabase-js
`

And finally we want to save the environment variables in a `.env`.
All we need are the API URL and the `anon` key that you copied [earlier](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#get-the-api-keys).

.env

`
VITE_SUPABASE_URL=YOUR_SUPABASE_URL
VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
`

Now that we have the API credentials in place, let's create a helper file to initialize the Supabase client. These variables will be exposed
on the browser, and that's completely fine since we have [Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) enabled on our Database.

src/supabaseClient.tsx

`
import { createClient } from '@supabase/supabase-js'
const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY
export const supabase = createClient(supabaseUrl, supabaseAnonKey)
`

### App styling (optional) [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#app-styling-optional)

An optional step is to update the CSS file `src/index.css` to make the app look nice.
You can find the full contents of this file [here](https://raw.githubusercontent.com/supabase/supabase/master/examples/user-management/solid-user-management/src/index.css).

### Set up a login component [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#set-up-a-login-component)

Let's set up a SolidJS component to manage logins and sign ups. We'll use Magic Links, so users can sign in with their email without using passwords.

src/Auth.tsx

`
import { createSignal } from 'solid-js'
import { supabase } from './supabaseClient'
export default function Auth() {
const [loading, setLoading] = createSignal(false)
const [email, setEmail] = createSignal('')
const handleLogin = async (e: SubmitEvent) => {
    e.preventDefault()
    try {
      setLoading(true)
      const { error } = await supabase.auth.signInWithOtp({ email: email() })
      if (error) throw error
      alert('Check your email for the login link!')
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      setLoading(false)
    }
}
return (
    <div class="row flex-center flex">
      <div class="col-6 form-widget" aria-live="polite">
        <h1 class="header">Supabase + SolidJS</h1>
        <p class="description">Sign in via magic link with your email below</p>
        <form class="form-widget" onSubmit={handleLogin}>
          <div>
            <label for="email">Email</label>
            <input
              id="email"
              class="inputField"
              type="email"
              placeholder="Your email"
              value={email()}
              onChange={(e) => setEmail(e.currentTarget.value)}
            />
          </div>
          <div>
            <button type="submit" class="button block" aria-live="polite">
              {loading() ? <span>Loading</span> : <span>Send magic link</span>}
            </button>
          </div>
        </form>
      </div>
    </div>
)
}
`

### Account page [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#account-page)

After a user is signed in we can allow them to edit their profile details and manage their account.

Let's create a new component for that called `Account.tsx`.

src/Account.tsx

``
import { AuthSession } from '@supabase/supabase-js'
import { Component, createEffect, createSignal } from 'solid-js'
import { supabase } from './supabaseClient'
interface Props {
session: AuthSession
}
const Account: Component<Props> = ({ session }) => {
const [loading, setLoading] = createSignal(true)
const [username, setUsername] = createSignal<string | null>(null)
const [website, setWebsite] = createSignal<string | null>(null)
const [avatarUrl, setAvatarUrl] = createSignal<string | null>(null)
createEffect(() => {
    getProfile()
})
const getProfile = async () => {
    try {
      setLoading(true)
      const { user } = session
      const { data, error, status } = await supabase
        .from('profiles')
        .select(`username, website, avatar_url`)
        .eq('id', user.id)
        .single()
      if (error && status !== 406) {
        throw error
      }
      if (data) {
        setUsername(data.username)
        setWebsite(data.website)
        setAvatarUrl(data.avatar_url)
      }
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      setLoading(false)
    }
}
const updateProfile = async (e: Event) => {
    e.preventDefault()
    try {
      setLoading(true)
      const { user } = session
      const updates = {
        id: user.id,
        username: username(),
        website: website(),
        avatar_url: avatarUrl(),
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
      setLoading(false)
    }
}
return (
    <div aria-live="polite">
      <form onSubmit={updateProfile} class="form-widget">
        <div>Email: {session.user.email}</div>
        <div>
          <label for="username">Name</label>
          <input
            id="username"
            type="text"
            value={username() || ''}
            onChange={(e) => setUsername(e.currentTarget.value)}
          />
        </div>
        <div>
          <label for="website">Website</label>
          <input
            id="website"
            type="text"
            value={website() || ''}
            onChange={(e) => setWebsite(e.currentTarget.value)}
          />
        </div>
        <div>
          <button type="submit" class="button primary block" disabled={loading()}>
            {loading() ? 'Saving ...' : 'Update profile'}
          </button>
        </div>
        <button type="button" class="button block" onClick={() => supabase.auth.signOut()}>
          Sign Out
        </button>
      </form>
    </div>
)
}
export default Account
``

### Launch! [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#launch)

Now that we have all the components in place, let's update `App.tsx`:

src/App.tsx

`
import { Component, createEffect, createSignal } from 'solid-js'
import { supabase } from './supabaseClient'
import { AuthSession } from '@supabase/supabase-js'
import Account from './Account'
import Auth from './Auth'
const App: Component = () => {
const [session, setSession] = createSignal<AuthSession | null>(null)
createEffect(() => {
    supabase.auth.getSession().then(({ data: { session } }) => {
      setSession(session)
    })
    supabase.auth.onAuthStateChange((_event, session) => {
      setSession(session)
    })
})
return (
    <div class="container" style={{ padding: '50px 0 100px 0' }}>
      {!session() ? <Auth /> : <Account session={session()!} />}
    </div>
)
}
export default App
`

Once that's done, run this in a terminal window:

`
npm start
`

And then open the browser to [localhost:3000](http://localhost:3000/) and you should see the completed app.

![Supabase SolidJS](https://supabase.com/docs/img/supabase-solidjs-demo.png)

## Bonus: Profile photos [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#bonus-profile-photos)

Every Supabase project is configured with [Storage](https://supabase.com/docs/guides/storage) for managing large files like photos and videos.

### Create an upload widget [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#create-an-upload-widget)

Let's create an avatar for the user so that they can upload a profile photo. We can start by creating a new component:

src/Avatar.tsx

``
import { Component, createEffect, createSignal, JSX } from 'solid-js'
import { supabase } from './supabaseClient'
interface Props {
size: number
url: string | null
onUpload: (event: Event, filePath: string) => void
}
const Avatar: Component<Props> = (props) => {
const [avatarUrl, setAvatarUrl] = createSignal<string | null>(null)
const [uploading, setUploading] = createSignal(false)
createEffect(() => {
    if (props.url) downloadImage(props.url)
})
const downloadImage = async (path: string) => {
    try {
      const { data, error } = await supabase.storage.from('avatars').download(path)
      if (error) {
        throw error
      }
      const url = URL.createObjectURL(data)
      setAvatarUrl(url)
    } catch (error) {
      if (error instanceof Error) {
        console.log('Error downloading image: ', error.message)
      }
    }
}
const uploadAvatar: JSX.EventHandler<HTMLInputElement, Event> = async (event) => {
    try {
      setUploading(true)
      const target = event.currentTarget
      if (!target?.files || target.files.length === 0) {
        throw new Error('You must select an image to upload.')
      }
      const file = target.files[0]
      const fileExt = file.name.split('.').pop()
      const fileName = `${Math.random()}.${fileExt}`
      const filePath = `${fileName}`
      const { error: uploadError } = await supabase.storage.from('avatars').upload(filePath, file)
      if (uploadError) {
        throw uploadError
      }
      props.onUpload(event, filePath)
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      setUploading(false)
    }
}
return (
    <div style={{ width: `${props.size}px` }} aria-live="polite">
      {avatarUrl() ? (
        <img
          src={avatarUrl()!}
          alt={avatarUrl() ? 'Avatar' : 'No image'}
          class="avatar image"
          style={{ height: `${props.size}px`, width: `${props.size}px` }}
        />
      ) : (
        <div
          class="avatar no-image"
          style={{ height: `${props.size}px`, width: `${props.size}px` }}
        />
      )}
      <div style={{ width: `${props.size}px` }}>
        <label class="button primary block" for="single">
          {uploading() ? 'Uploading ...' : 'Upload avatar'}
        </label>
        <span style="display:none">
          <input
            type="file"
            id="single"
            accept="image/*"
            onChange={uploadAvatar}
            disabled={uploading()}
          />
        </span>
      </div>
    </div>
)
}
export default Avatar
``

### Add the new widget [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs\#add-the-new-widget)

And then we can add the widget to the Account page:

src/Account.tsx

`
// Import the new component
import Avatar from './Avatar'
// ...
return (
<form onSubmit={updateProfile} class="form-widget">
    {/* Add to the body */}
    <Avatar
      url={avatarUrl()}
      size={150}
      onUpload={(e: Event, url: string) => {
        setAvatarUrl(url)
        updateProfile(e)
      }}
    />
    {/* ... */}
</div>
)
`

At this stage you have a fully functional application!

### Is this helpful?

NoYes

### On this page

[Project setup](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#project-setup) [Create a project](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#create-a-project) [Set up the database schema](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#set-up-the-database-schema) [Get the API Keys](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#get-the-api-keys) [Building the app](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#building-the-app) [Initialize a SolidJS app](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#initialize-a-solidjs-app) [App styling (optional)](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#app-styling-optional) [Set up a login component](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#set-up-a-login-component) [Account page](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#account-page) [Launch!](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#launch) [Bonus: Profile photos](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#bonus-profile-photos) [Create an upload widget](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#create-an-upload-widget) [Add the new widget](https://supabase.com/docs/guides/getting-started/tutorials/with-solidjs#add-the-new-widget)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings