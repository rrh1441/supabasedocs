Edge Functions

# Regional Invocations

## How to execute an Edge Function in a particular region.

* * *

Edge Functions are executed in the region closest to the user making the request. This helps to reduce network latency and provide faster responses to the user.

However, if your Function performs lots of database or storage operations, invoking the Function in the same region as your database may provide better performance. Some situations where this might be helpful include:

- Bulk adding and editing records in your database
- Uploading files

Supabase provides an option to specify the region when invoking the Function.

## Using the `x-region` header [\#](https://supabase.com/docs/guides/functions/regional-invocation\#using-the-x-region-header)

Use the `x-region` HTTP header when calling an Edge Function to determine where the Function should be executed:

cURL

JavaScript

`
# https://supabase.com/docs/guides/functions/deploy#invoking-remote-functions
curl --request POST 'https://<project_ref>.supabase.co/functions/v1/hello-world' \
  --header 'Authorization: Bearer ANON_KEY' \
  --header 'Content-Type: application/json' \
  --header 'x-region: eu-west-3' \
  --data '{ "name":"Functions" }'
`

You can verify the execution region by looking at the `x-sb-edge-region` HTTP header in the response. You can also find it as metadata in [Edge Function Logs](https://supabase.com/docs/guides/functions/logging).

## Available regions [\#](https://supabase.com/docs/guides/functions/regional-invocation\#available-regions)

These are the currently supported region values you can provide for `x-region` header.

- `ap-northeast-1`
- `ap-northeast-2`
- `ap-south-1`
- `ap-southeast-1`
- `ap-southeast-2`
- `ca-central-1`
- `eu-central-1`
- `eu-west-1`
- `eu-west-2`
- `eu-west-3`
- `sa-east-1`
- `us-east-1`
- `us-west-1`
- `us-west-2`

## Using the client library [\#](https://supabase.com/docs/guides/functions/regional-invocation\#using-the-client-library)

You can also specify the region when invoking a Function using the Supabase client library:

`
const { createClient, FunctionRegion } = require('@supabase/supabase-js')
const { data: ret, error } = await supabase.functions.invoke('my-function-name', {
headers: { 'Content-Type': 'application/json' },
method: 'GET',
body: {},
region: FunctionRegion.UsEast1,
})
`

## Handling regional outages [\#](https://supabase.com/docs/guides/functions/regional-invocation\#handling-regional-outages)

If you explicitly specify the region via `x-region` header, requests **will NOT** be automatically re-routed to another region and you should consider temporarily changing regions during the outage.

### Is this helpful?

NoYes

### On this page

[Using the x-region header](https://supabase.com/docs/guides/functions/regional-invocation#using-the-x-region-header) [Available regions](https://supabase.com/docs/guides/functions/regional-invocation#available-regions) [Using the client library](https://supabase.com/docs/guides/functions/regional-invocation#using-the-client-library) [Handling regional outages](https://supabase.com/docs/guides/functions/regional-invocation#handling-regional-outages)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings