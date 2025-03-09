Storage

# Storage Optimizations

## Scaling Storage

* * *

Here are some optimizations that you can consider to improve performance and reduce costs as you start scaling Storage.

## Egress [\#](https://supabase.com/docs/guides/storage/production/scaling\#egress)

If your project has high egress, these optimizations can help reducing it.

#### Resize images [\#](https://supabase.com/docs/guides/storage/production/scaling\#resize-images)

Images typically make up most of your egress. By keeping them as small as possible, you can cut down on egress and boost your application's performance. You can take advantage of our [Image Transformation](https://supabase.com/docs/guides/storage/serving/image-transformations) service to optimize any image on the fly.

#### Set a high cache-control value [\#](https://supabase.com/docs/guides/storage/production/scaling\#set-a-high-cache-control-value)

Using the browser cache can effectively lower your egress since the asset remains stored in the user's browser after the initial download. Setting a high `cache-control` value ensures the asset stays in the user's browser for an extended period, decreasing the need to download it from the server repeatedly. Read more [here](https://supabase.com/docs/guides/storage/cdn/smart-cdn#cache-duration)

#### Limit the upload size [\#](https://supabase.com/docs/guides/storage/production/scaling\#limit-the-upload-size)

You have the option to set a maximum upload size for your bucket. Doing this can prevent users from uploading and then downloading excessively large files. You can control the maximum file size by configuring this option at the [bucket level](https://supabase.com/docs/guides/storage/buckets/creating-buckets).

## Optimize listing objects [\#](https://supabase.com/docs/guides/storage/production/scaling\#optimize-listing-objects)

Once you have a substantial number of objects, you might observe that the `supabase.storage.list()` method starts to slow down. This occurs because the endpoint is quite generic and attempts to retrieve both folders and objects in a single query. While this approach is very useful for building features like the Storage viewer on the Supabase dashboard, it can impact performance with a large number of objects.

If your application doesn't need the entire hierarchy computed you can speed up drastically the query execution for listing your objects by creating a Postgres function as following:

`
create or replace function list_objects(
    bucketid text,
    prefix text,
    limits int default 100,
    offsets int default 0
) returns table (
    name text,
    id uuid,
    updated_at timestamptz,
    created_at timestamptz,
    last_accessed_at timestamptz,
    metadata jsonb
) as $$
begin
    return query SELECT
        objects.name,
        objects.id,
        objects.updated_at,
        objects.created_at,
        objects.last_accessed_at,
        objects.metadata
    FROM storage.objects
    WHERE objects.name like prefix || '%'
    AND bucket_id = bucketid
    ORDER BY name ASC
    LIMIT limits
    OFFSET offsets;
end;
$$ language plpgsql stable;
`

You can then use the your Postgres function as following:

Using SQL:

`
select * from list_objects('bucket_id', '', 100, 0);
`

Using the SDK:

`
const { data, error } = await supabase.rpc('list_objects', {
bucketid: 'yourbucket',
prefix: '',
limit: 100,
offset: 0,
})
`

## Optimizing RLS [\#](https://supabase.com/docs/guides/storage/production/scaling\#optimizing-rls)

When creating RLS policies against the storage tables you can add indexes to the interested columns to speed up the lookup

### Is this helpful?

NoYes

### On this page

[Egress](https://supabase.com/docs/guides/storage/production/scaling#egress) [Optimize listing objects](https://supabase.com/docs/guides/storage/production/scaling#optimize-listing-objects) [Optimizing RLS](https://supabase.com/docs/guides/storage/production/scaling#optimizing-rls)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings