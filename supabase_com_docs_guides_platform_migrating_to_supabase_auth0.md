Platform

# Migrate from Auth0 to Supabase Auth

## Learn how to migrate your users from Auth0

* * *

You can migrate your users from Auth0 to Supabase Auth.

Changing authentication providers for a production app is an important operation. It can affect most aspects of your application. Prepare in advance by reading this guide, and develop a plan for handling the key migration steps and possible problems.

With advance planning, a smooth and safe Auth migration is possible.

## Before you begin [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0\#before-you-begin)

Before beginning, consider the answers to the following questions. They will help you need decide if you need to migrate, and which strategy to use:

- How do Auth provider costs scale as your user base grows?
- Does the new Auth provider provide all needed features? (for example, OAuth, password logins, Security Assertion Markup Language (SAML), Multi-Factor Authentication (MFA))
- Is downtime acceptable during the migration?
- What is your timeline to migrate before terminating the old Auth provider?

## Migration strategies [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0\#migration-strategies)

Depending on your evaluation, you may choose to go with one of the following strategies:

1. Rolling migration
2. One-off migration

| Strategy | Advantages | Disadvantages |
| --- | --- | --- |
| Rolling | - 0 downtime<br>- Users may need to log in again | - Need to maintain 2 different Auth services, which may be more costly in the short-term<br>- Need to maintain separate codepaths for the period of the migration<br>- Some existing users may be inactive and have not signed in with the new provider. This means that you eventually need to backfill these users. However, this is a much smaller-scale one-off migration with lower risks since these users are inactive. |
| One-off | - No need to maintain 2 different auth services for an extended period of time | - Some downtime<br>- Users will need to log in again. Risky for active users. |

## Migration steps [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0\#migration-steps)

Auth provider migrations require 2 main steps:

1. Export your user data from the old provider (Auth0)
2. Import the data into your new provider (Supabase Auth)

### Step 1: Export your user data [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0\#step-1-export-your-user-data)

Auth0 provides two methods for exporting user data:

1. Use the [Auth0 data export feature](https://auth0.com/docs/troubleshoot/customer-support/manage-subscriptions/export-data)
2. Use the [Auth0 management API](https://auth0.com/docs/api/management/v2/users/get-users). This endpoint has a rate limit, so you may need to export your users in several batches.

To export password hashes and MFA factors, contact Auth0 support.

### Step 2: Import your users into Supabase Auth [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0\#step-2-import-your-users-into-supabase-auth)

The steps for importing your users depends on the login methods that you support.

See the following sections for how to import users with:

- [Password-based login](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0#password-based-methods)
- [Passwordless login](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0#passwordless-methods)
- [OAuth](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0#oauth)

#### Password-based methods [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0\#password-based-methods)

For users who sign in with passwords, we recommend a hybrid approach to reduce downtime:

1. For new users, use Supabase Auth for sign up.
2. Migrate existing users in a one-off migration.

##### Sign up new users

Sign up new users using Supabase Auth's [signin methods](https://supabase.com/docs/guides/auth/passwords#signing-up-with-an-email-and-password).

##### Migrate existing users to Supabase Auth

Migrate existing users to Supabase Auth. This requires two main steps: first, check which users need to be migrated, then create their accounts using the Supabase admin endpoints.

1. Get your Auth 0 user export and password hash export lists.

2. Filter for users who use password login.
   - Under the `identities` field in the user object, these users will have `auth0` as a provider. In the same identity object, you can find their Auth0 `user_id`.
   - Check that the user has a corresponding password hash by comparing their Auth0 `user_id` to the `oid` field in the password hash export.
3. Use Supabase Auth's [admin create user](https://supabase.com/docs/reference/javascript/auth-admin-createuser) method to recreate the user in Supabase Auth. If the user has a confirmed email address or phone number, set `email_confirm` or `phone_confirm` to `true`.



`
const { data, error } = await supabase.auth.admin.createUser({
email: 'valid.email@supabase.io',
password_hash: '$2y$10$a9pghn27d7m0ltXvlX8LiOowy7XfFw0hW0G80OjKYQ1jaoejaA7NC',
email_confirm: true,
})
`





##### Supported password hashing algorithms





Supabase supports bcrypt and Argon2 password hashes.





If you have a plaintext password instead of a hash, you can provide that instead. Supabase Auth will handle hashing the password for you. (Passwords are **always** stored hashed.)



`
const { data, error } = await supabase.auth.admin.createUser({
email: 'valid.email@supabase.io',
password: 'supersecurepassword123!',
})
`

4. To sign in your migrated users, use the Supabase Auth [sign in methods](https://supabase.com/docs/reference/javascript/auth-signinwithpassword).

To check for edge cases where users aren't successfully migrated, use a fallback strategy. This ensures that users can continue to sign in seamlessly:
1. Try to sign in the user with Supabase Auth.
2. If the signin fails, try to sign in with Auth0.
3. If Auth0 signin succeeds, call the admin create user method again to create the user in Supabase Auth.

#### Passwordless methods [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0\#passwordless-methods)

For passwordless signin via email or phone, check for users with verified email addresses or phone numbers. Create these users in Supabase Auth with `email_confirm` or `phone_confirm` set to `true`:

`
const { data, error } = await supabase.auth.admin.createUser({
email: 'valid.email@supabase.io',
email_confirm: true,
})
`

Check your Supabase Auth [email configuration](https://supabase.com/docs/guides/auth/auth-smtp) and configure your [email template](https://supabase.com/dashboard/project/_/auth/templates) for use with magic links. See the [Email templates guide](https://supabase.com/docs/guides/auth/auth-email-templates) to learn more.

Once you have imported your users, you can sign them in using the [`signInWithOtp`](https://supabase.com/docs/reference/javascript/auth-signinwithotp) method.

#### OAuth [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0\#oauth)

Configure your OAuth providers in Supabase by following the [Social login guides](https://supabase.com/docs/guides/auth/social-login).

For both new and existing users, sign in the user using the [`signInWithOAuth`](https://supabase.com/docs/reference/javascript/auth-signinwithoauth) method. This works without pre-migrating existing users, since the user always needs to sign in through the OAuth provider before being redirected to your service.

After the user has completed the OAuth flow successfully, you can check if the user is a new or existing user in Auth0 by mapping their social provider id to Auth0. Auth0 stores the social provider ID in the user ID, which has the format `provider_name|provider_id` (for example, `github|123456`). See the [Auth0 identity docs](https://auth0.com/docs/manage-users/user-accounts/identify-users) to learn more.

## Mapping between Auth0 and Supabase Auth [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0\#mapping-between-auth0-and-supabase-auth)

Each Auth provider has its own schema for tracking users and user information.

In Supabase Auth, your users are stored in your project's database under the `auth` schema. Every user has an identity (unless the user is an anonymous user), which represents the signin method they can use with Supabase. This is represented by the `auth.users` and `auth.identities` table.

See the [Users](https://supabase.com/docs/guides/auth/users) and [Identities](https://supabase.com/docs/guides/auth/identities) sections to learn more.

### Mapping user metadata and custom claims [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0\#mapping-user-metadata-and-custom-claims)

Supabase Auth provides 2 fields which you can use to map user-specific metadata from Auth0:

- `auth.users.raw_user_meta_data` : For storing non-sensitive user metadata that the user can update (e.g full name, age, favorite color).
- `auth.users.raw_app_meta_data` : For storing non-sensitive user metadata that the user should not be able to update (e.g pricing plan, access control roles).

Both columns are accessible from the admin user methods. To create a user with custom metadata, you can use the following method:

`
const { data, error } = await supabase.auth.admin.createUser({
email: 'valid.email@supabase.io',
user_metadata: {
    full_name: 'Foo Bar',
},
app_metadata: {
    role: 'admin',
},
})
`

These fields will be exposed in the user's access token JWT so it is recommended not to store excessive metadata in these fields.

These fields are stored as columns in the `auth.users` table using the `jsonb` type. Both fields can be updated by using the admin [`updateUserById` method](https://supabase.com/docs/reference/javascript/auth-admin-updateuserbyid). If you want to allow the user to update their own `raw_user_meta_data` , you can use the [`updateUser` method](https://supabase.com/docs/reference/javascript/auth-updateuser).

If you have a lot of user-specific metadata to store, it is recommended to create your own table in a private schema that uses the user id as a foreign key:

`
create table private.user_metadata (
	id int generated always as identity,
	user_id uuid references auth.users(id) on delete cascade,
	user_metadata jsonb
);
`

## Frequently Asked Questions (FAQ) [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0\#frequently-asked-questions-faq)

I have IDs assigned to existing users in my database, how can I maintain these IDs?

How can I allow my users to retain their existing password?

My users have multi-factor authentication (MFA) enabled, how do I make sure they don't have to set up MFA again?

How do I migrate existing SAML Single Sign-On (SSO) connections?

How do I migrate my Auth0 organizations to Supabase?

## Useful references [\#](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0\#useful-references)

- [Migrating 125k users from Auth0 to Supabase](https://kevcodez.medium.com/migrating-125-000-users-from-auth0-to-supabase-81c0568de307)
- [Loper to Supabase migration](https://eigen.sh/posts/auth-migration)

### Is this helpful?

NoYes

### On this page

[Before you begin](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0#before-you-begin) [Migration strategies](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0#migration-strategies) [Migration steps](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0#migration-steps) [Step 1: Export your user data](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0#step-1-export-your-user-data) [Step 2: Import your users into Supabase Auth](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0#step-2-import-your-users-into-supabase-auth) [Mapping between Auth0 and Supabase Auth](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0#mapping-between-auth0-and-supabase-auth) [Mapping user metadata and custom claims](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0#mapping-user-metadata-and-custom-claims) [Frequently Asked Questions (FAQ)](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0#frequently-asked-questions-faq) [Useful references](https://supabase.com/docs/guides/platform/migrating-to-supabase/auth0#useful-references)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings