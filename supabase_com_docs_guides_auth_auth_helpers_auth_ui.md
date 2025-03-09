Auth

# Auth UI

* * *

As of 7th Feb 2024, [this repository](https://github.com/supabase-community/auth-ui) is no longer maintained by the Supabase Team. At the moment, the team does not have capacity to give the expected level of care to this repository. We may revisit Auth UI in the future but regrettably have to leave it on hold for now as we focus on other priorities such as improving the Server-Side Rendering (SSR) package and advanced Auth primitives.

Auth UI is a pre-built React component for authenticating users.
It supports custom themes and extensible styles to match your brand and aesthetic.

## Set up Auth UI [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#set-up-auth-ui)

Install the latest version of [supabase-js](https://supabase.com/docs/reference/javascript) and the Auth UI package:

`
npm install @supabase/supabase-js @supabase/auth-ui-react @supabase/auth-ui-shared
`

### Import the Auth component [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#import-the-auth-component)

Pass `supabaseClient` from `@supabase/supabase-js` as a prop to the component.

/src/index.js

`
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'
const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')
const App = () => <Auth supabaseClient={supabase} />
`

This renders the Auth component without any styling.
We recommend using one of the predefined themes to style the UI.
Import the theme you want to use and pass it to the `appearance.theme` prop.

`
import { Auth } from '@supabase/auth-ui-react'
import {
// Import predefined theme
ThemeSupa,
} from '@supabase/auth-ui-shared'
const supabase = createClient(
'<INSERT PROJECT URL>',
'<INSERT PROJECT ANON API KEY>'
)
const App = () => (
<Auth
    supabaseClient={supabase}
    {/* Apply predefined theme */}
    appearance={{ theme: ThemeSupa }}
/>
)
`

### Social providers [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#social-providers)

The Auth component also supports login with [official social providers](https://supabase.com/docs/guides/auth#providers).

`
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'
import { ThemeSupa } from '@supabase/auth-ui-shared'
const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')
const App = () => (
<Auth
    supabaseClient={supabase}
    appearance={{ theme: ThemeSupa }}
    providers={['google', 'facebook', 'twitter']}
/>
)
`

### Options [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#options)

Options are available via `queryParams`:

`
<Auth
supabaseClient={supabase}
providers={['google']}
queryParams={{
    access_type: 'offline',
    prompt: 'consent',
    hd: 'domain.com',
}}
onlyThirdPartyProviders
/>
`

### Provider scopes [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#provider-scopes)

Provider Scopes can be requested through `providerScope`;

`
<Auth
supabaseClient={supabase}
providers={['google']}
queryParams={{
    access_type: 'offline',
    prompt: 'consent',
    hd: 'domain.com',
}}
providerScopes={{
    google: 'https://www.googleapis.com/auth/calendar.readonly',
}}
/>
`

### Supported views [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#supported-views)

The Auth component is currently shipped with the following views:

- [Email Login](https://supabase.com/docs/guides/auth/auth-email)
- [Magic Link login](https://supabase.com/docs/guides/auth/auth-magic-link)
- [Social Login](https://supabase.com/docs/guides/auth/social-login)
- Update password
- Forgotten password

We are planning on adding more views in the future. Follow along on that [repo](https://github.com/supabase/auth-ui).

## Customization [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#customization)

There are several ways to customize Auth UI:

- Use one of the [predefined themes](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#predefined-themes) that comes with Auth UI
- Extend a theme by [overriding the variable tokens](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#override-themes) in a theme
- [Create your own theme](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#create-theme)
- [Use your own CSS classes](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#custom-css-classes)
- [Use inline styles](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#custom-inline-styles)
- [Use your own labels](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#custom-labels)

### Predefined themes [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#predefined-themes)

Auth UI comes with several themes to customize the appearance. Each predefined theme comes with at least two variations, a `default` variation, and a `dark` variation. You can switch between these themes using the `theme` prop. Import the theme you want to use and pass it to the `appearance.theme` prop.

`
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'
import { ThemeSupa } from '@supabase/auth-ui-shared'
const supabase = createClient(
'<INSERT PROJECT URL>',
'<INSERT PROJECT ANON API KEY>'
)
const App = () => (
<Auth
    supabaseClient={supabase}
    {/* Apply predefined theme */}
    appearance={{ theme: ThemeSupa }}
/>
)
`

Currently there is only one predefined theme available, but we plan to add more.

### Switch theme variations [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#switch-theme-variations)

Auth UI comes with two theme variations: `default` and `dark`. You can switch between these themes with the `theme` prop.

`
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'
import { ThemeSupa } from '@supabase/auth-ui-shared'
const supabase = createClient(
'<INSERT PROJECT URL>',
'<INSERT PROJECT ANON API KEY>'
)
const App = () => (
<Auth
    supabaseClient={supabase}
    appearance={{ theme: ThemeSupa }}
    {/* Set theme to dark */}
    theme="dark"
/>
)
`

If you don't pass a value to `theme` it uses the `"default"` theme. You can pass `"dark"` to the theme prop to switch to the `dark` theme. If your theme has other variations, use the name of the variation in this prop.

### Override themes [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#override-themes)

Auth UI themes can be overridden using variable tokens. See the [list of variable tokens](https://github.com/supabase/auth-ui/blob/main/packages/shared/src/theming/Themes.ts).

`
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'
import { ThemeSupa } from '@supabase/auth-ui-shared'
const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')
const App = () => (
<Auth
    supabaseClient={supabase}
    appearance={{
      theme: ThemeSupa,
      variables: {
        default: {
          colors: {
            brand: 'red',
            brandAccent: 'darkred',
          },
        },
      },
    }}
/>
)
`

If you created your own theme, you may not need to override any of them.

### Create your own theme [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#create-theme)

You can create your own theme by following the same structure within a `appearance.theme` property.
See the list of [tokens within a theme](https://github.com/supabase/auth-ui/blob/main/packages/shared/src/theming/Themes.ts).

/src/index.js

`
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'
const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')
const customTheme = {
default: {
    colors: {
      brand: 'hsl(153 60.0% 53.0%)',
      brandAccent: 'hsl(154 54.8% 45.1%)',
      brandButtonText: 'white',
      // ..
    },
},
dark: {
    colors: {
      brandButtonText: 'white',
      defaultButtonBackground: '#2e2e2e',
      defaultButtonBackgroundHover: '#3e3e3e',
      //..
    },
},
// You can also add more theme variations with different names.
evenDarker: {
    colors: {
      brandButtonText: 'white',
      defaultButtonBackground: '#1e1e1e',
      defaultButtonBackgroundHover: '#2e2e2e',
      //..
    },
},
}
const App = () => (
<Auth
    supabaseClient={supabase}
    theme="default" // can also be "dark" or "evenDarker"
    appearance={{ theme: customTheme }}
/>
)
`

You can switch between different variations of your theme with the ["theme" prop](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#switch-theme-variations).

### Custom CSS classes [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#custom-css-classes)

You can use custom CSS classes for the following elements:
`"button"`, `"container"`, `"anchor"`, `"divider"`, `"label"`, `"input"`, `"loader"`, `"message"`.

/src/index.js

`
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'
const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')
const App = () => (
<Auth
    supabaseClient={supabase}
    appearance={{
      // If you want to extend the default styles instead of overriding it, set this to true
      extend: false,
      // Your custom classes
      className: {
        anchor: 'my-awesome-anchor',
        button: 'my-awesome-button',
        //..
      },
    }}
/>
)
`

### Custom inline CSS [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#custom-inline-styles)

You can use custom CSS inline styles for the following elements:
`"button"`, `"container"`, `"anchor"`, `"divider"`, `"label"`, `"input"`, `"loader"`, `"message"`.

/src/index.js

`
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'
const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')
const App = () => (
<Auth
    supabaseClient={supabase}
    appearance={{
      style: {
        button: { background: 'red', color: 'white' },
        anchor: { color: 'blue' },
        //..
      },
    }}
/>
)
`

### Custom labels [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#custom-labels)

You can use custom labels with `localization.variables` like so:

`
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'
const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')
const App = () => (
<Auth
    supabaseClient={supabase}
    localization={{
      variables: {
        sign_in: {
          email_label: 'Your email address',
          password_label: 'Your strong password',
        },
      },
    }}
/>
)
`

A full list of the available variables is below:

Sign UpSign InMagic LinkForgotten PasswordUpdate PasswordVerify OTP

| Label Tag | Default Label |
| --- | --- |
| `email_label` | Email address |
| `password_label` | Create a Password |
| `email_input_placeholder` | Your email address |
| `password_input_placeholder` | Your password |
| `button_label` | Sign up |
| `loading_button_label` | Signing up ... |
| `social_provider_text` | Sign in with `{{provider}}` |
| `link_text` | Don't have an account? Sign up |
| `confirmation_text` | Check your email for the confirmation link |

Currently, translating error messages (e.g. "Invalid credentials") is not supported. Check [related issue.](https://github.com/supabase/auth-ui/issues/86)

### Hiding links [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#hiding-links)

You can hide links by setting the `showLinks` prop to `false`

`
import { createClient } from '@supabase/supabase-js'
import { Auth } from '@supabase/auth-ui-react'
const supabase = createClient('<INSERT PROJECT URL>', '<INSERT PROJECT ANON API KEY>')
const App = () => <Auth supabaseClient={supabase} showLinks={false} />
`

Setting `showLinks` to `false` will hide the following links:

- Don't have an account? Sign up
- Already have an account? Sign in
- Send a magic link email
- Forgot your password?

### Sign in and sign up views [\#](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui\#sign-in-and-sign-up-views)

Add `sign_in` or `sign_up` views with the `view` prop:

`
<Auth
supabaseClient={supabase}
view="sign_up"
/>
`

### Is this helpful?

NoYes

### On this page

[Set up Auth UI](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#set-up-auth-ui) [Import the Auth component](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#import-the-auth-component) [Social providers](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#social-providers) [Options](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#options) [Provider scopes](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#provider-scopes) [Supported views](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#supported-views) [Customization](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#customization) [Predefined themes](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#predefined-themes) [Switch theme variations](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#switch-theme-variations) [Override themes](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#override-themes) [Create your own theme](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#create-theme) [Custom CSS classes](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#custom-css-classes) [Custom inline CSS](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#custom-inline-styles) [Custom labels](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#custom-labels) [Hiding links](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#hiding-links) [Sign in and sign up views](https://supabase.com/docs/guides/auth/auth-helpers/auth-ui#sign-in-and-sign-up-views)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings