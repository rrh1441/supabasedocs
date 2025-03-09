Auth

# Passwordless email logins

## Email logins using Magic Links or One-Time Passwords (OTPs)

* * *

Supabase Auth provides several passwordless login methods. Passwordless logins allow users to sign in without a password, by clicking a confirmation link or entering a verification code.

Passwordless login can:

- Improve the user experience by not requiring users to create and remember a password
- Increase security by reducing the risk of password-related security breaches
- Reduce support burden of dealing with password resets and other password-related flows

Supabase Auth offers two passwordless login methods that use the user's email address:

- [Magic Link](https://supabase.com/docs/guides/auth/auth-email-passwordless#with-magic-link)
- [OTP](https://supabase.com/docs/guides/auth/auth-email-passwordless#with-otp)

## With Magic Link [\#](https://supabase.com/docs/guides/auth/auth-email-passwordless\#with-magic-link)

Magic Links are a form of passwordless login where users click on a link sent to their email address to log in to their accounts. Magic Links only work with email addresses and are one-time use only.

### Enabling Magic Link [\#](https://supabase.com/docs/guides/auth/auth-email-passwordless\#enabling-magic-link)

Email authentication methods, including Magic Links, are enabled by default.

Configure the Site URL and any additional redirect URLs. These are the only URLs that are allowed as redirect destinations after the user clicks a Magic Link. You can change the URLs on the [Auth Providers page](https://supabase.com/dashboard/project/_/auth/providers) for hosted projects, or in the [configuration file](https://supabase.com/docs/guides/cli/config#auth.additional_redirect_urls) for self-hosted projects.

By default, a user can only request a magic link once every 60 seconds and they expire after 1 hour.

### Signing in with Magic Link [\#](https://supabase.com/docs/guides/auth/auth-email-passwordless\#signing-in-with-magic-link)

Call the "sign in with OTP" method from the client library.

Though the method is labelled "OTP", it sends a Magic Link by default. The two methods differ only in the content of the confirmation email sent to the user.

If the user hasn't signed up yet, they are automatically signed up by default. To prevent this, set the `shouldCreateUser` option to `false`.

JavaScriptExpo React NativeDartSwiftKotlinPython

`
async function signInWithEmail() {
const { data, error } = await supabase.auth.signInWithOtp({
    email: 'valid.email@supabase.io',
    options: {
      // set this to false if you do not want the user to be automatically signed up
      shouldCreateUser: false,
      emailRedirectTo: 'https://example.com/welcome',
    },
})
}
`

That's it for the implicit flow.

If you're using PKCE flow, edit the Magic Link [email template](https://supabase.com/docs/guides/auth/auth-email-templates) to send a token hash:

`
<h2>Magic Link</h2>
<p>Follow this link to login:</p>
<p><a href="{{ .SiteURL }}/auth/confirm?token_hash={{ .TokenHash }}&type=email">Log In</a></p>
`

At the `/auth/confirm` endpoint, exchange the hash for the session:

`
const { error } = await supabase.auth.verifyOtp({ token_hash, type })
`

## With OTP [\#](https://supabase.com/docs/guides/auth/auth-email-passwordless\#with-otp)

Email one-time passwords (OTP) are a form of passwordless login where users key in a six digit code sent to their email address to log in to their accounts.

### Enabling email OTP [\#](https://supabase.com/docs/guides/auth/auth-email-passwordless\#enabling-email-otp)

Email authentication methods, including Email OTPs, are enabled by default.

Email OTPs share an implementation with Magic Links. To send an OTP instead of a Magic Link, alter the **Magic Link** email template. For a hosted Supabase project, go to [Email Templates](https://supabase.com/dashboard/project/_/auth/templates) in the Dashboard. For a self-hosted project or local development, see the [Email Templates guide](https://supabase.com/docs/guides/auth/auth-email-templates).

Modify the template to include the `{{ .Token }}` variable, for example:

`
<h2>One time login code</h2>
<p>Please enter this code: {{ .Token }}</p>
`

By default, a user can only request an OTP once every 60 seconds and they expire after 1 hour. This is configurable via `Auth > Providers > Email > Email OTP Expiration`. An expiry duration of more than 86400 seconds (one day) is disallowed to guard against brute force attacks. The longer an OTP remains valid, the more time an attacker has to attempt brute force attacks. If the OTP is valid for several days, an attacker might have more opportunities to guess the correct OTP through repeated attempts.

### Signing in with email OTP [\#](https://supabase.com/docs/guides/auth/auth-email-passwordless\#signing-in-with-email-otp)

#### Step 1: Send the user an OTP code [\#](https://supabase.com/docs/guides/auth/auth-email-passwordless\#step-1-send-the-user-an-otp-code)

Get the user's email and call the "sign in with OTP" method from your client library.

If the user hasn't signed up yet, they are automatically signed up by default. To prevent this, set the `shouldCreateUser` option to `false`.

JavaScriptDartSwiftKotlinPython

`
const { data, error } = await supabase.auth.signInWithOtp({
email: 'valid.email@supabase.io',
options: {
    // set this to false if you do not want the user to be automatically signed up
    shouldCreateUser: false,
},
})
`

If the request is successful, you receive a response with `error: null` and a `data` object where both `user` and `session` are null. Let the user know to check their email inbox.

`
{
"data": {
    "user": null,
    "session": null
},
"error": null
}
`

#### Step 2: Verify the OTP to create a session [\#](https://supabase.com/docs/guides/auth/auth-email-passwordless\#step-2-verify-the-otp-to-create-a-session)

Provide an input field for the user to enter their one-time code.

Call the "verify OTP" method from your client library with the user's email address, the code, and a type of `email`:

JavaScriptSwiftKotlinPython

`
const {
data: { session },
error,
} = await supabase.auth.verifyOtp({
email,
token: '123456',
type: 'email',
})
`

If successful, the user is now logged in, and you receive a valid session that looks like:

`
{
"access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJhdXRoZW50aWNhdGVkIiwiZXhwIjoxNjI3MjkxNTc3LCJzdWIiOiJmYTA2NTQ1Zi1kYmI1LTQxY2EtYjk1NC1kOGUyOTg4YzcxOTEiLCJlbWFpbCI6IiIsInBob25lIjoiNjU4NzUyMjAyOSIsImFwcF9tZXRhZGF0YSI6eyJwcm92aWRlciI6InBob25lIn0sInVzZXJfbWV0YWRhdGEiOnt9LCJyb2xlIjoiYXV0aGVudGljYXRlZCJ9.1BqRi0NbS_yr1f6hnr4q3s1ylMR3c1vkiJ4e_N55dhM",
"token_type": "bearer",
"expires_in": 3600,
"refresh_token": "LSp8LglPPvf0DxGMSj-vaQ",
"user": {...}
}
`

### Is this helpful?

NoYes

### On this page

[With Magic Link](https://supabase.com/docs/guides/auth/auth-email-passwordless#with-magic-link) [Enabling Magic Link](https://supabase.com/docs/guides/auth/auth-email-passwordless#enabling-magic-link) [Signing in with Magic Link](https://supabase.com/docs/guides/auth/auth-email-passwordless#signing-in-with-magic-link) [With OTP](https://supabase.com/docs/guides/auth/auth-email-passwordless#with-otp) [Enabling email OTP](https://supabase.com/docs/guides/auth/auth-email-passwordless#enabling-email-otp) [Signing in with email OTP](https://supabase.com/docs/guides/auth/auth-email-passwordless#signing-in-with-email-otp)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings