Getting Started

# Use Supabase with Nuxt

## Learn how to create a Supabase project, add some sample data to your database, and query the data from a Nuxt app.

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

### Create a Nuxt app

Create a Nuxt app using the `npx nuxi` command.

Terminal

`
npx nuxi@latest init my-app
`

3

### Install the Supabase client library

The fastest way to get started is to use the `supabase-js` client library which provides a convenient interface for working with Supabase from a Nuxt app.

Navigate to the Nuxt app and install `supabase-js`.

Terminal

`
cd my-app && npm install @supabase/supabase-js
`

4

### Query data from the app

In `app.vue`, create a Supabase client using your project URL and public API (anon) key:

###### Project URL

No project found

To get your Project URL, [log in](https://supabase.com/dashboard).

###### Anon key

No project found

To get your Anon key, [log in](https://supabase.com/dashboard).

Replace the existing content in your `app.vue` file with the following code.

app.vue

`
<script setup>
import { createClient } from '@supabase/supabase-js'
const supabase = createClient('https://<project>.supabase.co', '<your-anon-key>')
const instruments = ref([])
async function getInstruments() {
const { data } = await supabase.from('instruments').select()
instruments.value = data
}
onMounted(() => {
getInstruments()
})
</script>
<template>
<ul>
    <li v-for="instrument in instruments" :key="instrument.id">{{ instrument.name }}</li>
</ul>
</template>
`

5

### Start the app

Start the app, navigate to [http://localhost:3000](http://localhost:3000/) in the browser, open the browser console, and you should see the list of instruments.

Terminal

`
npm run dev
`

The community-maintained [@nuxtjs/supabase](https://supabase.nuxtjs.org/) module provides an alternate DX for working with Supabase in Nuxt.

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings