Auth

# Password-based Auth

## Allow users to sign in with a password connected to their email or phone number.

* * *

Users often expect to sign in to your site with a password. Supabase Auth helps you implement password-based auth safely, using secure configuration options and best practices for storing and verifying passwords.

Users can associate a password with their identity using their [email address](https://supabase.com/docs/guides/auth/passwords#with-email) or a [phone number](https://supabase.com/docs/guides/auth/passwords#with-phone).

## With email [\#](https://supabase.com/docs/guides/auth/passwords\#with-email)

### Enabling email and password-based authentication [\#](https://supabase.com/docs/guides/auth/passwords\#enabling-email-and-password-based-authentication)

Email authentication is enabled by default.

You can configure whether users need to verify their email to sign in. On hosted Supabase projects, this is true by default. On self-hosted projects or in local development, this is false by default.

Change this setting on the [Auth Providers page](https://supabase.com/dashboard/project/_/auth/providers) for hosted projects, or in the [configuration file](https://supabase.com/docs/guides/cli/config#auth.email.enable_confirmations) for self-hosted projects.

### Signing up with an email and password [\#](https://supabase.com/docs/guides/auth/passwords\#signing-up-with-an-email-and-password)

There are two possible flows for email signup: [implicit flow](https://supabase.com/docs/guides/auth/sessions#implicit-flow) and [PKCE flow](https://supabase.com/docs/guides/auth/sessions#pkce-flow). If you're using SSR, you're using the PKCE flow. If you're using client-only code, the default flow depends upon the client library. The implicit flow is the default in JavaScript and Dart, and the PKCE flow is the default in Swift.

The instructions in this section assume that email confirmations are enabled.

Implicit flowPKCE flow

The implicit flow only works for client-only apps. Your site directly receives the access token after the user confirms their email.

JavaScriptDartSwiftKotlinPython

To sign up the user, call [signUp()](https://supabase.com/docs/reference/javascript/auth-signup) with their email address and password.

You can optionally specify a URL to redirect to after the user clicks the confirmation link. This URL must be configured as a [Redirect URL](https://supabase.com/docs/guides/auth/redirect-urls), which you can do in the [dashboard](https://supabase.com/dashboard/project/_/auth/url-configuration) for hosted projects, or in the [configuration file](https://supabase.com/docs/guides/cli/config#auth.additional_redirect_urls) for self-hosted projects.

If you don't specify a redirect URL, the user is automatically redirected to your site URL. This defaults to `localhost:3000`, but you can also configure this.

`
async function signUpNewUser() {
const { data, error } = await supabase.auth.signUp({
    email: 'valid.email@supabase.io',
    password: 'example-password',
    options: {
      emailRedirectTo: 'https://example.com/welcome',
    },
})
}
`

### Signing in with an email and password [\#](https://supabase.com/docs/guides/auth/passwords\#signing-in-with-an-email-and-password)

JavaScriptDartSwiftKotlinPython

When your user signs in, call [`signInWithPassword()`](https://supabase.com/docs/reference/javascript/auth-signinwithpassword) with their email address and password:

`
async function signInWithEmail() {
const { data, error } = await supabase.auth.signInWithPassword({
    email: 'valid.email@supabase.io',
    password: 'example-password',
})
}
`

### Resetting a password [\#](https://supabase.com/docs/guides/auth/passwords\#resetting-a-password)

Implicit flowPKCE flow

#### Step 1: Create a reset password page [\#](https://supabase.com/docs/guides/auth/passwords\#step-1-create-a-reset-password-page)

Create a **reset password** page. This page should be publicly accessible.

Collect the user's email address and request a password reset email. Specify the redirect URL, which should point to the URL of a **change password** page. This URL needs to be configured in your [redirect URLs](https://supabase.com/docs/guides/auth/redirect-urls).

JavaScriptSwiftKotlinPython

`
await supabase.auth.resetPasswordForEmail('valid.email@supabase.io', {
redirectTo: 'http://example.com/account/update-password',
})
`

#### Step 2: Create a change password page [\#](https://supabase.com/docs/guides/auth/passwords\#step-2-create-a-change-password-page)

Create a **change password** page at the URL you specified in the previous step. This page should be accessible only to authenticated users.

Collect the user's new password and call `updateUser` to update their password.

JavaScriptSwiftKotlinPython

`
await supabase.auth.updateUser({ password: new_password })
`

### Email sending [\#](https://supabase.com/docs/guides/auth/passwords\#email-sending)

The signup confirmation and password reset flows require an SMTP server to send emails.

The Supabase platform comes with a default email-sending service for you to try out. The service has a rate limit of 2 emails per hour, and availability is on a best-effort basis. For production use, you should consider configuring a custom SMTP server.

Consider configuring a custom SMTP server for production.

See the [Custom SMTP guide](https://supabase.com/docs/guides/auth/auth-smtp) for instructions.

#### Local development with Inbucket [\#](https://supabase.com/docs/guides/auth/passwords\#local-development-with-inbucket)

You can test email flows on your local machine. The Supabase CLI automatically captures emails sent locally by using [Inbucket](https://github.com/inbucket/inbucket).

In your terminal, run `supabase status` to get the Inbucket URL. Go to this URL in your browser, and follow the instructions to find your emails.

## With phone [\#](https://supabase.com/docs/guides/auth/passwords\#with-phone)

You can use a user's mobile phone number as an identifier, instead of an email address, when they sign up with a password.

This practice is usually discouraged because phone networks recycle mobile phone numbers. Anyone receiving a recycled phone number gets access to the original user's account. To mitigate this risk, [implement MFA](https://supabase.com/docs/guides/auth/auth-mfa).

Protect users who use a phone number as a password-based auth identifier by enabling MFA.

### Enabling phone and password-based authentication [\#](https://supabase.com/docs/guides/auth/passwords\#enabling-phone-and-password-based-authentication)

Enable phone authentication on the [Auth Providers page](https://supabase.com/dashboard/project/_/auth/providers) for hosted Supabase projects.

For self-hosted projects or local development, use the [configuration file](https://supabase.com/docs/guides/cli/config#auth.sms.enable_signup). See the configuration variables namespaced under `auth.sms`.

If you want users to confirm their phone number on signup, you need to set up an SMS provider. Each provider has its own configuration. Supported providers include MessageBird, Twilio, Vonage, and TextLocal (community-supported).

### Configuring SMS Providers

![MessageBird Icon](https://supabase.com/docs/img/icons/messagebird-icon.svg)

##### MessageBird

![Twilio Icon](https://supabase.com/docs/img/icons/twilio-icon.svg)

##### Twilio

![Vonage Icon](https://supabase.com/docs/img/icons/vonage-icon-light.svg)

##### Vonage

![Textlocal (Community Supported) Icon](https://supabase.com/docs/img/icons/textlocal-icon.svg)

##### Textlocal (Community Supported)

### Signing up with a phone number and password [\#](https://supabase.com/docs/guides/auth/passwords\#signing-up-with-a-phone-number-and-password)

To sign up the user, call [`signUp()`](https://supabase.com/docs/reference/javascript/auth-signup) with their phone number and password:

JavaScriptSwiftKotlinPythonHTTP

`
const { data, error } = await supabase.auth.signUp({
phone: '+13334445555',
password: 'some-password',
})
`

If you have phone verification turned on, the user receives an SMS with a 6-digit pin that you must verify within 60 seconds:

JavaScriptSwiftKotlinPythonHTTP

You should present a form to the user so they can input the 6 digit pin, then send it along with the phone number to `verifyOtp`:

`
const {
data: { session },
error,
} = await supabase.auth.verifyOtp({
phone: '+13334445555',
token: '123456',
type: 'sms',
})
`

### Signing in a with a phone number and password [\#](https://supabase.com/docs/guides/auth/passwords\#signing-in-a-with-a-phone-number-and-password)

Call the function to sign in with the user's phone number and password:

JavaScriptSwiftKotlinPythonHTTP

`
const { user, error } = await supabase.auth.signInWithPassword({
phone: '+13334445555',
password: 'some-password',
})
`

### Is this helpful?

NoYes

### On this page

[With email](https://supabase.com/docs/guides/auth/passwords#with-email) [Enabling email and password-based authentication](https://supabase.com/docs/guides/auth/passwords#enabling-email-and-password-based-authentication) [Signing up with an email and password](https://supabase.com/docs/guides/auth/passwords#signing-up-with-an-email-and-password) [Signing in with an email and password](https://supabase.com/docs/guides/auth/passwords#signing-in-with-an-email-and-password) [Resetting a password](https://supabase.com/docs/guides/auth/passwords#resetting-a-password) [Email sending](https://supabase.com/docs/guides/auth/passwords#email-sending) [With phone](https://supabase.com/docs/guides/auth/passwords#with-phone) [Enabling phone and password-based authentication](https://supabase.com/docs/guides/auth/passwords#enabling-phone-and-password-based-authentication) [Signing up with a phone number and password](https://supabase.com/docs/guides/auth/passwords#signing-up-with-a-phone-number-and-password) [Signing in a with a phone number and password](https://supabase.com/docs/guides/auth/passwords#signing-in-a-with-a-phone-number-and-password)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings