Getting Started

# Build a User Management App with Ionic Vue

* * *

This tutorial demonstrates how to build a basic user management app. The app authenticates and identifies the user, stores their profile information in the database, and allows the user to log in, update their profile details, and upload a profile photo. The app uses:

- [Supabase Database](https://supabase.com/docs/guides/database) \- a Postgres database for storing your user data and [Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) so data is protected and users can only access their own information.
- [Supabase Auth](https://supabase.com/docs/guides/auth) \- allow users to sign up and log in.
- [Supabase Storage](https://supabase.com/docs/guides/storage) \- users can upload a profile photo.

![Supabase User Management example](https://supabase.com/docs/img/ionic-demos/ionic-angular-account.png)

If you get stuck while working through this guide, refer to the [full example on GitHub](https://github.com/mhartington/supabase-ionic-vue).

## Project setup [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue\#project-setup)

Before we start building we're going to set up our Database and API. This is as simple as starting a new Project in Supabase and then creating a "schema" inside the database.

### Create a project [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue\#create-a-project)

1. [Create a new project](https://supabase.com/dashboard) in the Supabase Dashboard.
2. Enter your project details.
3. Wait for the new database to launch.

### Set up the database schema [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue\#set-up-the-database-schema)

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

### Get the API Keys [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue\#get-the-api-keys)

Now that you've created some database tables, you are ready to insert data using the auto-generated API.
We just need to get the Project URL and `anon` key from the API settings.

1. Go to the [API Settings](https://supabase.com/dashboard/project/_/settings/api) page in the Dashboard.
2. Find your Project `URL`, `anon`, and `service_role` keys on this page.

## Building the app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue\#building-the-app)

Let's start building the Vue app from scratch.

### Initialize an Ionic Vue app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue\#initialize-an-ionic-vue-app)

We can use the [Ionic CLI](https://ionicframework.com/docs/cli) to initialize
an app called `supabase-ionic-vue`:

`
npm install -g @ionic/cli
ionic start supabase-ionic-vue blank --type vue
cd supabase-ionic-vue
`

Then let's install the only additional dependency: [supabase-js](https://github.com/supabase/supabase-js)

`
npm install @supabase/supabase-js
`

And finally we want to save the environment variables in a `.env`.
All we need are the API URL and the `anon` key that you copied [earlier](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#get-the-api-keys).

.env

`
VUE_APP_SUPABASE_URL=YOUR_SUPABASE_URL
VUE_APP_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
`

Now that we have the API credentials in place, let's create a helper file to initialize the Supabase client. These variables will be exposed
on the browser, and that's completely fine since we have [Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) enabled on our Database.

src/supabase.ts"

`
import { createClient } from '@supabase/supabase-js';
const supabaseUrl = process.env.VUE_APP_SUPABASE_URL as string;
const supabaseAnonKey = process.env.VUE_APP_SUPABASE_ANON_KEY as string;
export const supabase = createClient(supabaseUrl, supabaseAnonKey);
`

### Set up a login route [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue\#set-up-a-login-route)

Let's set up a Vue component to manage logins and sign ups. We'll use Magic Links, so users can sign in with their email without using passwords.

/src/views/Login.vue

`
<template>
<ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Login</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <div class="ion-padding">
        <h1>Supabase + Ionic Vue</h1>
        <p>Sign in via magic link with your email below</p>
      </div>
      <ion-list inset="true">
        <form @submit.prevent="handleLogin">
          <ion-item>
            <ion-label position="stacked">Email</ion-label>
            <ion-input v-model="email" name="email" autocomplete type="email"></ion-input>
          </ion-item>
          <div class="ion-text-center">
            <ion-button type="submit" fill="clear">Login</ion-button>
          </div>
        </form>
      </ion-list>
      <p>{{email}}</p>
    </ion-content>
</ion-page>
</template>
<script lang="ts">
import { supabase } from '../supabase'
import {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonList,
    IonItem,
    IonLabel,
    IonInput,
    IonButton,
    toastController,
    loadingController,
} from '@ionic/vue'
import { defineComponent, ref } from 'vue'
export default defineComponent({
    name: 'LoginPage',
    components: {
      IonContent,
      IonHeader,
      IonPage,
      IonTitle,
      IonToolbar,
      IonList,
      IonItem,
      IonLabel,
      IonInput,
      IonButton,
    },
    setup() {
      const email = ref('')
      const handleLogin = async () => {
        const loader = await loadingController.create({})
        const toast = await toastController.create({ duration: 5000 })
        try {
          await loader.present()
          const { error } = await supabase.auth.signIn({ email: email.value })
          if (error) throw error
          toast.message = 'Check your email for the login link!'
          await toast.present()
        } catch (error: any) {
          toast.message = error.error_description || error.message
          await toast.present()
        } finally {
          await loader.dismiss()
        }
      }
      return { handleLogin, email }
    },
})
</script>
`

### Account page [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue\#account-page)

After a user is signed in we can allow them to edit their profile details and manage their account.

Let's create a new component for that called `Account.vue`.

src/views/Account.vue

``
<template>
<ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Account</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <form @submit.prevent="updateProfile">
        <ion-item>
          <ion-label>
            <p>Email</p>
            <p>{{ session?.user?.email }}</p>
          </ion-label>
        </ion-item>
        <ion-item>
          <ion-label position="stacked">Name</ion-label>
          <ion-input type="text" name="username" v-model="profile.username"></ion-input>
        </ion-item>
        <ion-item>
          <ion-label position="stacked">Website</ion-label>
          <ion-input type="url" name="website" v-model="profile.website"></ion-input>
        </ion-item>
        <div class="ion-text-center">
          <ion-button fill="clear" type="submit">Update Profile</ion-button>
        </div>
      </form>
      <div class="ion-text-center">
        <ion-button fill="clear" @click="signOut">Log Out</ion-button>
      </div>
    </ion-content>
</ion-page>
</template>
<script lang="ts">
import { store } from '@/store'
import { supabase } from '@/supabase'
import {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    toastController,
    loadingController,
    IonInput,
    IonItem,
    IonButton,
    IonLabel,
} from '@ionic/vue'
import { User } from '@supabase/supabase-js'
import { defineComponent, onMounted, ref } from 'vue'
export default defineComponent({
    name: 'AccountPage',
    components: {
      IonContent,
      IonHeader,
      IonPage,
      IonTitle,
      IonToolbar,
      IonInput,
      IonItem,
      IonButton,
      IonLabel,
    },
    setup() {
      const session = ref(supabase.auth.session())
      const profile = ref({
        username: '',
        website: '',
        avatar_url: '',
      })
      const user = store.user as User
      async function getProfile() {
        const loader = await loadingController.create({})
        const toast = await toastController.create({ duration: 5000 })
        await loader.present()
        try {
          const { data, error, status } = await supabase
            .from('profiles')
            .select(`username, website, avatar_url`)
            .eq('id', user.id)
            .single()
          if (error && status !== 406) throw error
          if (data) {
            console.log(data)
            profile.value = {
              username: data.username,
              website: data.website,
              avatar_url: data.avatar_url,
            }
          }
        } catch (error: any) {
          toast.message = error.message
          await toast.present()
        } finally {
          await loader.dismiss()
        }
      }
      const updateProfile = async () => {
        const loader = await loadingController.create({})
        const toast = await toastController.create({ duration: 5000 })
        try {
          await loader.present()
          const updates = {
            id: user.id,
            ...profile.value,
            updated_at: new Date(),
          }
          //
          const { error } = await supabase.from('profiles').upsert(updates, {
            returning: 'minimal', // Don't return the value after inserting
          })
          //
          if (error) throw error
        } catch (error: any) {
          toast.message = error.message
          await toast.present()
        } finally {
          await loader.dismiss()
        }
      }
      async function signOut() {
        const loader = await loadingController.create({})
        const toast = await toastController.create({ duration: 5000 })
        await loader.present()
        try {
          const { error } = await supabase.auth.signOut()
          if (error) throw error
        } catch (error: any) {
          toast.message = error.message
          await toast.present()
        } finally {
          await loader.dismiss()
        }
      }
      onMounted(() => {
        getProfile()
      })
      return { signOut, profile, session, updateProfile }
    },
})
</script>
``

### Launch! [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue\#launch)

Now that we have all the components in place, let's update `App.vue` and our routes:

src/router.index.ts

`
import { createRouter, createWebHistory } from '@ionic/vue-router'
import { RouteRecordRaw } from 'vue-router'
import LoginPage from '../views/Login.vue'
import AccountPage from '../views/Account.vue'
const routes: Array<RouteRecordRaw> = [\
{\
    path: '/',\
    name: 'Login',\
    component: LoginPage,\
},\
{\
    path: '/account',\
    name: 'Account',\
    component: AccountPage,\
},\
]
const router = createRouter({
history: createWebHistory(process.env.BASE_URL),
routes,
})
export default router
`

src/App.vue

`
<template>
<ion-app>
    <ion-router-outlet />
</ion-app>
</template>
<script lang="ts">
import { IonApp, IonRouterOutlet, useIonRouter } from '@ionic/vue'
import { defineComponent } from 'vue'
import { store } from './store'
import { supabase } from './supabase'
export default defineComponent({
    name: 'App',
    components: {
      IonApp,
      IonRouterOutlet,
    },
    setup() {
      const router = useIonRouter()
      store.user = supabase.auth.user() ?? {}
      supabase.auth.onAuthStateChange((_, session) => {
        store.user = session?.user ?? {}
        if (session?.user) {
          router.replace('/account')
        }
      })
    },
})
</script>
`

Once that's done, run this in a terminal window:

`
ionic serve
`

And then open the browser to [localhost:3000](http://localhost:3000/) and you should see the completed app.

![Supabase Ionic Vue](https://supabase.com/docs/img/ionic-demos/ionic-vue.png)

## Bonus: Profile photos [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue\#bonus-profile-photos)

Every Supabase project is configured with [Storage](https://supabase.com/docs/guides/storage) for managing large files like photos and videos.

### Create an upload widget [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue\#create-an-upload-widget)

First install two packages in order to interact with the user's camera.

`
npm install @ionic/pwa-elements @capacitor/camera
`

[Capacitor](https://capacitorjs.com/) is a cross-platform native runtime from Ionic that enables web apps to be deployed through the app store and provides access to native device API.

Ionic PWA elements is a companion package that will polyfill certain browser APIs that provide no user interface with custom Ionic UI.

With those packages installed we can update our `main.ts` to include an additional bootstrapping call for the Ionic PWA Elements.

src/main.tsx"

`
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import { IonicVue } from '@ionic/vue'
/* Core CSS required for Ionic components to work properly */
import '@ionic/vue/css/ionic.bundle.css'
/* Theme variables */
import './theme/variables.css'
import { defineCustomElements } from '@ionic/pwa-elements/loader'
defineCustomElements(window)
const app = createApp(App).use(IonicVue).use(router)
router.isReady().then(() => {
app.mount('#app')
})
`

Then create an `AvatarComponent`.

src/components/Avatar.vue

``
<template>
<div class="avatar">
    <div class="avatar_wrapper" @click="uploadAvatar">
      <img v-if="avatarUrl" :src="avatarUrl" />
      <ion-icon v-else name="person" class="no-avatar"></ion-icon>
    </div>
</div>
</template>
<script lang="ts">
import { ref, toRefs, watch, defineComponent } from 'vue'
import { supabase } from '../supabase'
import { Camera, CameraResultType } from '@capacitor/camera'
import { IonIcon } from '@ionic/vue'
import { person } from 'ionicons/icons'
export default defineComponent({
    name: 'AppAvatar',
    props: { path: String },
    emits: ['upload', 'update:path'],
    components: { IonIcon },
    setup(prop, { emit }) {
      const { path } = toRefs(prop)
      const avatarUrl = ref('')
      const downloadImage = async () => {
        try {
          const { data, error } = await supabase.storage.from('avatars').download(path.value)
          if (error) throw error
          avatarUrl.value = URL.createObjectURL(data!)
        } catch (error: any) {
          console.error('Error downloading image: ', error.message)
        }
      }
      const uploadAvatar = async () => {
        try {
          const photo = await Camera.getPhoto({
            resultType: CameraResultType.DataUrl,
          })
          if (photo.dataUrl) {
            const file = await fetch(photo.dataUrl)
              .then((res) => res.blob())
              .then((blob) => new File([blob], 'my-file', { type: `image/${photo.format}` }))
            const fileName = `${Math.random()}-${new Date().getTime()}.${photo.format}`
            const { error: uploadError } = await supabase.storage
              .from('avatars')
              .upload(fileName, file)
            if (uploadError) {
              throw uploadError
            }
            emit('update:path', fileName)
            emit('upload')
          }
        } catch (error) {
          console.log(error)
        }
      }
      watch(path, () => {
        if (path.value) downloadImage()
      })
      return { avatarUrl, uploadAvatar, person }
    },
})
</script>
<style>
.avatar {
    display: block;
    margin: auto;
    min-height: 150px;
}
.avatar .avatar_wrapper {
    margin: 16px auto 16px;
    border-radius: 50%;
    overflow: hidden;
    height: 150px;
    aspect-ratio: 1;
    background: var(--ion-color-step-50);
    border: thick solid var(--ion-color-step-200);
}
.avatar .avatar_wrapper:hover {
    cursor: pointer;
}
.avatar .avatar_wrapper ion-icon.no-avatar {
    width: 100%;
    height: 115%;
}
.avatar img {
    display: block;
    object-fit: cover;
    width: 100%;
    height: 100%;
}
</style>
``

### Add the new widget [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue\#add-the-new-widget)

And then we can add the widget to the Account page:

src/views/Account.vue

`
<template>
<ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Account</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <avatar v-model:path="profile.avatar_url" @upload="updateProfile"></avatar>
...
</template>
<script lang="ts">
import Avatar from '../components/Avatar.vue';
export default defineComponent({
name: 'AccountPage',
components: {
    Avatar,
    ....
}
</script>
`

At this stage you have a fully functional application!

### Is this helpful?

NoYes

### On this page

[Project setup](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#project-setup) [Create a project](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#create-a-project) [Set up the database schema](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#set-up-the-database-schema) [Get the API Keys](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#get-the-api-keys) [Building the app](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#building-the-app) [Initialize an Ionic Vue app](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#initialize-an-ionic-vue-app) [Set up a login route](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#set-up-a-login-route) [Account page](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#account-page) [Launch!](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#launch) [Bonus: Profile photos](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#bonus-profile-photos) [Create an upload widget](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#create-an-upload-widget) [Add the new widget](https://supabase.com/docs/guides/getting-started/tutorials/with-ionic-vue#add-the-new-widget)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings