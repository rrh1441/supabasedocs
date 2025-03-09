Realtime

# Migrate to Broadcast Changes

## How to migrate from Postgres Changes to Broadcast Changes

* * *

Postgres Changes has some [limitations](https://supabase.com/docs/guides/realtime/postgres-changes#limitations) as your application scales. To continue broadcasting database changes to users as you scale, you can use Broadcast Changes.

## Example application using Postgres Changes [\#](https://supabase.com/docs/guides/realtime/migrate-from-postgres-changes\#example-application-using-postgres-changes)

Here we have a simple chess application that has a game id and we want to track whenever we have new moves happening for a given game id.

We store this information in a `public.moves` table and every time a new move is added to a given `game_id` we want to receive the changes in our connected Realtime client

![Schema used for our example](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Frealtime%2Frealtime-broadcast-changes-migration-schema-example-light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

In our client we will have our implementation to receive insert events with the usual code:

``
const gameId = '4a8bbe89-f601-4414-bd47-8d0f7ab2a31a'
const changes = supabase
.channel('chess-moves')
.on(
    'postgres_changes',
    {
      event: 'INSERT',
      schema: 'public',
      table: 'moves',
      filter: `game_id=eq.${gameId}`,
    },
    (payload) => console.log(payload)
)
.subscribe()
...
``

## Migrate to broadcast changes [\#](https://supabase.com/docs/guides/realtime/migrate-from-postgres-changes\#migrate-to-broadcast-changes)

To use Broadcast Changes, first familiarize yourself with the [Broadcast Changes implementation](https://supabase.com/docs/guides/realtime/broadcast#trigger-broadcast-messages-from-your-database).

### Set up authorization [\#](https://supabase.com/docs/guides/realtime/migrate-from-postgres-changes\#set-up-authorization)

Broadcast Changes is private by default, using [Realtime Authorization](https://supabase.com/docs/guides/realtime/authorization) to control access. First, set up RLS policies to control user access to relevant messages:

`
CREATE POLICY "authenticated can listen to game moves"
ON "realtime"."messages"
FOR SELECT
TO authenticated
USING (
EXISTS (
    SELECT 1
    FROM game_users
    WHERE (SELECT auth.uid()) = user_id
      AND (select realtime.topic()) = 'games:' || game_id::text
      AND realtime.messages.extension = 'broadcast'
)
);
`

### Set up trigger function [\#](https://supabase.com/docs/guides/realtime/migrate-from-postgres-changes\#set-up-trigger-function)

We need to define our trigger function to adapt to our use case and use the provided function `realtime.broadcast_changes`

`
CREATE OR REPLACE FUNCTION public.broadcast_moves() RETURNS trigger AS $$
BEGIN
    PERFORM realtime.broadcast_changes(
	    'games:' || NEW.game_id::text,   -- topic
		   TG_OP,                          -- event
		   TG_OP,                          -- operation
		   TG_TABLE_NAME,                  -- table
		   TG_TABLE_SCHEMA,                -- schema
		   NEW,                            -- new record
		   OLD                             -- old record
		);
    RETURN NULL;
END;
$$ LANGUAGE plpgsql;
`

### Setup trigger with created function [\#](https://supabase.com/docs/guides/realtime/migrate-from-postgres-changes\#setup-trigger-with-created-function)

Now we need to setup our trigger to capture the events we want

`
CREATE TRIGGER chess_move_changes
AFTER INSERT ON public.moves
FOR EACH ROW
EXECUTE FUNCTION public.broadcast_moves();
`

### **Listen to changes in client** [\#](https://supabase.com/docs/guides/realtime/migrate-from-postgres-changes\#listen-to-changes-in-client)

Finally you can setup your client to listen for your events

``
const gameId = '4a8bbe89-f601-4414-bd47-8d0f7ab2a31a'
await supabase.realtime.setAuth() // Needed for Realtime Authorization
const changes = supabase
.channel(`games:${gameId}`)
.on(
    'broadcast',
    {
      event: 'INSERT',
    },
    (payload) => console.log(payload)
)
.subscribe()
``

### Is this helpful?

NoYes

### On this page

[Example application using Postgres Changes](https://supabase.com/docs/guides/realtime/migrate-from-postgres-changes#example-application-using-postgres-changes) [Migrate to broadcast changes](https://supabase.com/docs/guides/realtime/migrate-from-postgres-changes#migrate-to-broadcast-changes) [Set up authorization](https://supabase.com/docs/guides/realtime/migrate-from-postgres-changes#set-up-authorization) [Set up trigger function](https://supabase.com/docs/guides/realtime/migrate-from-postgres-changes#set-up-trigger-function) [Setup trigger with created function](https://supabase.com/docs/guides/realtime/migrate-from-postgres-changes#setup-trigger-with-created-function) [Listen to changes in client](https://supabase.com/docs/guides/realtime/migrate-from-postgres-changes#listen-to-changes-in-client)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings