Getting Started

# Use Supabase with SvelteKit

## Learn how to create a Supabase project, add some sample data to your database, and query the data from a SvelteKit app.

* * *

1

### Create a Supabase project

Go to [database.new](https://database.new/) and create a new Supabase project.

When your project is up and running, go to the [Table Editor](https://supabase.com/dashboard/project/_/editor), create a new table and insert some data.

Alternatively, you can run the following snippet in your project's [SQL Editor](https://supabase.com/dashboard/project/_/sql/new). This will create a `instruments` table with some sample data.

SQL\_EDITOR

`
-- Create the table
create table instruments (
id bigint primary key generated always as identity,
name text not null
);
-- Insert some sample data into the table
insert into instruments (name)
values
('violin'),
('viola'),
('cello');
alter table instruments enable row level security;
`

Make the data in your table publicly readable by adding an RLS policy:

SQL\_EDITOR

`
create policy "public can read instruments"
on public.instruments
for select to anon
using (true);
`

2

### Create a SvelteKit app

Create a SvelteKit app using the `npm create` command.

Terminal

`
npx sv create my-app
`

3

### Install the Supabase client library

The fastest way to get started is to use the `supabase-js` client library which provides a convenient interface for working with Supabase from a SvelteKit app.

Navigate to the SvelteKit app and install `supabase-js`.

Terminal

`
cd my-app && npm install @supabase/supabase-js
`

4

### Create the Supabase client

Create a `src/lib` directory in your SvelteKit app, create a file called `supabaseClient.js` and add the following code to initialize the Supabase client with your project URL and public API (anon) key:

###### Project URL

No project found

To get your Project URL, [log in](https://supabase.com/dashboard).

###### Anon key

No project found

To get your Anon key, [log in](https://supabase.com/dashboard).

src/lib/supabaseClient.js

`
import { createClient } from '@supabase/supabase-js'
export const supabase = createClient('https://<project>.supabase.co', '<your-anon-key>')
`

5

### Query data from the app

Use `load` method to fetch the data server-side and display the query results as a simple list.

Create `+page.server.js` file in the `src/routes` directory with the following code.

src/routes/+page.server.js

`
import { supabase } from "$lib/supabaseClient";
export async function load() {
    const { data } = await supabase.from("instruments").select();
    return {
      instruments: data ?? [],
    };
}
`

Replace the existing content in your `+page.svelte` file in the `src/routes` directory with the following code.

src/routes/+page.svelte

`
<script>
    let { data } = $props();
</script>
<ul>
    {#each data.instruments as instrument}
      <li>{instrument.name}</li>
    {/each}
</ul>
`

6

### Start the app

Start the app and go to [http://localhost:5173](http://localhost:5173/) in a browser and you should see the list of instruments.

Terminal

`
npm run dev
`

## Next steps [\#](https://supabase.com/docs/guides/getting-started/quickstarts/sveltekit\#next-steps)

- Set up [Auth](https://supabase.com/docs/guides/auth) for your app
- [Insert more data](https://supabase.com/docs/guides/database/import-data) into your database
- Upload and serve static files using [Storage](https://supabase.com/docs/guides/storage)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings