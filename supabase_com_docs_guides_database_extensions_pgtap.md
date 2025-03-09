Database

# pgTAP: Unit Testing

* * *

`pgTAP` is a unit testing extension for Postgres.

## Overview [\#](https://supabase.com/docs/guides/database/extensions/pgtap\#overview)

Let's cover some basic concepts:

- Unit tests: allow you to test small parts of a system (like a database table!).
- TAP: stands for [Test Anything Protocol](http://testanything.org/). It is an framework which aims to simplify the error reporting during testing.

## Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/pgtap\#enable-the-extension)

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Extensions** in the sidebar.
3. Search for `pgtap` and enable the extension.

## Testing tables [\#](https://supabase.com/docs/guides/database/extensions/pgtap\#testing-tables)

`
begin;
select plan( 1 );
select has_table( 'profiles' );
select * from finish();
rollback;
`

API:

- [`has_table()`](https://pgtap.org/documentation.html#has_table): Tests whether or not a table exists in the database
- [`has_index()`](https://pgtap.org/documentation.html#has_index): Checks for the existence of a named index associated with the named table.
- [`has_relation()`](https://pgtap.org/documentation.html#has_relation): Tests whether or not a relation exists in the database.

## Testing columns [\#](https://supabase.com/docs/guides/database/extensions/pgtap\#testing-columns)

`
begin;
select plan( 2 );
select has_column( 'profiles', 'id' ); -- test that the "id" column exists in the "profiles" table
select col_is_pk( 'profiles', 'id' ); -- test that the "id" column is a primary key
select * from finish();
rollback;
`

API:

- [`has_column()`](https://pgtap.org/documentation.html#has_column): Tests whether or not a column exists in a given table, view, materialized view or composite type.
- [`col_is_pk()`](https://pgtap.org/documentation.html#col_is_pk): Tests whether the specified column or columns in a table is/are the primary key for that table.

## Testing RLS policies [\#](https://supabase.com/docs/guides/database/extensions/pgtap\#testing-rls-policies)

`
begin;
select plan( 1 );
select policies_are(
'public',
'profiles',
ARRAY [\
    'Profiles are public', -- Test that there is a policy called  "Profiles are public" on the "profiles" table.\
    'Profiles can only be updated by the owner'  -- Test that there is a policy called  "Profiles can only be updated by the owner" on the "profiles" table.\
]
);
select * from finish();
rollback;
`

API:

- [`policies_are()`](https://pgtap.org/documentation.html#policies_are): Tests that all of the policies on the named table are only the policies that should be on that table.
- [`policy_roles_are()`](https://pgtap.org/documentation.html#policy_roles_are): Tests whether the roles to which policy applies are only the roles that should be on that policy.
- [`policy_cmd_is()`](https://pgtap.org/documentation.html#policy_cmd_is): Tests whether the command to which policy applies is same as command that is given in function arguments.

You can also use the `results_eq()` method to test that a Policy returns the correct data:

`
begin;
select plan( 1 );
select results_eq(
    'select * from profiles()',
    $$VALUES ( 1, 'Anna'), (2, 'Bruce'), (3, 'Caryn')$$,
    'profiles() should return all users'
);
select * from finish();
rollback;
`

API:

- [`results_eq()`](https://pgtap.org/documentation.html#results_eq)
- [`results_ne()`](https://pgtap.org/documentation.html#results_ne)

## Testing functions [\#](https://supabase.com/docs/guides/database/extensions/pgtap\#testing-functions)

`
prepare hello_expr as select 'hello'
begin;
select plan(3);
-- You'll need to create a hello_world and is_even function
select function_returns( 'hello_world', 'text' );                   -- test if the function "hello_world" returns text
select function_returns( 'is_even', ARRAY['integer'], 'boolean' );  -- test if the function "is_even" returns a boolean
select results_eq('select * from hello_world()', 'hello_expr');          -- test if the function "hello_world" returns "hello"
select * from finish();
rollback;
`

API:

- [`function_returns()`](https://pgtap.org/documentation.html#function_returns): Tests that a particular function returns a particular data type
- [`is_definer()`](https://pgtap.org/documentation.html#is_definer): Tests that a function is a security definer (that is, a `setuid` function).

## Resources [\#](https://supabase.com/docs/guides/database/extensions/pgtap\#resources)

- Official [`pgTAP` documentation](https://pgtap.org/)

### Is this helpful?

NoYes

### On this page

[Overview](https://supabase.com/docs/guides/database/extensions/pgtap#overview) [Enable the extension](https://supabase.com/docs/guides/database/extensions/pgtap#enable-the-extension) [Testing tables](https://supabase.com/docs/guides/database/extensions/pgtap#testing-tables) [Testing columns](https://supabase.com/docs/guides/database/extensions/pgtap#testing-columns) [Testing RLS policies](https://supabase.com/docs/guides/database/extensions/pgtap#testing-rls-policies) [Testing functions](https://supabase.com/docs/guides/database/extensions/pgtap#testing-functions) [Resources](https://supabase.com/docs/guides/database/extensions/pgtap#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings