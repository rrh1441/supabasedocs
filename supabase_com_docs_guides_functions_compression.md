Edge Functions

# Handling Compressed Requests

## Handling Gzip compressed requests.

* * *

To decompress Gzip bodies, you can use `gunzipSync` from the `node:zlib` API to decompress and then read the body.

``
import { gunzipSync } from 'node:zlib'
Deno.serve(async (req) => {
try {
    // Check if the request body is gzip compressed
    const contentEncoding = req.headers.get('content-encoding')
    if (contentEncoding !== 'gzip') {
      return new Response('Request body is not gzip compressed', {
        status: 400,
      })
    }
    // Read the compressed body
    const compressedBody = await req.arrayBuffer()
    // Decompress the body
    const decompressedBody = gunzipSync(new Uint8Array(compressedBody))
    // Convert the decompressed body to a string
    const decompressedString = new TextDecoder().decode(decompressedBody)
    const data = JSON.parse(decompressedString)
    // Process the decompressed body as needed
    console.log(`Received: ${JSON.stringify(data)}`)
    return new Response('ok', {
      headers: { 'Content-Type': 'text/plain' },
    })
} catch (error) {
    console.error('Error:', error)
    return new Response('Error processing request', { status: 500 })
}
})
``

Edge functions have a runtime memory limit of 150MB. Overly large compressed payloads may result in an out-of-memory error.

### Is this helpful?

NoYes

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings