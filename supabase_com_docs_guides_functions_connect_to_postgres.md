Edge Functions

# Connecting directly to Postgres

## Connecting to Postgres from Edge Functions.

* * *

Connect to your Postgres database from an Edge Function by using the `supabase-js` client.
You can also use other Postgres clients like [Deno Postgres](https://deno.land/x/postgres)

## Using supabase-js [\#](https://supabase.com/docs/guides/functions/connect-to-postgres\#using-supabase-js)

The `supabase-js` client is a great option for connecting to your Supabase database since it handles authorization with Row Level Security, and it automatically formats your response as JSON.

index.ts

`
import { createClient } from 'jsr:@supabase/supabase-js@2'
Deno.serve(async (req) => {
try {
    const supabase = createClient(
      Deno.env.get('SUPABASE_URL') ?? '',
      Deno.env.get('SUPABASE_ANON_KEY') ?? '',
      { global: { headers: { Authorization: req.headers.get('Authorization')! } } }
    )
    const { data, error } = await supabase.from('countries').select('*')
    if (error) {
      throw error
    }
    return new Response(JSON.stringify({ data }), {
      headers: { 'Content-Type': 'application/json' },
      status: 200,
    })
} catch (err) {
    return new Response(String(err?.message ?? err), { status: 500 })
}
})
`

## Using a Postgres client [\#](https://supabase.com/docs/guides/functions/connect-to-postgres\#using-a-postgres-client)

Because Edge Functions are a server-side technology, it's safe to connect directly to your database using any popular Postgres client. This means you can run raw SQL from your Edge Functions.

Here is how you can connect to the database using Deno Postgres driver and run raw SQL.

Check out the [full example](https://github.com/supabase/supabase/tree/master/examples/edge-functions/supabase/functions/postgres-on-the-edge).

index.ts

``
import * as postgres from 'https://deno.land/x/postgres@v0.17.0/mod.ts'
// Get the connection string from the environment variable "SUPABASE_DB_URL"
const databaseUrl = Deno.env.get('SUPABASE_DB_URL')!
// Create a database pool with three connections that are lazily established
const pool = new postgres.Pool(databaseUrl, 3, true)
Deno.serve(async (_req) => {
try {
    // Grab a connection from the pool
    const connection = await pool.connect()
    try {
      // Run a query
      const result = await connection.queryObject`SELECT * FROM animals`
      const animals = result.rows // [{ id: 1, name: "Lion" }, ...]
      // Encode the result as pretty printed JSON
      const body = JSON.stringify(
        animals,
        (key, value) => (typeof value === 'bigint' ? value.toString() : value),
        2
      )
      // Return the response with the correct content type header
      return new Response(body, {
        status: 200,
        headers: { 'Content-Type': 'application/json; charset=utf-8' },
      })
    } finally {
      // Release the connection back into the pool
      connection.release()
    }
} catch (err) {
    console.error(err)
    return new Response(String(err?.message ?? err), { status: 500 })
}
})
``

## Using Drizzle [\#](https://supabase.com/docs/guides/functions/connect-to-postgres\#using-drizzle)

You can use Drizzle together with [Postgres.js](https://github.com/porsager/postgres). Both can be loaded directly from npm:

supabase/functions/import\_map.json

`
{
"imports": {
    "drizzle-orm": "npm:drizzle-orm@0.29.1",
    "drizzle-orm/": "npm:/drizzle-orm@0.29.1/",
    "postgres": "npm:postgres@3.4.3"
}
}
`

supabase/functions/drizzle/index.ts

`
import { drizzle } from 'drizzle-orm/postgres-js'
import postgres from 'postgres'
import { countries } from '../_shared/schema.ts'
const connectionString = Deno.env.get('SUPABASE_DB_URL')!
Deno.serve(async (_req) => {
// Disable prefetch as it is not supported for "Transaction" pool mode
const client = postgres(connectionString, { prepare: false })
const db = drizzle(client)
const allCountries = await db.select().from(countries)
return Response.json(allCountries)
})
`

You can find the full example on [GitHub](https://github.com/thorwebdev/edgy-drizzle).

## SSL connections [\#](https://supabase.com/docs/guides/functions/connect-to-postgres\#ssl-connections)

Deployed edge functions are pre-configured to use SSL for connections to the Supabase database. You don't need to add any extra configurations.

If you want to use SSL connections during local development, follow these steps:

- Download the SSL certificate from [Database settings](https://supabase.com/dashboard/project/_/settings/database)

- In your [local .env file](https://supabase.com/docs/guides/functions/secrets), add these two variables:


`
SSL_CERT_FILE=/path/to/cert.crt # set the path to the downloaded cert
DENO_TLS_CA_STORE=mozilla,system
`

Connecting to Postgres from Edge Functions - YouTube

Supabase

45.5K subscribers

[Connecting to Postgres from Edge Functions](https://www.youtube.com/watch?v=cl7EuF1-RsY)

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

[Watch on YouTube](https://www.youtube.com/watch?v=cl7EuF1-RsY "Watch on YouTube")

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2Fcl7EuF1-RsY%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Using supabase-js](https://supabase.com/docs/guides/functions/connect-to-postgres#using-supabase-js) [Using a Postgres client](https://supabase.com/docs/guides/functions/connect-to-postgres#using-a-postgres-client) [Using Drizzle](https://supabase.com/docs/guides/functions/connect-to-postgres#using-drizzle) [SSL connections](https://supabase.com/docs/guides/functions/connect-to-postgres#ssl-connections)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings