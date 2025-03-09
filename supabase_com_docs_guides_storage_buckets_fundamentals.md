Storage

# Storage Buckets

* * *

Buckets allow you to keep your files organized and determines the [Access Model](https://supabase.com/docs/guides/storage/buckets/fundamentals#access-model) for your assets. [Upload restrictions](https://supabase.com/docs/guides/storage/buckets/creating-buckets#restricting-uploads) like max file size and allowed content types are also defined at the bucket level.

## Access model [\#](https://supabase.com/docs/guides/storage/buckets/fundamentals\#access-model)

There are 2 access models for buckets, **public** and **private** buckets.

### Private buckets [\#](https://supabase.com/docs/guides/storage/buckets/fundamentals\#private-buckets)

When a bucket is set to **Private** all operations are subject to access control via [RLS policies](https://supabase.com/docs/guides/storage/security/access-control). This also applies when downloading assets. Buckets are private by default.

The only ways to download assets within a private bucket is to:

- Use the [download method](https://supabase.com/docs/reference/javascript/storage-from-download) by providing a authorization header containing your user's JWT. The RLS policy you create on the `storage.objects` table will use this user to determine if they have access.
- Create a signed URL with the [`createSignedUrl` method](https://supabase.com/docs/reference/javascript/storage-from-createsignedurl) that can be accessed for a limited time.

#### Example use cases: [\#](https://supabase.com/docs/guides/storage/buckets/fundamentals\#example-use-cases)

- Uploading users' sensitive documents
- Securing private assets by using RLS to set up fine-grain access controls

### Public buckets [\#](https://supabase.com/docs/guides/storage/buckets/fundamentals\#public-buckets)

When a bucket is designated as 'Public,' it effectively bypasses access controls for both retrieving and serving files within the bucket. This means that anyone who possesses the asset URL can readily access the file.

Access control is still enforced for other types of operations including uploading, deleting, moving, and copying.

#### Example use cases: [\#](https://supabase.com/docs/guides/storage/buckets/fundamentals\#example-use-cases)

- User profile pictures
- User public media
- Blog post content

Public buckets are more performant than private buckets since they are [cached differently](https://supabase.com/docs/guides/storage/cdn/fundamentals#public-vs-private-buckets).

### Is this helpful?

NoYes

### On this page

[Access model](https://supabase.com/docs/guides/storage/buckets/fundamentals#access-model) [Private buckets](https://supabase.com/docs/guides/storage/buckets/fundamentals#private-buckets) [Public buckets](https://supabase.com/docs/guides/storage/buckets/fundamentals#public-buckets)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings