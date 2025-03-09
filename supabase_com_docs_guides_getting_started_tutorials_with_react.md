Getting Started

# Build a User Management App with React

* * *

This tutorial demonstrates how to build a basic user management app. The app authenticates and identifies the user, stores their profile information in the database, and allows the user to log in, update their profile details, and upload a profile photo. The app uses:

- [Supabase Database](https://supabase.com/docs/guides/database) \- a Postgres database for storing your user data and [Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) so data is protected and users can only access their own information.
- [Supabase Auth](https://supabase.com/docs/guides/auth) \- allow users to sign up and log in.
- [Supabase Storage](https://supabase.com/docs/guides/storage) \- users can upload a profile photo.

![Supabase User Management example](https://supabase.com/docs/img/user-management-demo.png)

If you get stuck while working through this guide, refer to the [full example on GitHub](https://github.com/supabase/supabase/tree/master/examples/user-management/react-user-management).

## Project setup [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#project-setup)

Before we start building we're going to set up our Database and API. This is as simple as starting a new Project in Supabase and then creating a "schema" inside the database.

### Create a project [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#create-a-project)

1. [Create a new project](https://supabase.com/dashboard) in the Supabase Dashboard.
2. Enter your project details.
3. Wait for the new database to launch.

### Set up the database schema [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#set-up-the-database-schema)

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

### Get the API Keys [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#get-the-api-keys)

Now that you've created some database tables, you are ready to insert data using the auto-generated API.
We just need to get the Project URL and `anon` key from the API settings.

1. Go to the [API Settings](https://supabase.com/dashboard/project/_/settings/api) page in the Dashboard.
2. Find your Project `URL`, `anon`, and `service_role` keys on this page.

## Building the app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#building-the-app)

Let's start building the React app from scratch.

### Initialize a React app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#initialize-a-react-app)

We can use [Vite](https://vitejs.dev/guide/) to initialize
an app called `supabase-react`:

`
npm create vite@latest supabase-react -- --template react
cd supabase-react
`

Then let's install the only additional dependency: [supabase-js](https://github.com/supabase/supabase-js).

`
npm install @supabase/supabase-js
`

And finally we want to save the environment variables in a `.env.local` file.
All we need are the API URL and the `anon` key that you copied [earlier](https://supabase.com/docs/guides/getting-started/tutorials/with-react#get-the-api-keys).

.env

`
VITE_SUPABASE_URL=YOUR_SUPABASE_URL
VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
`

Now that we have the API credentials in place, let's create a helper file to initialize the Supabase client. These variables will be exposed
on the browser, and that's completely fine since we have [Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) enabled on our Database.

Create and edit `src/supabaseClient.js`:

src/supabaseClient.js

`
import { createClient } from '@supabase/supabase-js'
const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY
export const supabase = createClient(supabaseUrl, supabaseAnonKey)
`

### App styling (optional) [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#app-styling-optional)

An optional step is to update the CSS file `src/index.css` to make the app look nice.
You can find the full contents of this file [here](https://raw.githubusercontent.com/supabase/supabase/master/examples/user-management/react-user-management/src/index.css).

### Set up a login component [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#set-up-a-login-component)

Let's set up a React component to manage logins and sign ups. We'll use Magic Links, so users can sign in with their email without using passwords.

Create and edit `src/Auth.jsx`:

src/Auth.jsx

`
import { useState } from 'react'
import { supabase } from './supabaseClient'
export default function Auth() {
const [loading, setLoading] = useState(false)
const [email, setEmail] = useState('')
const handleLogin = async (event) => {
    event.preventDefault()
    setLoading(true)
    const { error } = await supabase.auth.signInWithOtp({ email })
    if (error) {
      alert(error.error_description || error.message)
    } else {
      alert('Check your email for the login link!')
    }
    setLoading(false)
}
return (
    <div className="row flex flex-center">
      <div className="col-6 form-widget">
        <h1 className="header">Supabase + React</h1>
        <p className="description">Sign in via magic link with your email below</p>
        <form className="form-widget" onSubmit={handleLogin}>
          <div>
            <input
              className="inputField"
              type="email"
              placeholder="Your email"
              value={email}
              required={true}
              onChange={(e) => setEmail(e.target.value)}
            />
          </div>
          <div>
            <button className={'button block'} disabled={loading}>
              {loading ? <span>Loading</span> : <span>Send magic link</span>}
            </button>
          </div>
        </form>
      </div>
    </div>
)
}
`

### Account page [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#account-page)

After a user is signed in we can allow them to edit their profile details and manage their account.

Let's create a new component for that called `src/Account.jsx`.

src/Account.jsx

``
import { useState, useEffect } from 'react'
import { supabase } from './supabaseClient'
export default function Account({ session }) {
const [loading, setLoading] = useState(true)
const [username, setUsername] = useState(null)
const [website, setWebsite] = useState(null)
const [avatar_url, setAvatarUrl] = useState(null)
useEffect(() => {
    let ignore = false
    async function getProfile() {
      setLoading(true)
      const { user } = session
      const { data, error } = await supabase
        .from('profiles')
        .select(`username, website, avatar_url`)
        .eq('id', user.id)
        .single()
      if (!ignore) {
        if (error) {
          console.warn(error)
        } else if (data) {
          setUsername(data.username)
          setWebsite(data.website)
          setAvatarUrl(data.avatar_url)
        }
      }
      setLoading(false)
    }
    getProfile()
    return () => {
      ignore = true
    }
}, [session])
async function updateProfile(event, avatarUrl) {
    event.preventDefault()
    setLoading(true)
    const { user } = session
    const updates = {
      id: user.id,
      username,
      website,
      avatar_url: avatarUrl,
      updated_at: new Date(),
    }
    const { error } = await supabase.from('profiles').upsert(updates)
    if (error) {
      alert(error.message)
    } else {
      setAvatarUrl(avatarUrl)
    }
    setLoading(false)
}
return (
    <form onSubmit={updateProfile} className="form-widget">
      <div>
        <label htmlFor="email">Email</label>
        <input id="email" type="text" value={session.user.email} disabled />
      </div>
      <div>
        <label htmlFor="username">Name</label>
        <input
          id="username"
          type="text"
          required
          value={username || ''}
          onChange={(e) => setUsername(e.target.value)}
        />
      </div>
      <div>
        <label htmlFor="website">Website</label>
        <input
          id="website"
          type="url"
          value={website || ''}
          onChange={(e) => setWebsite(e.target.value)}
        />
      </div>
      <div>
        <button className="button block primary" type="submit" disabled={loading}>
          {loading ? 'Loading ...' : 'Update'}
        </button>
      </div>
      <div>
        <button className="button block" type="button" onClick={() => supabase.auth.signOut()}>
          Sign Out
        </button>
      </div>
    </form>
)
}
``

### Launch! [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#launch)

Now that we have all the components in place, let's update `src/App.jsx`:

src/App.jsx

`
import './App.css'
import { useState, useEffect } from 'react'
import { supabase } from './supabaseClient'
import Auth from './Auth'
import Account from './Account'
function App() {
const [session, setSession] = useState(null)
useEffect(() => {
    supabase.auth.getSession().then(({ data: { session } }) => {
      setSession(session)
    })
    supabase.auth.onAuthStateChange((_event, session) => {
      setSession(session)
    })
}, [])
return (
    <div className="container" style={{ padding: '50px 0 100px 0' }}>
      {!session ? <Auth /> : <Account key={session.user.id} session={session} />}
    </div>
)
}
export default App
`

Once that's done, run this in a terminal window:

`
npm run dev
`

And then open the browser to [localhost:5173](http://localhost:5173/) and you should see the completed app.

![Supabase React](https://supabase.com/docs/img/supabase-react-demo.png)

## Bonus: Profile photos [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#bonus-profile-photos)

Every Supabase project is configured with [Storage](https://supabase.com/docs/guides/storage) for managing large files like photos and videos.

### Create an upload widget [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#create-an-upload-widget)

Let's create an avatar for the user so that they can upload a profile photo. We can start by creating a new component:

Create and edit `src/Avatar.jsx`:

src/Avatar.jsx

``
import { useEffect, useState } from 'react'
import { supabase } from './supabaseClient'
export default function Avatar({ url, size, onUpload }) {
const [avatarUrl, setAvatarUrl] = useState(null)
const [uploading, setUploading] = useState(false)
useEffect(() => {
    if (url) downloadImage(url)
}, [url])
async function downloadImage(path) {
    try {
      const { data, error } = await supabase.storage.from('avatars').download(path)
      if (error) {
        throw error
      }
      const url = URL.createObjectURL(data)
      setAvatarUrl(url)
    } catch (error) {
      console.log('Error downloading image: ', error.message)
    }
}
async function uploadAvatar(event) {
    try {
      setUploading(true)
      if (!event.target.files || event.target.files.length === 0) {
        throw new Error('You must select an image to upload.')
      }
      const file = event.target.files[0]
      const fileExt = file.name.split('.').pop()
      const fileName = `${Math.random()}.${fileExt}`
      const filePath = `${fileName}`
      const { error: uploadError } = await supabase.storage.from('avatars').upload(filePath, file)
      if (uploadError) {
        throw uploadError
      }
      onUpload(event, filePath)
    } catch (error) {
      alert(error.message)
    } finally {
      setUploading(false)
    }
}
return (
    <div>
      {avatarUrl ? (
        <img
          src={avatarUrl}
          alt="Avatar"
          className="avatar image"
          style={{ height: size, width: size }}
        />
      ) : (
        <div className="avatar no-image" style={{ height: size, width: size }} />
      )}
      <div style={{ width: size }}>
        <label className="button primary block" htmlFor="single">
          {uploading ? 'Uploading ...' : 'Upload'}
        </label>
        <input
          style={{
            visibility: 'hidden',
            position: 'absolute',
          }}
          type="file"
          id="single"
          accept="image/*"
          onChange={uploadAvatar}
          disabled={uploading}
        />
      </div>
    </div>
)
}
``

### Add the new widget [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-react\#add-the-new-widget)

And then we can add the widget to the Account page at `src/Account.jsx`:

src/Account.jsx

`
// Import the new component
import Avatar from './Avatar'
// ...
return (
<form onSubmit={updateProfile} className="form-widget">
    {/* Add to the body */}
    <Avatar
      url={avatar_url}
      size={150}
      onUpload={(event, url) => {
        updateProfile(event, url)
      }}
    />
    {/* ... */}
</form>
)
`

At this stage you have a fully functional application!

### Is this helpful?

NoYes

### On this page

[Project setup](https://supabase.com/docs/guides/getting-started/tutorials/with-react#project-setup) [Create a project](https://supabase.com/docs/guides/getting-started/tutorials/with-react#create-a-project) [Set up the database schema](https://supabase.com/docs/guides/getting-started/tutorials/with-react#set-up-the-database-schema) [Get the API Keys](https://supabase.com/docs/guides/getting-started/tutorials/with-react#get-the-api-keys) [Building the app](https://supabase.com/docs/guides/getting-started/tutorials/with-react#building-the-app) [Initialize a React app](https://supabase.com/docs/guides/getting-started/tutorials/with-react#initialize-a-react-app) [App styling (optional)](https://supabase.com/docs/guides/getting-started/tutorials/with-react#app-styling-optional) [Set up a login component](https://supabase.com/docs/guides/getting-started/tutorials/with-react#set-up-a-login-component) [Account page](https://supabase.com/docs/guides/getting-started/tutorials/with-react#account-page) [Launch!](https://supabase.com/docs/guides/getting-started/tutorials/with-react#launch) [Bonus: Profile photos](https://supabase.com/docs/guides/getting-started/tutorials/with-react#bonus-profile-photos) [Create an upload widget](https://supabase.com/docs/guides/getting-started/tutorials/with-react#create-an-upload-widget) [Add the new widget](https://supabase.com/docs/guides/getting-started/tutorials/with-react#add-the-new-widget)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings