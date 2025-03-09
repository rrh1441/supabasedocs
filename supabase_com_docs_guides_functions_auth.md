Edge Functions

# Integrating With Supabase Auth

## Supabase Edge Functions and Auth.

* * *

Edge Functions work seamlessly with [Supabase Auth](https://supabase.com/docs/guides/auth).

## Auth context [\#](https://supabase.com/docs/guides/functions/auth\#auth-context)

When a user makes a request to an Edge Function, you can use the Authorization header to set the Auth context in the Supabase client:

`
import { createClient } from 'jsr:@supabase/supabase-js@2'
Deno.serve(async (req: Request) => {
const supabaseClient = createClient(
    Deno.env.get('SUPABASE_URL') ?? '',
    Deno.env.get('SUPABASE_ANON_KEY') ?? '',
);
// Get the session or user object
const authHeader = req.headers.get('Authorization')!;
const token = authHeader.replace('Bearer ', '');
const { data } = await supabaseClient.auth.getUser(token);
})
`

Importantly, this is done _inside_ the `Deno.serve()` callback argument, so that the Authorization header is set for each request.

## Fetching the user [\#](https://supabase.com/docs/guides/functions/auth\#fetching-the-user)

After initializing a Supabase client with the Auth context, you can use `getUser()` to fetch the user object, and run queries in the context of the user with [Row Level Security (RLS)](https://supabase.com/docs/guides/database/postgres/row-level-security) policies enforced.

`
import { createClient } from 'jsr:@supabase/supabase-js@2'
Deno.serve(async (req: Request) => {
const supabaseClient = createClient(
    Deno.env.get('SUPABASE_URL') ?? '',
    Deno.env.get('SUPABASE_ANON_KEY') ?? '',
)
// Get the session or user object
const authHeader = req.headers.get('Authorization')!
const token = authHeader.replace('Bearer ', '')
const { data } = await supabaseClient.auth.getUser(token)
const user = data.user
return new Response(JSON.stringify({ user }), {
    headers: { 'Content-Type': 'application/json' },
    status: 200,
})
})
`

## Row Level Security [\#](https://supabase.com/docs/guides/functions/auth\#row-level-security)

After initializing a Supabase client with the Auth context, all queries will be executed with the context of the user. For database queries, this means [Row Level Security](https://supabase.com/docs/guides/database/postgres/row-level-security) will be enforced.

`
import { createClient } from 'jsr:@supabase/supabase-js@2'
Deno.serve(async (req: Request) => {
const supabaseClient = createClient(
    Deno.env.get('SUPABASE_URL') ?? '',
    Deno.env.get('SUPABASE_ANON_KEY') ?? '',
);
// Get the session or user object
const authHeader = req.headers.get('Authorization')!;
const token = authHeader.replace('Bearer ', '');
const { data: userData } = await supabaseClient.auth.getUser(token);
const { data, error } = await supabaseClient.from('profiles').select('*');
return new Response(JSON.stringify({ data }), {
    headers: { 'Content-Type': 'application/json' },
    status: 200,
})
})
`

## Example code [\#](https://supabase.com/docs/guides/functions/auth\#example-code)

See a full [example on GitHub](https://github.com/supabase/supabase/blob/master/examples/edge-functions/supabase/functions/select-from-table-with-auth-rls/index.ts).

### Is this helpful?

NoYes

### On this page

[Auth context](https://supabase.com/docs/guides/functions/auth#auth-context) [Fetching the user](https://supabase.com/docs/guides/functions/auth#fetching-the-user) [Row Level Security](https://supabase.com/docs/guides/functions/auth#row-level-security) [Example code](https://supabase.com/docs/guides/functions/auth#example-code)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings