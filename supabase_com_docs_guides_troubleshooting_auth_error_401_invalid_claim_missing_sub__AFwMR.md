# Auth error: {401: invalid claim: missing sub}

Last edited: 2/4/2025

* * *

The missing sub claim error is returned when `supabase.auth.getUser()` is called with an invalid JWT in the session or when the user attempts to register/sign in but hasn't completed the sign in when the `getUser` call is made.

A common pitfall, is inadvertently using a Supabase API key (such as the anon or service\_role keys) instead of the Supabase Auth access token.

**Why Does This Happen?**

- The Supabase API keys are designed for different purposes and, while they are recognized by the Supabase Auth system, they do not carry the sub claim. The sub claim is essential as it encodes the user ID, which is a mandatory field for authentication processes. This mistake leads to the "missing sub claim" error because the system expects a token that contains user identification information.

**How to Avoid This Issue:**

- Ensure that the token being passed to `supabase.auth.getUser()` is, indeed, an Auth access token and not one of the API keys.

- Are you creating the client on a per-request basis or are you creating a global client to be shared? If you're creating the client on a per-request basis, then you need to pass the session with the user's JWT from the client to the server somehow. This can be done by sending the user's JWT in a header like an `Authorization: Bearer <user_jwt>` . You can then get this header and call `supabase.auth.getUser(user_jwt)` with the user's JWT.

- Examine how the Supabase client is being initialized, especially in server-side scenarios.


1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings