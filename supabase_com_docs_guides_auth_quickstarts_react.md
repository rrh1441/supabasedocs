Auth

# Use Supabase Auth with React

## Learn how to use Supabase Auth with React.js.

* * *

1

### Create a new Supabase project

[Launch a new project](https://supabase.com/dashboard) in the Supabase Dashboard.

Your new database has a table for storing your users. You can see that this table is currently empty by running some SQL in the [SQL Editor](https://supabase.com/dashboard/project/_/sql).

SQL\_EDITOR

`
select * from auth.users;
`

2

### Create a React app

Create a React app using the `create-react-app` command.

Terminal

`
npx create-react-app my-app
`

3

### Install the Supabase client library

The fastest way to get started is to use Supabase's `auth-ui-react` library which provides a convenient interface for working with Supabase Auth from a React app.

Navigate to the React app and install the Supabase libraries.

Terminal

`
cd my-app && npm install @supabase/supabase-js @supabase/auth-ui-react @supabase/auth-ui-shared
`

4

### Set up your login component

In `App.js`, create a Supabase client using your [Project URL and public API (anon) key](https://supabase.com/dashboard/project/_/settings/api).

You can configure the Auth component to display whenever there is no session inside `supabase.auth.getSession()`

src/App.js

`
import './index.css'
import { useState, useEffect } from 'react'
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'
import { ThemeSupa } from '@supabase/auth-ui-shared'
const supabase = createClient('https://<project>.supabase.co', '<your-anon-key>')
export default function App() {
    const [session, setSession] = useState(null)
    useEffect(() => {
      supabase.auth.getSession().then(({ data: { session } }) => {
        setSession(session)
      })
      const {
        data: { subscription },
      } = supabase.auth.onAuthStateChange((_event, session) => {
        setSession(session)
      })
      return () => subscription.unsubscribe()
    }, [])
    if (!session) {
      return (<Auth supabaseClient={supabase} appearance={{ theme: ThemeSupa }} />)
    }
    else {
      return (<div>Logged in!</div>)
    }
}
`

5

### Start the app

Start the app, go to [http://localhost:3000](http://localhost:3000/) in a browser, and open the browser console and you should be able to log in.

Terminal

`
npm start
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings