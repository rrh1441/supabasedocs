REST API

# Using Custom Schemas

* * *

By default, your database has a `public` schema which is automatically exposed on data APIs.

## Creating custom schemas [\#](https://supabase.com/docs/guides/api/using-custom-schemas\#creating-custom-schemas)

You can create your own custom schema/s by running the following SQL, substituting `myschema` with the name you want to use for your schema:

`
CREATE SCHEMA myschema;
`

## Exposing custom schemas [\#](https://supabase.com/docs/guides/api/using-custom-schemas\#exposing-custom-schemas)

You can expose custom database schemas - to do so you need to follow these steps:

1. Go to [API settings](https://supabase.com/dashboard/project/_/settings/api) and add your custom schema to "Exposed schemas".
2. Run the following SQL, substituting `myschema` with your schema name:

`
GRANT USAGE ON SCHEMA myschema TO anon, authenticated, service_role;
GRANT ALL ON ALL TABLES IN SCHEMA myschema TO anon, authenticated, service_role;
GRANT ALL ON ALL ROUTINES IN SCHEMA myschema TO anon, authenticated, service_role;
GRANT ALL ON ALL SEQUENCES IN SCHEMA myschema TO anon, authenticated, service_role;
ALTER DEFAULT PRIVILEGES FOR ROLE postgres IN SCHEMA myschema GRANT ALL ON TABLES TO anon, authenticated, service_role;
ALTER DEFAULT PRIVILEGES FOR ROLE postgres IN SCHEMA myschema GRANT ALL ON ROUTINES TO anon, authenticated, service_role;
ALTER DEFAULT PRIVILEGES FOR ROLE postgres IN SCHEMA myschema GRANT ALL ON SEQUENCES TO anon, authenticated, service_role;
`

Now you can access these schemas from data APIs:

JavaScriptDartcURL

`
// Initialize the JS client
import { createClient } from '@supabase/supabase-js'
const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY, { db: { schema: 'myschema' } })
// Make a request
const { data: todos, error } = await supabase.from('todos').select('*')
// You can also change the target schema on a per-query basis
const { data: todos, error } = await supabase.schema('myschema').from('todos').select('*')
`

### Is this helpful?

NoYes

### On this page

[Creating custom schemas](https://supabase.com/docs/guides/api/using-custom-schemas#creating-custom-schemas) [Exposing custom schemas](https://supabase.com/docs/guides/api/using-custom-schemas#exposing-custom-schemas)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings