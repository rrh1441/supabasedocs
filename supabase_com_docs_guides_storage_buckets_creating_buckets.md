Storage

# Creating Buckets

* * *

You can create a bucket using the Supabase Dashboard. Since storage is interoperable with your Postgres database, you can also use SQL or our client libraries.
Here we create a bucket called "avatars":

JavaScriptDashboardSQLDartSwiftPython

`
// Use the JS library to create a bucket.
const { data, error } = await supabase.storage.createBucket('avatars', {
public: true, // default: false
})
`

[Reference.](https://supabase.com/docs/reference/javascript/storage-createbucket)

## Restricting uploads [\#](https://supabase.com/docs/guides/storage/buckets/creating-buckets\#restricting-uploads)

When creating a bucket you can add additional configurations to restrict the type or size of files you want this bucket to contain.
For example, imagine you want to allow your users to upload only images to the `avatars` bucket and the size must not be greater than 1MB.

You can achieve the following by providing: `allowedMimeTypes` and `maxFileSize`

`
// Use the JS library to create a bucket.
const { data, error } = await supabase.storage.createBucket('avatars', {
public: true,
allowedMimeTypes: ['image/*'],
fileSizeLimit: '1MB',
})
`

If an upload request doesn't meet the above restrictions it will be rejected.

For more information check [File Limits](https://supabase.com/docs/guides/storage/uploads/file-limits) Section.

### Is this helpful?

NoYes

### On this page

[Restricting uploads](https://supabase.com/docs/guides/storage/buckets/creating-buckets#restricting-uploads)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings