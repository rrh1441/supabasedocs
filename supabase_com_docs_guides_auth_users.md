Auth

# Users

* * *

A **user** in Supabase Auth is someone with a user ID, stored in the Auth schema. Once someone is a user, they can be issued an Access Token, which can be used to access Supabase endpoints. The token is tied to the user, so you can restrict access to resources via [RLS policies](https://supabase.com/docs/guides/database/postgres/row-level-security).

## Permanent and anonymous users [\#](https://supabase.com/docs/guides/auth/users\#permanent-and-anonymous-users)

Supabase distinguishes between permanent and anonymous users.

- **Permanent users** are tied to a piece of Personally Identifiable Information (PII), such as an email address, a phone number, or a third-party identity. They can use these identities to sign back into their account after signing out.
- **Anonymous users** aren't tied to any identities. They have a user ID and a personalized Access Token, but they have no way of signing back in as the same user if they are signed out.

Anonymous users are useful for:

- E-commerce applications, to create shopping carts before checkout
- Full-feature demos without collecting personal information
- Temporary or throw-away accounts

See the [Anonymous Signins guide](https://supabase.com/docs/guides/auth/auth-anonymous) to learn more about anonymous users.

##### Anonymous users do not use the anon role

Just like permanent users, anonymous users use the **authenticated** role for database access.

The **anon** role is for those who aren't signed in at all and are not tied to any user ID. We refer to these as unauthenticated or public users.

## The user object [\#](https://supabase.com/docs/guides/auth/users\#the-user-object)

The user object stores all the information related to a user in your application. The user object can be retrieved using one of these methods:

1. [`supabase.auth.getUser()`](https://supabase.com/docs/reference/javascript/auth-getuser)
2. Retrieve a user object as an admin using [`supabase.auth.admin.getUserById()`](https://supabase.com/docs/reference/javascript/auth-admin-listusers)

A user can sign in with one of the following methods:

- Password-based method (with email or phone)
- Passwordless method (with email or phone)
- OAuth
- SAML SSO

An identity describes the authentication method that a user can use to sign in. A user can have multiple identities. These are the types of identities supported:

- Email
- Phone
- OAuth
- SAML

A user with an email or phone identity will be able to sign in with either a password or passwordless method (e.g. use a one-time password (OTP) or magic link). By default, a user with an unverified email or phone number will not be able to sign in.

The user object contains the following attributes:

| Attributes | Type | Description |
| --- | --- | --- |
| id | `string` | The unique id of the identity of the user. |
| aud | `string` | The audience claim. |
| role | `string` | The role claim used by Postgres to perform Row Level Security (RLS) checks. |
| email | `string` | The user's email address. |
| email\_confirmed\_at | `string` | The timestamp that the user's email was confirmed. If null, it means that the user's email is not confirmed. |
| phone | `string` | The user's phone number. |
| phone\_confirmed\_at | `string` | The timestamp that the user's phone was confirmed. If null, it means that the user's phone is not confirmed. |
| confirmed\_at | `string` | The timestamp that either the user's email or phone was confirmed. If null, it means that the user does not have a confirmed email address and phone number. |
| last\_sign\_in\_at | `string` | The timestamp that the user last signed in. |
| app\_metadata | `object` | The `provider` attribute indicates the first provider that the user used to sign up with. The `providers` attribute indicates the list of providers that the user can use to login with. |
| user\_metadata | `object` | Defaults to the first provider's identity data but can contain additional custom user metadata if specified. Refer to [**User Identity**](https://supabase.com/docs/guides/auth/auth-identity-linking#the-user-identity) for more information about the identity object. |
| identities | `UserIdentity[]` | Contains an object array of identities linked to the user. |
| created\_at | `string` | The timestamp that the user was created. |
| updated\_at | `string` | The timestamp that the user was last updated. |
| is\_anonymous | `boolean` | Is true if the user is an anonymous user. |

## Resources [\#](https://supabase.com/docs/guides/auth/users\#resources)

- [User Management guide](https://supabase.com/docs/guides/auth/managing-user-data)

### Is this helpful?

NoYes

### On this page

[Permanent and anonymous users](https://supabase.com/docs/guides/auth/users#permanent-and-anonymous-users) [The user object](https://supabase.com/docs/guides/auth/users#the-user-object) [Resources](https://supabase.com/docs/guides/auth/users#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings