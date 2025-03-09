# Deprecated RLS features

Last edited: 3/7/2025

* * *

## The `auth.role()` function is now deprecated [\#](https://supabase.com/docs/guides/troubleshooting/deprecated-rls-features-Pm77Zs\#the-authrole-function-is-now-deprecated)

The `auth.role()` function has been deprecated in favour of using the `TO` field, natively supported within Postgres:

`
-- DEPRECATED
create policy "Public profiles are viewable by everyone."
on profiles for select using (
auth.role() = 'authenticated' or auth.role() = 'anon'
);
-- RECOMMENDED
create policy "Public profiles are viewable by everyone."
on profiles for select
to authenticated, anon
using (
true
);
`

## The `auth.email()` function is now deprecated [\#](https://supabase.com/docs/guides/troubleshooting/deprecated-rls-features-Pm77Zs\#the-authemail-function-is-now-deprecated)

The `auth.email()` function has been deprecated in favour a more generic function to return the full JWT:

`
- DEPRECATED
create policy "User can view their profile."
on profiles for select using (
auth.email() = email
);
-- RECOMMENDED
create policy "User can view their profile."
on profiles for select using (
(auth.jwt() ->> 'email') = email
);
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings