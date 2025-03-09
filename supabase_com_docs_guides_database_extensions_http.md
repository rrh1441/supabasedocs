Database

# http: RESTful Client

* * *

The `http` extension allows you to call RESTful endpoints within Postgres.

## Quick demo [\#](https://supabase.com/docs/guides/database/extensions/http\#quick-demo)

Using PostgreSQL functions to call an API with Supabase - YouTube

Supabase

45.5K subscribers

[Using PostgreSQL functions to call an API with Supabase](https://www.youtube.com/watch?v=rARgrELRCwY)

Supabase

Search

Info

Shopping

Tap to unmute

If playback doesn't begin shortly, try restarting your device.

You're signed out

Videos you watch may be added to the TV's watch history and influence TV recommendations. To avoid this, cancel and sign in to YouTube on your computer.

CancelConfirm

Share

Include playlist

An error occurred while retrieving sharing information. Please try again later.

Watch later

Share

Copy link

Watch on

0:00

/ •Live

•

[Watch on YouTube](https://www.youtube.com/watch?v=rARgrELRCwY "Watch on YouTube")

## Overview [\#](https://supabase.com/docs/guides/database/extensions/http\#overview)

Let's cover some basic concepts:

- REST: stands for REpresentational State Transfer. It's a way to request data from external services.
- RESTful APIs are servers which accept HTTP "calls". The calls are typically:
  - `GET` − Read only access to a resource.
  - `POST` − Creates a new resource.
  - `DELETE` − Removes a resource.
  - `PUT` − Updates an existing resource or creates a new resource.

You can use the `http` extension to make these network requests from Postgres.

## Usage [\#](https://supabase.com/docs/guides/database/extensions/http\#usage)

### Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/http\#enable-the-extension)

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Extensions** in the sidebar.
3. Search for `http` and enable the extension.

### Available functions [\#](https://supabase.com/docs/guides/database/extensions/http\#available-functions)

While the main usage is `http('http_request')`, there are 5 wrapper functions for specific functionality:

- `http_get()`
- `http_post()`
- `http_put()`
- `http_delete()`
- `http_head()`

### Returned values [\#](https://supabase.com/docs/guides/database/extensions/http\#returned-values)

A successful call to a web URL from the `http` extension returns a record with the following fields:

- `status`: integer
- `content_type`: character varying
- `headers`: http\_header\[\]
- `content`: character varying. Typically you would want to cast this to `jsonb` using the format `content::jsonb`

## Examples [\#](https://supabase.com/docs/guides/database/extensions/http\#examples)

### Simple `GET` example [\#](https://supabase.com/docs/guides/database/extensions/http\#simple-get-example)

`
select
"status", "content"::jsonb
from
http_get('https://jsonplaceholder.typicode.com/todos/1');
`

### Simple `POST` example [\#](https://supabase.com/docs/guides/database/extensions/http\#simple-post-example)

`
select
"status", "content"::jsonb
from
http_post(
    'https://jsonplaceholder.typicode.com/posts',
    '{ "title": "foo", "body": "bar", "userId": 1 }',
    'application/json'
);
`

## Resources [\#](https://supabase.com/docs/guides/database/extensions/http\#resources)

- Official [`http` GitHub Repository](https://github.com/pramsey/pgsql-http)

### Is this helpful?

NoYes

### On this page

[Quick demo](https://supabase.com/docs/guides/database/extensions/http#quick-demo) [Overview](https://supabase.com/docs/guides/database/extensions/http#overview) [Usage](https://supabase.com/docs/guides/database/extensions/http#usage) [Enable the extension](https://supabase.com/docs/guides/database/extensions/http#enable-the-extension) [Available functions](https://supabase.com/docs/guides/database/extensions/http#available-functions) [Returned values](https://supabase.com/docs/guides/database/extensions/http#returned-values) [Examples](https://supabase.com/docs/guides/database/extensions/http#examples) [Simple GET example](https://supabase.com/docs/guides/database/extensions/http#simple-get-example) [Simple POST example](https://supabase.com/docs/guides/database/extensions/http#simple-post-example) [Resources](https://supabase.com/docs/guides/database/extensions/http#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings