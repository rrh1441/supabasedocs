REST API

# Creating API Routes

* * *

API routes are automatically created when you create Postgres Tables, Views, or Functions.

## Create a table [\#](https://supabase.com/docs/guides/api/creating-routes\#create-a-table)

Let's create our first API route by creating a table called `todos` to store tasks.
This creates a corresponding route `todos` which can accept `GET`, `POST`, `PATCH`, & `DELETE` requests.

DashboardSQL

1. Go to the [Table editor](https://supabase.com/dashboard/project/_/editor) page in the Dashboard.
2. Click **New Table** and create a table with the name `todos`.
3. Click **Save**.
4. Click **New Column** and create a column with the name `task` and type `text`.
5. Click **Save**.

## API URL and keys [\#](https://supabase.com/docs/guides/api/creating-routes\#api-url-and-keys)

Every Supabase project has a unique API URL. Your API is secured behind an API gateway which requires an API Key for every request.

1. Go to the [Settings](https://supabase.com/dashboard/project/_/settings/general) page in the Dashboard.
2. Click **API** in the sidebar.
3. Find your API `URL`, `anon`, and `service_role` keys on this page.

The REST API is accessible through the URL `https://<project_ref>.supabase.co/rest/v1`

Both of these routes require the `anon` key to be passed through an `apikey` header.

## Using the API [\#](https://supabase.com/docs/guides/api/creating-routes\#using-the-api)

You can interact with your API directly via HTTP requests, or you can use the client libraries which we provide.

Let's see how to make a request to the `todos` table which we created in the first step,
using the API URL ( `SUPABASE_URL`) and Key ( `SUPABASE_ANON_KEY`) we provided:

JavascriptcURL

`
// Initialize the JS client
import { createClient } from '@supabase/supabase-js'
const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY)
// Make a request
const { data: todos, error } = await supabase.from('todos').select('*')
`

JS Reference: [`select()`](https://supabase.com/docs/reference/javascript/select),
[`insert()`](https://supabase.com/docs/reference/javascript/insert),
[`update()`](https://supabase.com/docs/reference/javascript/update),
[`upsert()`](https://supabase.com/docs/reference/javascript/upsert),
[`delete()`](https://supabase.com/docs/reference/javascript/delete),
[`rpc()`](https://supabase.com/docs/reference/javascript/rpc) (call Postgres functions).

### Is this helpful?

NoYes

### On this page

[Create a table](https://supabase.com/docs/guides/api/creating-routes#create-a-table) [API URL and keys](https://supabase.com/docs/guides/api/creating-routes#api-url-and-keys) [Using the API](https://supabase.com/docs/guides/api/creating-routes#using-the-api)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings