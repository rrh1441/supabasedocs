REST API

# Securing your API

* * *

The data APIs are designed to work with Postgres Row Level Security (RLS). If you use [Supabase Auth](https://supabase.com/docs/guides/auth), you can restrict data based on the logged-in user.

To control access to your data, you can use [Policies](https://supabase.com/docs/guides/auth#policies).

## Enabling row level security [\#](https://supabase.com/docs/guides/api/securing-your-api\#enabling-row-level-security)

Any table you create in the `public` schema will be accessible via the Supabase Data API.

To restrict access, enable Row Level Security (RLS) on all tables, views, and functions in the `public` schema. You can then write RLS policies to grant users access to specific database rows or functions based on their authentication token.

Always enable Row Level Security on tables, views, and functions in the `public` schema to protect your data.

Any table created through the Supabase Dashboard will have RLS enabled by default. If you created the tables via the SQL editor or via another way, enable RLS like so:

DashboardSQL

1. Go to the [Authentication > Policies](https://supabase.com/dashboard/project/_/auth/policies) page in the Dashboard.
2. Select **Enable RLS** to enable Row Level Security.

With RLS enabled, you can create Policies that allow or disallow users to access and update data. We provide a detailed guide for creating Row Level Security Policies in our [Authorization documentation](https://supabase.com/docs/guides/database/postgres/row-level-security).

Any table **without RLS enabled** in the `public` schema will be accessible to the public, using the `anon` role. Always make sure that RLS is enabled or that you've got other security measures in place to avoid unauthorized access to your project's data!

## Disable the API or restrict to custom schema [\#](https://supabase.com/docs/guides/api/securing-your-api\#disable-the-api-or-restrict-to-custom-schema)

If you don't use the Data API, or if you don't want to expose the `public` schema, you can either disable it entirely or change the automatically exposed schema to one of your choice. See **[Hardening the Data API](https://supabase.com/docs/guides/database/hardening-data-api)** for instructions.

## Enforce additional rules on each request [\#](https://supabase.com/docs/guides/api/securing-your-api\#enforce-additional-rules-on-each-request)

Using Row Level Security policies may not always be adequate or sufficient to protect APIs.

Here are some common situations where additional protections are necessary:

- Enforcing per-IP or per-user rate limits.
- Checking custom or additional API keys before allowing further access.
- Rejecting requests after exceeding a quota or requiring payment.
- Disallowing direct access to certain tables, views or functions in the `public` schema.

You can build these cases in your application by creating a Postgres function that will read information from the request and perform additional checks, such as counting the number of requests received or checking that an API key is already registered in your database before serving the response.

Define a function like so:

`
create function public.check_request()
returns void
language plpgsql
security definer
as $$
begin
  -- your logic here
end;
$$;
`

And register it to run on every Data API request using:

`
alter role authenticator
set pgrst.db_pre_request = 'public.check_request';
`

This configures the `public.check_request` function to run on every Data API request. To have the changes take effect, you should run:

`
notify pgrst, 'reload config';
`

Inside the function you can perform any additional checks on the request headers or JWT and raise an exception to prevent the request from completing. For example, this exception raises a HTTP 402 Payment Required response with a `hint` and additional `X-Powered-By` header:

`
raise sqlstate 'PGRST' using
message = json_build_object(
    'code',    '123',
    'message', 'Payment Required',
    'details', 'Quota exceeded',
    'hint',    'Upgrade your plan')::text,
detail = json_build_object(
    'status',  402,
    'headers', json_build_object(
      'X-Powered-By', 'Nerd Rage'))::text;
`

When raised within the `public.check_request` function, the resulting HTTP response will look like:

`
HTTP/1.1 402 Payment Required
Content-Type: application/json; charset=utf-8
X-Powered-By: Nerd Rage
{
"message": "Payment Required",
"details": "Quota exceeded",
"hint": "Upgrade your plan",
"code": "123"
}
`

Use the [JSON operator functions](https://www.postgresql.org/docs/current/functions-json.html) to build rich and dynamic responses from exceptions.

If you use a custom HTTP status code like 419, you can supply the `status_text` key in the `detail` clause of the exception to describe the HTTP status.

If you're using PostgREST version 11 or lower ( [find out your PostgREST version](https://supabase.com/dashboard/project/_/settings/infrastructure)) a different and less powerful [syntax](https://postgrest.org/en/stable/references/errors.html#raise-errors-with-http-status-codes) needs to be used.

### Accessing request information [\#](https://supabase.com/docs/guides/api/securing-your-api\#accessing-request-information)

Like with RLS policies, you can access information about the request by using the `current_setting()` Postgres function. Here are some examples on how this works:

`
-- To get all the headers sent in the request
SELECT current_setting('request.headers', true)::json;
-- To get a single header, you can use JSON arrow operators
SELECT current_setting('request.headers', true)::json->>'user-agent';
-- Access Cookies
SELECT current_setting('request.cookies', true)::json;
`

| `current_setting()` | Example | Description |
| --- | --- | --- |
| `request.method` | `GET`, `HEAD`, `POST`, `PUT`, `PATCH`, `DELETE` | Request's method |
| `request.path` | `table` | Table's path |
| `request.path` | `view` | View's path |
| `request.path` | `rpc/function` | Functions's path |
| `request.headers` | `{ "User-Agent": "...", ... }` | JSON object of the request's headers |
| `request.cookies` | `{ "cookieA": "...", "cookieB": "..." }` | JSON object of the request's cookies |
| `request.jwt` | `{ "sub": "a7194ea3-...", ... }` | JSON object of the JWT payload |

To access the IP address of the client look up the [X-Forwarded-For header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-For) in the `request.headers` setting. For example:

`
SELECT split_part(
current_setting('request.headers', true)::json->>'x-forwarded-for',
',', 1); -- takes the client IP before the first comma (,)
`

Read more about [PostgREST's pre-request function](https://postgrest.org/en/stable/references/transactions.html#pre-request).

### Examples [\#](https://supabase.com/docs/guides/api/securing-your-api\#examples)

Rate limit per IPUse additional API keys

You can only rate-limit `POST`, `PUT`, `PATCH` and `DELETE` requests. This is because `GET` and `HEAD` requests run in read-only mode, and will be served by [Read Replicas](https://supabase.com/docs/guides/platform/read-replicas) which do not support writing to the database.

Outline:

- A new row is added to a `private.rate_limits` table each time a modifying action is done to the database containing the IP address and the timestamp of the action.
- If there are over 100 requests from the same IP address in the last 5 minutes, the request is rejected with a HTTP 420 code.

Create the table:

`
create table private.rate_limits (
ip inet,
request_at timestamp
);
-- add an index so that lookups are fast
create index rate_limits_ip_request_at_idx on private.rate_limits (ip, request_at desc);
`

The `private` schema is used as it cannot be accessed over the API!

Create the `public.check_request` function:

`
create function public.check_request()
returns void
language plpgsql
security definer
as $$
declare
req_method text := current_setting('request.method', true);
req_ip inet := split_part(
    current_setting('request.headers', true)::json->>'x-forwarded-for',
    ',', 1)::inet;
count_in_five_mins integer;
begin
if req_method = 'GET' or req_method = 'HEAD' or req_method is null then
    -- rate limiting can't be done on GET and HEAD requests
    return;
end if;
select
    count(*) into count_in_five_mins
from private.rate_limits
where
    ip = req_ip and request_at between now() - interval '5 minutes' and now();
if count_in_five_mins > 100 then
    raise sqlstate 'PGRST' using
      message = json_build_object(
        'message', 'Rate limit exceeded, try again after a while')::text,
      detail = json_build_object(
        'status',  420,
        'status_text', 'Enhance Your Calm')::text;
end if;
insert into private.rate_limits (ip, request_at) values (req_ip, now());
end;
$$;
`

Finally, configure the `public.check_request()` function to run on every Data API request:

`
alter role authenticator
set pgrst.db_pre_request = 'public.check_request';
notify pgrst, 'reload config';
`

To clear old entries in the `private.rate_limits` table, set up a [pg\_cron](https://supabase.com/docs/guides/database/extensions/pg_cron) job to clean them up.

### Is this helpful?

NoYes

### On this page

[Enabling row level security](https://supabase.com/docs/guides/api/securing-your-api#enabling-row-level-security) [Disable the API or restrict to custom schema](https://supabase.com/docs/guides/api/securing-your-api#disable-the-api-or-restrict-to-custom-schema) [Enforce additional rules on each request](https://supabase.com/docs/guides/api/securing-your-api#enforce-additional-rules-on-each-request) [Accessing request information](https://supabase.com/docs/guides/api/securing-your-api#accessing-request-information) [Examples](https://supabase.com/docs/guides/api/securing-your-api#examples)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings