Edge Functions

# Generating OG Images

* * *

Generating OG Images with Edge Functions - YouTube

Supabase

45.5K subscribers

[Generating OG Images with Edge Functions](https://www.youtube.com/watch?v=jZgyOJGWayQ)

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

[Watch on YouTube](https://www.youtube.com/watch?v=jZgyOJGWayQ "Watch on YouTube")

Generate Open Graph images with Deno and Supabase Edge Functions. [View on GitHub](https://github.com/supabase/supabase/tree/master/examples/edge-functions/supabase/functions/opengraph).

## Code [\#](https://supabase.com/docs/guides/functions/examples/og-image\#code)

Create a `handler.tsx` file to construct the OG image in React:

handler.tsx

`
import React from 'https://esm.sh/react@18.2.0'
import { ImageResponse } from 'https://deno.land/x/og_edge@0.0.4/mod.ts'
export default function handler(req: Request) {
return new ImageResponse(
    (
      <div
        style={{
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
          fontSize: 128,
          background: 'lavender',
        }}
      >
        Hello OG Image!
      </div>
    )
)
}
`

Create an `index.ts` file to execute the handler on incoming requests:

index.ts

`
import handler from './handler.tsx'
console.log('Hello from og-image Function!')
Deno.serve(handler)
`

### Is this helpful?

NoYes

### On this page

[Code](https://supabase.com/docs/guides/functions/examples/og-image#code)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings