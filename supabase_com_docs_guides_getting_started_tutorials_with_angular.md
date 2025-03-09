Getting Started

# Build a User Management App with Angular

* * *

This tutorial demonstrates how to build a basic user management app. The app authenticates and identifies the user, stores their profile information in the database, and allows the user to log in, update their profile details, and upload a profile photo. The app uses:

- [Supabase Database](https://supabase.com/docs/guides/database) \- a Postgres database for storing your user data and [Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) so data is protected and users can only access their own information.
- [Supabase Auth](https://supabase.com/docs/guides/auth) \- allow users to sign up and log in.
- [Supabase Storage](https://supabase.com/docs/guides/storage) \- users can upload a profile photo.

![Supabase User Management example](https://supabase.com/docs/img/user-management-demo.png)

If you get stuck while working through this guide, refer to the [full example on GitHub](https://github.com/supabase/supabase/tree/master/examples/user-management/angular-user-management).

## Project setup [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-angular\#project-setup)

Before we start building we're going to set up our Database and API. This is as simple as starting a new Project in Supabase and then creating a "schema" inside the database.

### Create a project [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-angular\#create-a-project)

1. [Create a new project](https://supabase.com/dashboard) in the Supabase Dashboard.
2. Enter your project details.
3. Wait for the new database to launch.

### Set up the database schema [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-angular\#set-up-the-database-schema)

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

### Get the API Keys [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-angular\#get-the-api-keys)

Now that you've created some database tables, you are ready to insert data using the auto-generated API.
We just need to get the Project URL and `anon` key from the API settings.

1. Go to the [API Settings](https://supabase.com/dashboard/project/_/settings/api) page in the Dashboard.
2. Find your Project `URL`, `anon`, and `service_role` keys on this page.

## Building the app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-angular\#building-the-app)

Let's start building the Angular app from scratch.

### Initialize an Angular app [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-angular\#initialize-an-angular-app)

We can use the [Angular CLI](https://angular.io/cli) to initialize
an app called `supabase-angular`:

`
npx ng new supabase-angular --routing false --style css --standalone false
cd supabase-angular
`

Then let's install the only additional dependency: [supabase-js](https://github.com/supabase/supabase-js)

`
npm install @supabase/supabase-js
`

And finally we want to save the environment variables in the `src/environments/environment.ts` file.
All we need are the API URL and the `anon` key that you copied [earlier](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#get-the-api-keys).
These variables will be exposed on the browser, and that's completely fine since we have [Row Level Security](https://supabase.com/docs/guides/auth#row-level-security) enabled on our Database.

src/environments/environment.ts

`
export const environment = {
production: false,
supabaseUrl: 'YOUR_SUPABASE_URL',
supabaseKey: 'YOUR_SUPABASE_KEY',
}
`

Now that we have the API credentials in place, let's create a `SupabaseService` with `ng g s supabase` to initialize the Supabase client and implement functions to communicate with the Supabase API.

src/app/supabase.service.ts

``
import { Injectable } from '@angular/core'
import {
AuthChangeEvent,
AuthSession,
createClient,
Session,
SupabaseClient,
User,
} from '@supabase/supabase-js'
import { environment } from '../environments/environment'
export interface Profile {
id?: string
username: string
website: string
avatar_url: string
}
@Injectable({
providedIn: 'root',
})
export class SupabaseService {
private supabase: SupabaseClient
_session: AuthSession | null = null
constructor() {
    this.supabase = createClient(environment.supabaseUrl, environment.supabaseKey)
}
get session() {
    this.supabase.auth.getSession().then(({ data }) => {
      this._session = data.session
    })
    return this._session
}
profile(user: User) {
    return this.supabase
      .from('profiles')
      .select(`username, website, avatar_url`)
      .eq('id', user.id)
      .single()
}
authChanges(callback: (event: AuthChangeEvent, session: Session | null) => void) {
    return this.supabase.auth.onAuthStateChange(callback)
}
signIn(email: string) {
    return this.supabase.auth.signInWithOtp({ email })
}
signOut() {
    return this.supabase.auth.signOut()
}
updateProfile(profile: Profile) {
    const update = {
      ...profile,
      updated_at: new Date(),
    }
    return this.supabase.from('profiles').upsert(update)
}
downLoadImage(path: string) {
    return this.supabase.storage.from('avatars').download(path)
}
uploadAvatar(filePath: string, file: File) {
    return this.supabase.storage.from('avatars').upload(filePath, file)
}
}
``

Optionally, update [src/styles.css](https://raw.githubusercontent.com/supabase/supabase/master/examples/user-management/angular-user-management/src/styles.css) to style the app.

### Set up a login component [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-angular\#set-up-a-login-component)

Let's set up an Angular component to manage logins and sign ups. We'll use Magic Links, so users can sign in with their email without using passwords.
Create an `AuthComponent` with `ng g c auth` Angular CLI command.

src/app/auth/auth.component.ts

`
import { Component } from '@angular/core'
import { FormBuilder } from '@angular/forms'
import { SupabaseService } from '../supabase.service'
@Component({
selector: 'app-auth',
templateUrl: './auth.component.html',
styleUrls: ['./auth.component.css'],
})
export class AuthComponent {
loading = false
signInForm = this.formBuilder.group({
    email: '',
})
constructor(
    private readonly supabase: SupabaseService,
    private readonly formBuilder: FormBuilder
) {}
async onSubmit(): Promise<void> {
    try {
      this.loading = true
      const email = this.signInForm.value.email as string
      const { error } = await this.supabase.signIn(email)
      if (error) throw error
      alert('Check your email for the login link!')
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      this.signInForm.reset()
      this.loading = false
    }
}
}
`

src/app/auth/auth.component.html

`
<div class="row flex-center flex">
<div class="col-6 form-widget" aria-live="polite">
    <h1 class="header">Supabase + Angular</h1>
    <p class="description">Sign in via magic link with your email below</p>
    <form [formGroup]="signInForm" (ngSubmit)="onSubmit()" class="form-widget">
      <div>
        <label for="email">Email</label>
        <input
          id="email"
          formControlName="email"
          class="inputField"
          type="email"
          placeholder="Your email"
        />
      </div>
      <div>
        <button type="submit" class="button block" [disabled]="loading">
          {{ loading ? 'Loading' : 'Send magic link' }}
        </button>
      </div>
    </form>
</div>
</div>
`

### Account page [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-angular\#account-page)

Users also need a way to edit their profile details and manage their accounts after signing in.
Create an `AccountComponent` with the `ng g c account` Angular CLI command.

src/app/account/account.component.ts

`
import { Component, Input, OnInit } from '@angular/core'
import { FormBuilder } from '@angular/forms'
import { AuthSession } from '@supabase/supabase-js'
import { Profile, SupabaseService } from '../supabase.service'
@Component({
selector: 'app-account',
templateUrl: './account.component.html',
styleUrls: ['./account.component.css'],
})
export class AccountComponent implements OnInit {
loading = false
profile!: Profile
@Input()
session!: AuthSession
updateProfileForm = this.formBuilder.group({
    username: '',
    website: '',
    avatar_url: '',
})
constructor(
    private readonly supabase: SupabaseService,
    private formBuilder: FormBuilder
) {}
async ngOnInit(): Promise<void> {
    await this.getProfile()
    const { username, website, avatar_url } = this.profile
    this.updateProfileForm.patchValue({
      username,
      website,
      avatar_url,
    })
}
async getProfile() {
    try {
      this.loading = true
      const { user } = this.session
      const { data: profile, error, status } = await this.supabase.profile(user)
      if (error && status !== 406) {
        throw error
      }
      if (profile) {
        this.profile = profile
      }
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      this.loading = false
    }
}
async updateProfile(): Promise<void> {
    try {
      this.loading = true
      const { user } = this.session
      const username = this.updateProfileForm.value.username as string
      const website = this.updateProfileForm.value.website as string
      const avatar_url = this.updateProfileForm.value.avatar_url as string
      const { error } = await this.supabase.updateProfile({
        id: user.id,
        username,
        website,
        avatar_url,
      })
      if (error) throw error
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      this.loading = false
    }
}
async signOut() {
    await this.supabase.signOut()
}
}
`

src/app/account/account.component.html

`
<form [formGroup]="updateProfileForm" (ngSubmit)="updateProfile()" class="form-widget">
<div>
    <label for="email">Email</label>
    <input id="email" type="text" [value]="session.user.email" disabled />
</div>
<div>
    <label for="username">Name</label>
    <input formControlName="username" id="username" type="text" />
</div>
<div>
    <label for="website">Website</label>
    <input formControlName="website" id="website" type="url" />
</div>
<div>
    <button type="submit" class="button primary block" [disabled]="loading">
      {{ loading ? 'Loading ...' : 'Update' }}
    </button>
</div>
<div>
    <button class="button block" (click)="signOut()">Sign Out</button>
</div>
</form>
`

### Launch! [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-angular\#launch)

Now that we have all the components in place, let's update `AppComponent`:

src/app/app.component.ts

`
import { Component, OnInit } from '@angular/core'
import { SupabaseService } from './supabase.service'
@Component({
selector: 'app-root',
templateUrl: './app.component.html',
styleUrls: ['./app.component.css'],
})
export class AppComponent implements OnInit {
title = 'angular-user-management'
session = this.supabase.session
constructor(private readonly supabase: SupabaseService) {}
ngOnInit() {
    this.supabase.authChanges((_, session) => (this.session = session))
}
}
`

src/app/app.component.html

`
<div class="container" style="padding: 50px 0 100px 0">
<app-account *ngIf="session; else auth" [session]="session"></app-account>
<ng-template #auth>
    <app-auth></app-auth>
</ng-template>
</div>
`

`app.module.ts` also needs to be modified to include the `ReactiveFormsModule` from the `@angular/forms` package.

src/app/app.module.ts

`
import { NgModule } from '@angular/core'
import { BrowserModule } from '@angular/platform-browser'
import { AppComponent } from './app.component'
import { AuthComponent } from './auth/auth.component'
import { AccountComponent } from './account/account.component'
import { ReactiveFormsModule } from '@angular/forms'
import { AvatarComponent } from './avatar/avatar.component'
@NgModule({
declarations: [AppComponent, AuthComponent, AccountComponent, AvatarComponent],
imports: [BrowserModule, ReactiveFormsModule],
providers: [],
bootstrap: [AppComponent],
})
export class AppModule {}
`

Once that's done, run this in a terminal window:

`
npm run start
`

And then open the browser to [localhost:4200](http://localhost:4200/) and you should see the completed app.

![Supabase Angular](https://supabase.com/docs/img/supabase-angular-demo.png)

## Bonus: Profile photos [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-angular\#bonus-profile-photos)

Every Supabase project is configured with [Storage](https://supabase.com/docs/guides/storage) for managing large files like photos and videos.

### Create an upload widget [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-angular\#create-an-upload-widget)

Let's create an avatar for the user so that they can upload a profile photo.
Create an `AvatarComponent` with `ng g c avatar` Angular CLI command.

src/app/avatar/avatar.component.ts

``
import { Component, EventEmitter, Input, Output } from '@angular/core'
import { SafeResourceUrl, DomSanitizer } from '@angular/platform-browser'
import { SupabaseService } from '../supabase.service'
@Component({
selector: 'app-avatar',
templateUrl: './avatar.component.html',
styleUrls: ['./avatar.component.css'],
})
export class AvatarComponent {
_avatarUrl: SafeResourceUrl | undefined
uploading = false
@Input()
set avatarUrl(url: string | null) {
    if (url) {
      this.downloadImage(url)
    }
}
@Output() upload = new EventEmitter<string>()
constructor(
    private readonly supabase: SupabaseService,
    private readonly dom: DomSanitizer
) {}
async downloadImage(path: string) {
    try {
      const { data } = await this.supabase.downLoadImage(path)
      if (data instanceof Blob) {
        this._avatarUrl = this.dom.bypassSecurityTrustResourceUrl(URL.createObjectURL(data))
      }
    } catch (error) {
      if (error instanceof Error) {
        console.error('Error downloading image: ', error.message)
      }
    }
}
async uploadAvatar(event: any) {
    try {
      this.uploading = true
      if (!event.target.files || event.target.files.length === 0) {
        throw new Error('You must select an image to upload.')
      }
      const file = event.target.files[0]
      const fileExt = file.name.split('.').pop()
      const filePath = `${Math.random()}.${fileExt}`
      await this.supabase.uploadAvatar(filePath, file)
      this.upload.emit(filePath)
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      this.uploading = false
    }
}
}
``

src/app/avatar/avatar.component.html

`
<div>
<img
    *ngIf="_avatarUrl"
    [src]="_avatarUrl"
    alt="Avatar"
    class="avatar image"
    style="height: 150px; width: 150px"
/>
</div>
<div *ngIf="!_avatarUrl" class="avatar no-image" style="height: 150px; width: 150px"></div>
<div style="width: 150px">
<label class="button primary block" for="single">
    {{ uploading ? 'Uploading ...' : 'Upload' }}
</label>
<input
    style="visibility: hidden;position: absolute"
    type="file"
    id="single"
    accept="image/*"
    (change)="uploadAvatar($event)"
    [disabled]="uploading"
/>
</div>
`

### Add the new widget [\#](https://supabase.com/docs/guides/getting-started/tutorials/with-angular\#add-the-new-widget)

And then we can add the widget on top of the `AccountComponent` HTML template:

src/app/account.component.html

`
<form [formGroup]="updateProfileForm" (ngSubmit)="updateProfile()" class="form-widget">
<app-avatar [avatarUrl]="this.avatarUrl" (upload)="updateAvatar($event)"> </app-avatar>
<!-- input fields -->
</form>
`

And add an `updateAvatar` function along with an `avatarUrl` getter to the `AccountComponent` typescript file:

src/app/account.component.ts

`
@Component({
selector: 'app-account',
templateUrl: './account.component.html',
styleUrls: ['./account.component.css'],
})
export class AccountComponent implements OnInit {
// ...
get avatarUrl() {
    return this.updateProfileForm.value.avatar_url as string
}
async updateAvatar(event: string): Promise<void> {
    this.updateProfileForm.patchValue({
      avatar_url: event,
    })
    await this.updateProfile()
}
// ...
}
`

At this stage you have a fully functional application!

### Is this helpful?

NoYes

### On this page

[Project setup](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#project-setup) [Create a project](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#create-a-project) [Set up the database schema](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#set-up-the-database-schema) [Get the API Keys](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#get-the-api-keys) [Building the app](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#building-the-app) [Initialize an Angular app](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#initialize-an-angular-app) [Set up a login component](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#set-up-a-login-component) [Account page](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#account-page) [Launch!](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#launch) [Bonus: Profile photos](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#bonus-profile-photos) [Create an upload widget](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#create-an-upload-widget) [Add the new widget](https://supabase.com/docs/guides/getting-started/tutorials/with-angular#add-the-new-widget)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings