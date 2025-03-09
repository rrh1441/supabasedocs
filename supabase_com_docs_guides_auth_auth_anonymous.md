Auth

# Anonymous Sign-Ins

## Create and use anonymous users to authenticate with Supabase

* * *

[Enable Anonymous Sign-Ins](https://supabase.com/dashboard/project/_/settings/auth) to build apps which provide users an authenticated experience without requiring users to enter an email address, password, use an OAuth provider or provide any other PII (Personally Identifiable Information). Later, when ready, the user can link an authentication method to their account.

##### Anonymous user vs the anon key

Calling `signInAnonymously()` creates an anonymous user. It's just like a permanent user, except the user can't access their account if they sign out, clear browsing data, or use another device.

Like permanent users, the `authenticated` Postgres role will be used when using the Data APIs to access your project. JWTs for these users will have an `is_anonymous` claim which you can use to distinguish in RLS policies.

This is different from the `anon` API key which does not create a user and can be used to implement public access to your database as it uses the `anonymous` Postgres role.

Anonymous sign-ins can be used to build:

- E-commerce applications, such as shopping carts before check-out
- Full-feature demos without collecting personal information
- Temporary or throw-away accounts

Review your existing RLS policies before enabling anonymous sign-ins. Anonymous users use the `authenticated` role. To distinguish between anonymous users and permanent users, your policies need to check the `is_anonymous` field of the user's JWT.

See the [Access control section](https://supabase.com/docs/guides/auth/auth-anonymous#access-control) for more details.

##### Use Dynamic Rendering with Next.js

The Supabase team has received reports of user metadata being cached across unique anonymous users as a result of Next.js static page rendering. For the best user experience, utilize dynamic page rendering.

##### Self hosting and local development

For self-hosting, you can update your project configuration using the files and environment variables provided. See the [local development docs](https://supabase.com/docs/guides/cli/config) for more details.

## Sign in anonymously [\#](https://supabase.com/docs/guides/auth/auth-anonymous\#sign-in-anonymously)

JavaScriptFlutterSwiftKotlinPython

Call the [`signInAnonymously()`](https://supabase.com/docs/reference/javascript/auth-signinanonymously) method:

`
const { data, error } = await supabase.auth.signInAnonymously()
`

## Convert an anonymous user to a permanent user [\#](https://supabase.com/docs/guides/auth/auth-anonymous\#convert-an-anonymous-user-to-a-permanent-user)

Converting an anonymous user to a permanent user requires [linking an identity](https://supabase.com/docs/guides/auth/auth-identity-linking#manual-linking-beta) to the user. This requires you to [enable manual linking](https://supabase.com/dashboard/project/_/settings/auth) in your Supabase project.

### Link an email / phone identity [\#](https://supabase.com/docs/guides/auth/auth-anonymous\#link-an-email--phone-identity)

JavaScriptFlutterSwiftKotlinPython

You can use the [`updateUser()`](https://supabase.com/docs/reference/javascript/auth-updateuser) method to link an email or phone identity to the anonymous user. To add a password for the anonymous user, the user's email or phone number needs to be verified first.

`
const { data, error } = await supabase.auth.updateUser({ email: 'valid.email@supabase.io' })
// verify the user's email by clicking on the email change link
// or entering the 6-digit OTP sent to the email address
// once the user has been verified, update the password
const { data, error } = await supabase.auth.updateUser({ password: 'password' })
`

### Link an OAuth identity [\#](https://supabase.com/docs/guides/auth/auth-anonymous\#link-an-oauth-identity)

JavaScriptFlutterSwiftKotlinPython

You can use the [`linkIdentity()`](https://supabase.com/docs/reference/javascript/auth-linkidentity) method to link an OAuth identity to the anonymous user.

`
const { data, error } = await supabase.auth.linkIdentity({ provider: 'google' })
`

## Access control [\#](https://supabase.com/docs/guides/auth/auth-anonymous\#access-control)

An anonymous user assumes the `authenticated` role just like a permanent user. You can use row-level security (RLS) policies to differentiate between an anonymous user and a permanent user by checking for the `is_anonymous` claim in the JWT returned by `auth.jwt()`:

`
create policy "Only permanent users can post to the news feed"
on news_feed as restrictive for insert
to authenticated
with check ((select (auth.jwt()->>'is_anonymous')::boolean) is false );
create policy "Anonymous and permanent users can view the news feed"
on news_feed for select
to authenticated
using ( true );
`

##### Use restrictive policies

RLS policies are permissive by default, which means that they are combined using an "OR" operator when multiple policies are applied. It is important to construct restrictive policies to ensure that the checks for an anonymous user are always enforced when combined with other policies.

## Resolving identity conflicts [\#](https://supabase.com/docs/guides/auth/auth-anonymous\#resolving-identity-conflicts)

Depending on your application requirements, data conflicts can arise when an anonymous user is converted to a permanent user. For example, in the context of an e-commerce application, an anonymous user would be allowed to add items to the shopping cart without signing up / signing in. When they decide to sign-in to an existing account, you will need to decide how you want to resolve data conflicts in the shopping cart:

1. Overwrite the items in the cart with those in the existing account
2. Overwrite the items in the cart with those from the anonymous user
3. Merge the items in the cart together

### Linking an anonymous user to an existing account [\#](https://supabase.com/docs/guides/auth/auth-anonymous\#linking-an-anonymous-user-to-an-existing-account)

In some cases, you may need to link an anonymous user to an existing account rather than creating a new permanent account. This process requires manual handling of potential conflicts. Here's a general approach:

`
// 1. Sign in anonymously (assuming the user is already signed in anonymously)
const { data: anonData, error: anonError } = await supabase.auth.getSession()
// 2. Attempt to update the user with the existing email
const { data: updateData, error: updateError } = await supabase.auth.updateUser({
email: 'valid.email@supabase.io',
})
// 3. Handle the error (since the email belongs to an existing user)
if (updateError) {
console.log('This email belongs to an existing user. Please sign in to that account.')
// 4. Sign in to the existing account
const {
    data: { user: existingUser },
    error: signInError,
} = await supabase.auth.signInWithPassword({
    email: 'valid.email@supabase.io',
    password: 'user_password',
})
if (existingUser) {
    // 5. Reassign entities tied to the anonymous user
    // This step will vary based on your specific use case and data model
    const { data: reassignData, error: reassignError } = await supabase
      .from('your_table')
      .update({ user_id: existingUser.id })
      .eq('user_id', anonData.session.user.id)
    // 6. Implement your chosen conflict resolution strategy
    // This could involve merging data, overwriting, or other custom logic
    await resolveDataConflicts(anonData.session.user.id, existingUser.id)
}
}
// Helper function to resolve data conflicts (implement based on your strategy)
async function resolveDataConflicts(anonymousUserId, existingUserId) {
// Implement your conflict resolution logic here
// This could involve ignoring the anonymous user's metadata, overwriting the existing user's metadata, or merging the data of both the anonymous and existing user.
}
`

## Abuse prevention and rate limits [\#](https://supabase.com/docs/guides/auth/auth-anonymous\#abuse-prevention-and-rate-limits)

Since anonymous users are stored in your database, bad actors can abuse the endpoint to increase your database size drastically. It is strongly recommended to [enable invisible CAPTCHA or Cloudflare Turnstile](https://supabase.com/docs/guides/auth/auth-captcha) to prevent abuse for anonymous sign-ins. An IP-based rate limit is enforced at 30 requests per hour which can be modified in your [dashboard](https://supabase.com/dashboard/project/_/auth/rate-limits). You can refer to the full list of rate limits [here](https://supabase.com/docs/guides/platform/going-into-prod#rate-limiting-resource-allocation--abuse-prevention).

## Automatic cleanup [\#](https://supabase.com/docs/guides/auth/auth-anonymous\#automatic-cleanup)

Automatic cleanup of anonymous users is currently not available. Instead, you can delete anonymous users from your project by running the following SQL:

`
-- deletes anonymous users created more than 30 days ago
delete from auth.users
where is_anonymous is true and created_at < now() - interval '30 days';
`

## Resources [\#](https://supabase.com/docs/guides/auth/auth-anonymous\#resources)

- [Supabase - Get started for free](https://supabase.com/)
- [Supabase JS Client](https://github.com/supabase/supabase-js)
- [Supabase Flutter Client](https://github.com/supabase/supabase-flutter)
- [Supabase Kotlin Client](https://github.com/supabase-community/supabase-kt)

### Is this helpful?

NoYes

### On this page

[Sign in anonymously](https://supabase.com/docs/guides/auth/auth-anonymous#sign-in-anonymously) [Convert an anonymous user to a permanent user](https://supabase.com/docs/guides/auth/auth-anonymous#convert-an-anonymous-user-to-a-permanent-user) [Link an email / phone identity](https://supabase.com/docs/guides/auth/auth-anonymous#link-an-email--phone-identity) [Link an OAuth identity](https://supabase.com/docs/guides/auth/auth-anonymous#link-an-oauth-identity) [Access control](https://supabase.com/docs/guides/auth/auth-anonymous#access-control) [Resolving identity conflicts](https://supabase.com/docs/guides/auth/auth-anonymous#resolving-identity-conflicts) [Linking an anonymous user to an existing account](https://supabase.com/docs/guides/auth/auth-anonymous#linking-an-anonymous-user-to-an-existing-account) [Abuse prevention and rate limits](https://supabase.com/docs/guides/auth/auth-anonymous#abuse-prevention-and-rate-limits) [Automatic cleanup](https://supabase.com/docs/guides/auth/auth-anonymous#automatic-cleanup) [Resources](https://supabase.com/docs/guides/auth/auth-anonymous#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings