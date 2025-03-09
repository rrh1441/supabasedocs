Database

# Working With Arrays

* * *

Postgres supports flexible [array types](https://www.postgresql.org/docs/12/arrays.html). These arrays are also supported in the Supabase Dashboard and in the JavaScript API.

## Create a table with an array column [\#](https://supabase.com/docs/guides/database/arrays\#create-a-table-with-an-array-column)

Create a test table with a text array (an array of strings):

DashboardSQL

1. Go to the [Table editor](https://supabase.com/dashboard/project/_/editor) page in the Dashboard.
2. Click **New Table** and create a table with the name `arraytest`.
3. Click **Save**.
4. Click **New Column** and create a column with the name `textarray`, type `text`, and select **Define as array**.
5. Click **Save**.

## Insert a record with an array value [\#](https://supabase.com/docs/guides/database/arrays\#insert-a-record-with-an-array-value)

DashboardSQLJavaScriptSwiftPython

1. Go to the [Table editor](https://supabase.com/dashboard/project/_/editor) page in the Dashboard.
2. Select the `arraytest` table.
3. Click **Insert row** and add `["Harry", "Larry", "Moe"]`.
4. Click **Save.**

## View the results [\#](https://supabase.com/docs/guides/database/arrays\#view-the-results)

DashboardSQL

1. Go to the [Table editor](https://supabase.com/dashboard/project/_/editor) page in the Dashboard.
2. Select the `arraytest` table.

You should see:

`
| id  | textarray               |
| --- | ----------------------- |
| 1   | ["Harry","Larry","Moe"] |
`

## Query array data [\#](https://supabase.com/docs/guides/database/arrays\#query-array-data)

Postgres uses 1-based indexing (e.g., `textarray[1]` is the first item in the array).

SQLJavaScriptSwift

To select the first item from the array and get the total length of the array:

`
SELECT textarray[1], array_length(textarray, 1) FROM arraytest;
`

returns:

`
| textarray | array_length |
| --------- | ------------ |
| Harry     | 3            |
`

## Resources [\#](https://supabase.com/docs/guides/database/arrays\#resources)

- [Supabase JS Client](https://github.com/supabase/supabase-js)
- [Supabase - Get started for free](https://supabase.com/)
- [Postgres Arrays](https://www.postgresql.org/docs/15/arrays.html)

### Is this helpful?

NoYes

### On this page

[Create a table with an array column](https://supabase.com/docs/guides/database/arrays#create-a-table-with-an-array-column) [Insert a record with an array value](https://supabase.com/docs/guides/database/arrays#insert-a-record-with-an-array-value) [View the results](https://supabase.com/docs/guides/database/arrays#view-the-results) [Query array data](https://supabase.com/docs/guides/database/arrays#query-array-data) [Resources](https://supabase.com/docs/guides/database/arrays#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings