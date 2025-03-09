# Dashboard errors when managing users

Last edited: 2/4/2025

* * *

## PROBLEM [\#](https://supabase.com/docs/guides/troubleshooting/dashboard-errors-when-managing-users-N1ls4A\#problem)

### Receiving the following or _similar_ error messages in the Dashboard when managing users. [\#](https://supabase.com/docs/guides/troubleshooting/dashboard-errors-when-managing-users-N1ls4A\#receiving-the-following-or-similar-error-messages-in-the-dashboard-when-managing-users)

> ... Database error
> ... Error sending

![](https://github.com/supabase/supabase/assets/91111415/91732e7d-0e83-4bc8-8ce3-57ebd871d981)![](https://github.com/supabase/supabase/assets/91111415/30f4cd9f-3736-40ae-8606-2f060cfac0d0)![](https://github.com/supabase/supabase/assets/91111415/50961019-b461-44cb-9e4b-f1361aa3ad5d)

### Or, receiving a comparable 500 error from the Auth REST API: [\#](https://supabase.com/docs/guides/troubleshooting/dashboard-errors-when-managing-users-N1ls4A\#or-receiving-a-comparable-500-error-from-the-auth-rest-api)

> Database error ...

![Screenshot 2024-02-13 at 10 59 17 PM](https://github.com/supabase/supabase/assets/91111415/cd151dda-5160-415c-925c-cea332887641)

## SOLUTION 1 (trigger related) [\#](https://supabase.com/docs/guides/troubleshooting/dashboard-errors-when-managing-users-N1ls4A\#solution-1-trigger-related)

Check if the auth schema contains any triggers in the [Dashboard's trigger section](https://supabase.com/dashboard/project/_/database/triggers). Remove all triggers by dropping their functions with a CASCADE modifier:

`
DROP FUNCTION <function name>() CASCADE;
-- If you'd prefer, you can drop the trigger alone with the following query:
-- DROP TRIGGER <trigger_name> on auth.<table_name>;
`

Then recreate the functions with a [security definer](https://supabase.com/docs/guides/database/functions#security-definer-vs-invoker) modifier before recreating the triggers.

The [SQL Editor](https://supabase.com/dashboard/project/_/sql/) contains a template for [User Management](https://supabase.com/dashboard/project/_/sql/quickstarts). Within it, there is a working example of how to setup triggers with security definer that may be worth referencing:

`
create table profiles (
id uuid references auth.users on delete cascade not null primary key,
updated_at timestamp with time zone,
username text unique,
full_name text,
avatar_url text,
website text,
constraint username_length check (char_length(username) >= 3)
);
create function public.handle_new_user()
returns trigger as $$
begin
insert into public.profiles (id, full_name, avatar_url)
values (new.id, new.raw_user_meta_data->>'full_name', new.raw_user_meta_data->>'avatar_url');
return new;
end;
$$ language plpgsql security definer;
create trigger on_auth_user_created
after insert on auth.users
for each row execute procedure public.handle_new_user();
`

### EXPLANATION [\#](https://supabase.com/docs/guides/troubleshooting/dashboard-errors-when-managing-users-N1ls4A\#explanation)

One of the most common design patterns in Supabase is to add a trigger to the `auth.users` table. The database role managing authentication (supabase\_auth\_admin) only has the necessary permissions it needs to perform its duties. So, when a trigger operated by the supabase\_auth\_admin interacts outside the auth schema, it causes a permission error.

A security definer function retains the privileges of the database user that created it. As long as it is the `postgres` role, your auth triggers should be able to engage with outside tables.

## SOLUTION 2 (constraint related) [\#](https://supabase.com/docs/guides/troubleshooting/dashboard-errors-when-managing-users-N1ls4A\#solution-2-constraint-related)

If you did not create a trigger, check if you created a foreign/primary key relationship between the auth.users table and another table. If you did, then ALTER the [behavior](https://stackoverflow.com/questions/5383612/setting-up-table-relations-what-do-cascade-set-null-and-restrict-do) of the relationship and recreate it with a less [restrictive constraint](https://stackoverflow.com/questions/3359329/how-to-change-the-foreign-key-referential-action-behavior).

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings