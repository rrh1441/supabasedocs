Storage

# Storage Access Control

* * *

Supabase Storage is designed to work perfectly with Postgres [Row Level Security](https://supabase.com/docs/guides/database/postgres/row-level-security) (RLS).

You can use RLS to create [Security Access Policies](https://www.postgresql.org/docs/current/sql-createpolicy.html) that are incredibly powerful and flexible, allowing you to restrict access based on your business needs.

## Access policies [\#](https://supabase.com/docs/guides/storage/security/access-control\#access-policies)

By default Storage does not allow any uploads to buckets without RLS policies. You selectively allow certain operations by creating RLS policies on the `storage.objects` table.

You can find the documentation for the storage schema [here](https://supabase.com/docs/guides/storage/schema/design) , and to simplify the process of crafting your policies, you can utilize these [helper functions](https://supabase.com/docs/guides/storage/schema/helper-functions) .

The RLS policies required for different operations are documented [here](https://supabase.com/docs/reference/javascript/storage-createbucket)

For example, the only RLS policy required for [uploading](https://supabase.com/docs/reference/javascript/storage-from-upload) objects is to grant the `INSERT` permission to the `storage.objects` table.

To allow overwriting files using the `upsert` functionality you will need to additionally grant `SELECT` and `UPDATE` permissions.

## Policy examples [\#](https://supabase.com/docs/guides/storage/security/access-control\#policy-examples)

An easy way to get started would be to create RLS policies for `SELECT`, `INSERT`, `UPDATE`, `DELETE` operations and restrict the policies to meet your security requirements. For example, one can start with the following `INSERT` policy:

`
create policy "policy_name"
ON storage.objects
for insert with check (
true
);
`

and modify it to only allow authenticated users to upload assets to a specific bucket by changing it to:

`
create policy "policy_name"
on storage.objects for insert to authenticated with check (
    -- restrict bucket
    bucket_id = 'my_bucket_id'
);
`

This example demonstrates how you would allow authenticated users to upload files to a folder called `private` inside `my_bucket_id`:

`
create policy "Allow authenticated uploads"
on storage.objects
for insert
to authenticated
with check (
bucket_id = 'my_bucket_id' and
(storage.foldername(name))[1] = 'private'
);
`

This example demonstrates how you would allow authenticated users to upload files to a folder called with their `users.id` inside `my_bucket_id`:

`
create policy "Allow authenticated uploads"
on storage.objects
for insert
to authenticated
with check (
bucket_id = 'my_bucket_id' and
(storage.foldername(name))[1] = (select auth.uid()::text)
);
`

Allow a user to access a file that was previously uploaded by the same user:

`
create policy "Individual user Access"
on storage.objects for select
to authenticated
using ( (select auth.uid()) = owner_id::uuid );
`

* * *

How to Configure Access Control on Your Supabase Storage Buckets - SupabaseTips - YouTube

Supabase

45.5K subscribers

[How to Configure Access Control on Your Supabase Storage Buckets - SupabaseTips](https://www.youtube.com/watch?v=4ERX__Y908k)

Supabase

Search

Info

Shopping

Tap to unmute

If playback doesn't begin shortly, try restarting your device.

You're signed out

Videos you watch may be added to the TV's watch history and influence TV recommendations. To avoid this, cancel and sign in to YouTube on your computer.

CancelConfirm

Share

Include playlist

An error occurred while retrieving sharing information. Please try again later.

Watch later

Share

Copy link

Watch on

0:00

/ •Live

•

[Watch on YouTube](https://www.youtube.com/watch?v=4ERX__Y908k "Watch on YouTube")

## Bypassing access controls [\#](https://supabase.com/docs/guides/storage/security/access-control\#bypassing-access-controls)

If you exclusively use Storage from trusted clients, such as your own servers, and need to bypass the RLS policies, you can use the `service key` in the `Authorization` header. Service keys entirely bypass RLS policies, granting you unrestricted access to all Storage APIs.

Remember you should not share the service key publicly.

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2F4ERX__Y908k%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Access policies](https://supabase.com/docs/guides/storage/security/access-control#access-policies) [Policy examples](https://supabase.com/docs/guides/storage/security/access-control#policy-examples) [Bypassing access controls](https://supabase.com/docs/guides/storage/security/access-control#bypassing-access-controls)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings