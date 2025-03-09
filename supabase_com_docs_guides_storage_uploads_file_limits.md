Storage

# Limits

## Learn how to increase Supabase file limits.

* * *

## Global file size [\#](https://supabase.com/docs/guides/storage/uploads/file-limits\#global-file-size)

You can set the max file size across all your buckets by setting this global value in the dashboard [here](https://supabase.com/dashboard/project/_/settings/storage). For Free projects, the limit can't exceed 50 MB. On the Pro Plan and up, you can set this value to up to 50 GB. If you need more than 50 GB, [contact us](https://supabase.com/dashboard/support/new).

| Plan | Max File Size Limit |
| --- | --- |
| Free | 50 MB |
| Pro | 50 GB |
| Team | 50 GB |
| Enterprise | Custom |

This option is a global limit, which applies to all your buckets.

Additionally, you can specify the max file size on a per [bucket level](https://supabase.com/docs/guides/storage/buckets/creating-buckets#restricting-uploads) but it can't be higher than this global limit. As a good practice, the global limit should be set to the highest possible file size that your application accepts, and apply per bucket limits.

## Per bucket restrictions [\#](https://supabase.com/docs/guides/storage/uploads/file-limits\#per-bucket-restrictions)

You can have different restrictions on a per bucket level such as restricting the file types (e.g. `pdf`, `images`, `videos`) or the max file size, which should be lower than the global limit. To apply these limit on a bucket level see [Creating Buckets](https://supabase.com/docs/guides/storage/buckets/creating-buckets#restricting-uploads).

### Is this helpful?

NoYes

### On this page

[Global file size](https://supabase.com/docs/guides/storage/uploads/file-limits#global-file-size) [Per bucket restrictions](https://supabase.com/docs/guides/storage/uploads/file-limits#per-bucket-restrictions)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings