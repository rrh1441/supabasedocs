Auth

# Identity Linking

## Manage the identities associated with your user

* * *

## Identity linking strategies [\#](https://supabase.com/docs/guides/auth/auth-identity-linking\#identity-linking-strategies)

Currently, Supabase Auth supports 2 strategies to link an identity to a user:

1. [Automatic Linking](https://supabase.com/docs/guides/auth/auth-identity-linking#automatic-linking)
2. [Manual Linking](https://supabase.com/docs/guides/auth/auth-identity-linking#manual-linking-beta)

### Automatic linking [\#](https://supabase.com/docs/guides/auth/auth-identity-linking\#automatic-linking)

Supabase Auth automatically links identities with the same email address to a single user. This helps to improve the user experience when multiple OAuth login options are presented since the user does not need to remember which OAuth account they used to sign up with. When a new user signs in with OAuth, Supabase Auth will attempt to look for an existing user that uses the same email address. If a match is found, the new identity is linked to the user.

In order for automatic linking to correctly identify the user for linking, Supabase Auth needs to ensure that all user emails are unique. It would also be an insecure practice to automatically link an identity to a user with an unverified email address since that could lead to pre-account takeover attacks. To prevent this from happening, when a new identity can be linked to an existing user, Supabase Auth will remove any other unconfirmed identities linked to an existing user.

Users that signed up with [SAML SSO](https://supabase.com/docs/guides/auth/sso/auth-sso-saml) will not be considered as targets for automatic linking.

### Manual linking (beta) [\#](https://supabase.com/docs/guides/auth/auth-identity-linking\#manual-linking-beta)

JavaScriptDartSwiftKotlinPython

Supabase Auth allows a user to initiate identity linking with a different email address when they are logged in. To link an OAuth identity to the user, call [`linkIdentity()`](https://supabase.com/docs/reference/javascript/auth-linkidentity):

`
const { data, error } = await supabase.auth.linkIdentity({ provider: 'google' })
`

In the example above, the user will be redirected to Google to complete the OAuth2.0 flow. Once the OAuth2.0 flow has completed successfully, the user will be redirected back to the application and the Google identity will be linked to the user. You can enable manual linking from your project's authentication [configuration options](https://supabase.com/dashboard/project/_/settings/auth) or by setting the environment variable `GOTRUE_SECURITY_MANUAL_LINKING_ENABLED: true` when self-hosting.

## Unlink an identity [\#](https://supabase.com/docs/guides/auth/auth-identity-linking\#unlink-an-identity)

JavaScriptDartSwiftKotlinPython

You can use [`getUserIdentities()`](https://supabase.com/docs/reference/javascript/auth-getuseridentities) to fetch all the identities linked to a user. Then, call [`unlinkIdentity()`](https://supabase.com/docs/reference/javascript/auth-unlinkidentity) to unlink the identity. The user needs to be logged in and have at least 2 linked identities in order to unlink an existing identity.

`
// retrieve all identities linked to a user
const {
data: { identities },
} = await supabase.auth.getUserIdentities()
// find the google identity linked to the user
const googleIdentity = identities.find((identity) => identity.provider === 'google')
// unlink the google identity from the user
const { data, error } = await supabase.auth.unlinkIdentity(googleIdentity)
`

## Frequently asked questions [\#](https://supabase.com/docs/guides/auth/auth-identity-linking\#frequently-asked-questions)

### How to add email/password login to an OAuth account? [\#](https://supabase.com/docs/guides/auth/auth-identity-linking\#how-to-add-emailpassword-login-to-an-oauth-account)

Call the `updateUser({ password: 'validpassword'})` to add email with password authentication to an account created with an OAuth provider (Google, GitHub, etc.).

### Can you sign up with email if already using OAuth? [\#](https://supabase.com/docs/guides/auth/auth-identity-linking\#can-you-sign-up-with-email-if-already-using-oauth)

If you try to create an email account after previously signing up with OAuth using the same email, you'll receive an obfuscated user response with no verification email sent. This prevents user enumeration attacks.

### Is this helpful?

NoYes

### On this page

[Identity linking strategies](https://supabase.com/docs/guides/auth/auth-identity-linking#identity-linking-strategies) [Automatic linking](https://supabase.com/docs/guides/auth/auth-identity-linking#automatic-linking) [Manual linking (beta)](https://supabase.com/docs/guides/auth/auth-identity-linking#manual-linking-beta) [Unlink an identity](https://supabase.com/docs/guides/auth/auth-identity-linking#unlink-an-identity) [Frequently asked questions](https://supabase.com/docs/guides/auth/auth-identity-linking#frequently-asked-questions) [How to add email/password login to an OAuth account?](https://supabase.com/docs/guides/auth/auth-identity-linking#how-to-add-emailpassword-login-to-an-oauth-account) [Can you sign up with email if already using OAuth?](https://supabase.com/docs/guides/auth/auth-identity-linking#can-you-sign-up-with-email-if-already-using-oauth)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings