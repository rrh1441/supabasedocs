Storage

# Delete Objects

## Learn about deleting objects

* * *

When you delete one or more objects from a bucket, the files are permanently removed and not recoverable. You can delete a single object or multiple objects at once.

Deleting objects should always be done via the **Storage API** and NOT via a **SQL query**. Deleting objects via a SQL query will not remove the object from the bucket and will result in the object being orphaned.

## Delete objects [\#](https://supabase.com/docs/guides/storage/management/delete-objects\#delete-objects)

To delete one or more objects, use the `remove` method.

`
await supabase.storage.from('bucket').remove(['object-path-2', 'folder/avatar2.png'])
`

## RLS [\#](https://supabase.com/docs/guides/storage/management/delete-objects\#rls)

To delete an object, the user must have the `delete` permission on the object. For example:

`
create policy "User can delete their own objects"
on storage.objects
for delete
TO authenticated
USING (
    owner = (select auth.uid()::text)
);
`

### Is this helpful?

NoYes

### On this page

[Delete objects](https://supabase.com/docs/guides/storage/management/delete-objects#delete-objects) [RLS](https://supabase.com/docs/guides/storage/management/delete-objects#rls)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings