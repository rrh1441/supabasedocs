Storage

# The Storage Schema

## Learn about the storage schema

* * *

Storage uses Postgres to store metadata regarding your buckets and objects. Users can use RLS (Row-Level Security) policies for access control. This data is stored in a dedicated schema within your project called `storage`.

When working with SQL, it's crucial to consider all records in Storage tables as read-only. All operations, including uploading, copying, moving, and deleting, should **exclusively go through the API**.

This is important because the storage schema only stores the metadata and the actual objects are stored in a provider like S3. Deleting the metadata doesn't remove the object in the underlying storage provider. This results in your object being inaccessible, but you'll still be billed for it.

Here is the schema that represents the Storage service:

![Storage schema design](https://supabase.com/docs/img/storage/schema-design.png)

You have the option to query this table directly to retrieve information about your files in Storage without the need to go through our API.

## Modifying the schema [\#](https://supabase.com/docs/guides/storage/schema/design\#modifying-the-schema)

We strongly recommend refraining from making any alterations to the `storage` schema and treating it as read-only. This approach is important because any modifications to the schema on your end could potentially clash with our future updates, leading to downtime.

However, we encourage you to add custom indexes as they can significantly improve the performance of the RLS policies you create for enforcing access control.

### Is this helpful?

NoYes

### On this page

[Modifying the schema](https://supabase.com/docs/guides/storage/schema/design#modifying-the-schema)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings