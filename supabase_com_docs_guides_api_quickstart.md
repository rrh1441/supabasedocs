REST API

# Build an API route in less than 2 minutes.

## Create your first API route by creating a table called `todos` to store tasks.

* * *

Let's create our first REST route which we can query using `cURL` or the browser.

We'll create a database table called `todos` for storing tasks. This creates a corresponding API route `/rest/v1/todos` which can accept `GET`, `POST`, `PATCH`, & `DELETE` requests.

1

### Set up a Supabase project with a 'todos' table

[Create a new project](https://supabase.com/dashboard) in the Supabase Dashboard.

After your project is ready, create a table in your Supabase database. You can do this with either the Table interface or the [SQL Editor](https://supabase.com/dashboard/project/_/sql).

SQLDashboard

`
-- Create a table called "todos"
-- with a column to store tasks.
create table todos (
id serial primary key,
task text
);
`

2

### Allow public access

Let's turn on Row Level Security for this table and allow public access.

`
-- Turn on security
alter table "todos"
enable row level security;
-- Allow anonymous access
create policy "Allow public access"
on todos
for select
to anon
using (true);
`

3

### Insert some dummy data

Now we can add some data to our table which we can access through our API.

`
insert into todos (task)
values
('Create tables'),
('Enable security'),
('Add data'),
('Fetch data from the API');
`

4

### Fetch the data

Find your API URL and Keys in your Dashboard [API Settings](https://supabase.com/dashboard/project/_/settings/api). You can now query your "todos" table by appending `/rest/v1/todos` to the API URL.

Copy this block of code, substitute `<PROJECT_REF>` and `<ANON_KEY>`, then run it from a terminal.

Terminal

`
curl 'https://<PROJECT_REF>.supabase.co/rest/v1/todos' \
-H "apikey: <ANON_KEY>" \
-H "Authorization: Bearer <ANON_KEY>"
`

## Bonus [\#](https://supabase.com/docs/guides/api/quickstart\#bonus)

There are several options for accessing your data:

### Browser [\#](https://supabase.com/docs/guides/api/quickstart\#browser)

You can query the route in your browser, by appending the `anon` key as a query parameter:

`https://<PROJECT_REF>.supabase.co/rest/v1/todos?apikey=<ANON_KEY>`

### Client libraries [\#](https://supabase.com/docs/guides/api/quickstart\#client-libraries)

We provide a number of [Client Libraries](https://github.com/supabase/supabase#client-libraries).

JavaScriptDartPythonSwift

`
const { data, error } = await supabase.from('todos').select()
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings