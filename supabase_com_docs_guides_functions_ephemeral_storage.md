Edge Functions

# Ephemeral Storage

## Read and write from temporary directory

* * *

Edge Functions provides ephemeral file storage. You can read and write files to the `/tmp` directory.

Ephemeral storage will reset on each function invocation. This means the files you write during an invocation can only be read within the same invocation.

### Use cases [\#](https://supabase.com/docs/guides/functions/ephemeral-storage\#use-cases)

Here are some use cases where ephemeral storage can be useful:

- Unzip an archive of CSVs and then add them as records to the DB
- Custom image manipulation workflows (using [`magick-wasm`](https://supabase.com/docs/guides/functions/examples/image-manipulation))

You can use [Background Tasks](https://supabase.com/docs/guides/functions/background-tasks) to handle slow file processing outside of a request.

### How to use [\#](https://supabase.com/docs/guides/functions/ephemeral-storage\#how-to-use)

You can use [Deno File System APIs](https://docs.deno.com/api/deno/file-system) or the [`node:fs` module](https://docs.deno.com/api/node/fs/) to access the `/tmp` path.

### Example [\#](https://supabase.com/docs/guides/functions/ephemeral-storage\#example)

Here is an example of how to write a user-uploaded zip file into temporary storage for further processing.

`
Deno.serve(async (req) => {
if (req.headers.get('content-type') !== 'application/zip') {
    return new Response('file must be a zip file', {
      status: 400,
    })
}
const uploadId = crypto.randomUUID()
await Deno.writeFile('/tmp/' + uploadId, req.body)
// do something with the written zip file
return new Response('ok')
})
`

### Unavailable APIs [\#](https://supabase.com/docs/guides/functions/ephemeral-storage\#unavailable-apis)

Currently, the synchronous APIs (e.g. `Deno.writeFileSync` or `Deno.mkdirSync`) for creating or writing files are not supported.

You can use sync variations of read APIs (e.g. `Deno.readFileSync`).

### Limits [\#](https://supabase.com/docs/guides/functions/ephemeral-storage\#limits)

In the hosted platform, a free project can write up to 256MB of data to ephemeral storage. A paid project can write up to 512MB.

### Is this helpful?

NoYes

### On this page

[Use cases](https://supabase.com/docs/guides/functions/ephemeral-storage#use-cases) [How to use](https://supabase.com/docs/guides/functions/ephemeral-storage#how-to-use) [Example](https://supabase.com/docs/guides/functions/ephemeral-storage#example) [Unavailable APIs](https://supabase.com/docs/guides/functions/ephemeral-storage#unavailable-apis) [Limits](https://supabase.com/docs/guides/functions/ephemeral-storage#limits)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings