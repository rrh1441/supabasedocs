Edge Functions

# Upstash Redis

* * *

Increment Redis Counter in Edge Functions - YouTube

Supabase

45.5K subscribers

[Increment Redis Counter in Edge Functions](https://www.youtube.com/watch?v=OPg3_oPZCh0)

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

[Watch on YouTube](https://www.youtube.com/watch?v=OPg3_oPZCh0 "Watch on YouTube")

A Redis counter example that stores a [hash](https://redis.io/commands/hincrby/) of function invocation count per region. Find the code on [GitHub](https://github.com/supabase/supabase/tree/master/examples/edge-functions/supabase/functions/upstash-redis-counter).

## Redis database setup [\#](https://supabase.com/docs/guides/functions/examples/upstash-redis\#redis-database-setup)

Create a Redis database using the [Upstash Console](https://console.upstash.com/) or [Upstash CLI](https://github.com/upstash/cli).

Select the `Global` type to minimize the latency from all edge locations. Copy the `UPSTASH_REDIS_REST_URL` and `UPSTASH_REDIS_REST_TOKEN` to your .env file.

You'll find them under **Details > REST API > .env**.

`
cp supabase/functions/upstash-redis-counter/.env.example supabase/functions/upstash-redis-counter/.env
`

## Code [\#](https://supabase.com/docs/guides/functions/examples/upstash-redis\#code)

Make sure you have the latest version of the [Supabase CLI installed](https://supabase.com/docs/guides/cli#installation).

Create a new function in your project:

`
supabase functions new upstash-redis-counter
`

And add the code to the `index.ts` file:

index.ts

``
import { Redis } from 'https://deno.land/x/upstash_redis@v1.19.3/mod.ts'
console.log(`Function "upstash-redis-counter" up and running!`)
Deno.serve(async (_req) => {
try {
    const redis = new Redis({
      url: Deno.env.get('UPSTASH_REDIS_REST_URL')!,
      token: Deno.env.get('UPSTASH_REDIS_REST_TOKEN')!,
    })
    const deno_region = Deno.env.get('DENO_REGION')
    if (deno_region) {
      // Increment region counter
      await redis.hincrby('supa-edge-counter', deno_region, 1)
    } else {
      // Increment localhost counter
      await redis.hincrby('supa-edge-counter', 'localhost', 1)
    }
    // Get all values
    const counterHash: Record<string, number> | null = await redis.hgetall('supa-edge-counter')
    const counters = Object.entries(counterHash!)
      .sort(([, a], [, b]) => b - a) // sort desc
      .reduce((r, [k, v]) => ({ total: r.total + v, regions: { ...r.regions, [k]: v } }), {
        total: 0,
        regions: {},
      })
    return new Response(JSON.stringify({ counters }), { status: 200 })
} catch (error) {
    return new Response(JSON.stringify({ error: error.message }), { status: 200 })
}
})
``

## Run locally [\#](https://supabase.com/docs/guides/functions/examples/upstash-redis\#run-locally)

`
supabase start
supabase functions serve --no-verify-jwt --env-file supabase/functions/upstash-redis-counter/.env
`

Navigate to [http://localhost:54321/functions/v1/upstash-redis-counter](http://localhost:54321/functions/v1/upstash-redis-counter).

## Deploy [\#](https://supabase.com/docs/guides/functions/examples/upstash-redis\#deploy)

`
supabase functions deploy upstash-redis-counter --no-verify-jwt
supabase secrets set --env-file supabase/functions/upstash-redis-counter/.env
`

### Is this helpful?

NoYes

### On this page

[Redis database setup](https://supabase.com/docs/guides/functions/examples/upstash-redis#redis-database-setup) [Code](https://supabase.com/docs/guides/functions/examples/upstash-redis#code) [Run locally](https://supabase.com/docs/guides/functions/examples/upstash-redis#run-locally) [Deploy](https://supabase.com/docs/guides/functions/examples/upstash-redis#deploy)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings