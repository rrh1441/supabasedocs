Database

# Replication

* * *

Replication is a technique for copying the data from one database to another. Supabase uses replication functionality to provide a real-time API. Replication is useful for:

- Spreading out the "load." For example, if your database has a lot of reads, you might want to split it between two databases.
- Reducing latency. For example, you may want one database in London to serve your European customers, and one in New York to serve the US.

Replication is done through _publications_, a method of choosing which changes to send to other systems (usually another Postgres database). Publications can be managed in the [Dashboard](https://supabase.com/dashboard) or with SQL.

## Manage publications in the dashboard [\#](https://supabase.com/docs/guides/database/replication\#manage-publications-in-the-dashboard)

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Publications** in the sidebar.
3. Control which database events are sent by toggling **Insert**, **Update**, and **Delete**.
4. Control which tables broadcast changes by selecting **Source** and toggling each table.

## Create a publication [\#](https://supabase.com/docs/guides/database/replication\#create-a-publication)

This publication contains changes to all tables.

`
create publication publication_name
for all tables;
`

## Create a publication to listen to individual tables [\#](https://supabase.com/docs/guides/database/replication\#create-a-publication-to-listen-to-individual-tables)

`
create publication publication_name
for table table_one, table_two;
`

## Add tables to an existing publication [\#](https://supabase.com/docs/guides/database/replication\#add-tables-to-an-existing-publication)

`
alter publication publication_name
add table table_name;
`

## Listen to `insert` [\#](https://supabase.com/docs/guides/database/replication\#listen-to-insert)

`
create publication publication_name
for all tables
with (publish = 'insert');
`

## Listen to `update` [\#](https://supabase.com/docs/guides/database/replication\#listen-to-update)

`
create publication publication_name
for all tables
with (publish = 'update');
`

## Listen to `delete` [\#](https://supabase.com/docs/guides/database/replication\#listen-to-delete)

`
create publication publication_name
for all tables
with (publish = 'delete');
`

## Remove a publication [\#](https://supabase.com/docs/guides/database/replication\#remove-a-publication)

`
drop publication if exists publication_name;
`

## Recreate a publication [\#](https://supabase.com/docs/guides/database/replication\#recreate-a-publication)

If you're recreating a publication, it's best to do it in a transaction to ensure the operation succeeds.

`
begin;
  -- remove the realtime publication
drop publication if exists publication_name;
  -- re-create the publication but don't enable it for any tables
create publication publication_name;
commit;
`

### Is this helpful?

NoYes

### On this page

[Manage publications in the dashboard](https://supabase.com/docs/guides/database/replication#manage-publications-in-the-dashboard) [Create a publication](https://supabase.com/docs/guides/database/replication#create-a-publication) [Create a publication to listen to individual tables](https://supabase.com/docs/guides/database/replication#create-a-publication-to-listen-to-individual-tables) [Add tables to an existing publication](https://supabase.com/docs/guides/database/replication#add-tables-to-an-existing-publication) [Listen to insert](https://supabase.com/docs/guides/database/replication#listen-to-insert) [Listen to update](https://supabase.com/docs/guides/database/replication#listen-to-update) [Listen to delete](https://supabase.com/docs/guides/database/replication#listen-to-delete) [Remove a publication](https://supabase.com/docs/guides/database/replication#remove-a-publication) [Recreate a publication](https://supabase.com/docs/guides/database/replication#recreate-a-publication)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings