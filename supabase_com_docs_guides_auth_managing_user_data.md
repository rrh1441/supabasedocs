Auth

# User Management

## View, delete, and export user information.

* * *

You can view your users on the [Users page](https://supabase.com/dashboard/project/_/auth/users) of the Dashboard. You can also view the contents of the Auth schema in the [Table Editor](https://supabase.com/dashboard/project/_/editor).

## Accessing user data via API [\#](https://supabase.com/docs/guides/auth/managing-user-data\#accessing-user-data-via-api)

For security, the Auth schema is not exposed in the auto-generated API. If you want to access users data via the API, you can create your own user tables in the `public` schema.

Make sure to protect the table by enabling [Row Level Security](https://supabase.com/docs/guides/database/postgres/row-level-security). Reference the `auth.users` table to ensure data integrity. Specify `on delete cascade` in the reference. For example, a `public.profiles` table might look like this:

`
create table public.profiles (
id uuid not null references auth.users on delete cascade,
first_name text,
last_name text,
primary key (id)
);
alter table public.profiles enable row level security;
`

Only use primary keys as [foreign key references](https://www.postgresql.org/docs/current/tutorial-fk.html) for schemas and tables like `auth.users` which are managed by Supabase. Postgres lets you specify a foreign key reference for columns backed by a unique index (not necessarily primary keys).

Primary keys are **guaranteed not to change**. Columns, indices, constraints or other database objects managed by Supabase **may change at any time** and you should be careful when referencing them directly.

To update your `public.profiles` table every time a user signs up, set up a trigger. If the trigger fails, it could block signups, so test your code thoroughly.

`
-- inserts a row into public.profiles
create function public.handle_new_user()
returns trigger
language plpgsql
security definer set search_path = ''
as $$
begin
insert into public.profiles (id, first_name, last_name)
values (new.id, new.raw_user_meta_data ->> 'first_name', new.raw_user_meta_data ->> 'last_name');
return new;
end;
$$;
-- trigger the function every time a user is created
create trigger on_auth_user_created
after insert on auth.users
for each row execute procedure public.handle_new_user();
`

## Adding and retrieving user metadata [\#](https://supabase.com/docs/guides/auth/managing-user-data\#adding-and-retrieving-user-metadata)

You can assign metadata to users on sign up:

JavaScriptDartSwiftKotlin

`
const { data, error } = await supabase.auth.signUp({
email: 'valid.email@supabase.io',
password: 'example-password',
options: {
    data: {
      first_name: 'John',
      age: 27,
    },
},
})
`

User metadata is stored on the `raw_user_meta_data` column of the `auth.users` table. To view the metadata:

JavaScriptDartSwiftKotlin

`
const {
data: { user },
} = await supabase.auth.getUser()
let metadata = user.user_metadata
`

## Deleting users [\#](https://supabase.com/docs/guides/auth/managing-user-data\#deleting-users)

You may delete users directly or via the management console at Authentication > Users. Note that deleting a user from the `auth.users` table does not automatically sign out a user. As Supabase makes use of JSON Web Tokens (JWT), a user's JWT will remain "valid" until it has expired. Should you wish to immediately revoke access for a user, do consider making use of a Row Level Security policy as described below.

You cannot delete a user if they are the owner of any objects in Supabase Storage.

You will encounter an error when you try to delete an Auth user that owns any Storage objects. If this happens, try deleting all the objects for that user, or reassign ownership to another user.

## Exporting users [\#](https://supabase.com/docs/guides/auth/managing-user-data\#exporting-users)

As Supabase is built on top of Postgres, you can query the `auth.users` and `auth.identities` table via the `SQL Editor` tab to extract all users:

`
select * from auth.users;
`

You can then export the results as CSV.

### Is this helpful?

NoYes

### On this page

[Accessing user data via API](https://supabase.com/docs/guides/auth/managing-user-data#accessing-user-data-via-api) [Adding and retrieving user metadata](https://supabase.com/docs/guides/auth/managing-user-data#adding-and-retrieving-user-metadata) [Deleting users](https://supabase.com/docs/guides/auth/managing-user-data#deleting-users) [Exporting users](https://supabase.com/docs/guides/auth/managing-user-data#exporting-users)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings