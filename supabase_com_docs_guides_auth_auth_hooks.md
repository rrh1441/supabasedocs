Auth

# Auth Hooks

## Use HTTP or Postgres Functions to customize your authentication flow

* * *

## What is a hook [\#](https://supabase.com/docs/guides/auth/auth-hooks\#what-is-a-hook)

A hook is an endpoint that allows you to alter the default Supabase Auth flow at specific execution points. Developers can use hooks to add custom behavior that's not supported natively.

Hooks help you:

- Track the origin of user signups by adding metadata
- Improve security by adding additional checks to password and multi-factor authentication
- Support legacy systems by integrating with identity credentials from external authentication systems
- Add additional custom claims to your JWT
- Send authentication emails or SMS messages through a custom provider

The following hooks are available:

| Hook | Available on Plan |
| --- | --- |
| [Custom Access Token](https://supabase.com/docs/guides/auth/auth-hooks/custom-access-token-hook) | Free, Pro |
| [Send SMS](https://supabase.com/docs/guides/auth/auth-hooks/send-sms-hook) | Free, Pro |
| [Send Email](https://supabase.com/docs/guides/auth/auth-hooks/send-email-hook) | Free, Pro |
| [MFA Verification Attempt](https://supabase.com/docs/guides/auth/auth-hooks/mfa-verification-hook) | Teams and Enterprise |
| [Password Verification Attempt](https://supabase.com/docs/guides/auth/auth-hooks/password-verification-hook) | Teams and Enterprise |

Supabase supports 2 ways to [configure a hook](https://supabase.com/dashboard/project/_/auth/hooks) in your project:

Postgres FunctionHTTP Endpoint

A [Postgres function](https://supabase.com/docs/guides/database/functions) can be configured as a hook. The function should take in a single argument -- the event of type JSONB -- and return a JSONB object. Since the Postgres function runs on your database, the request does not leave your project's instance.

## Security model [\#](https://supabase.com/docs/guides/auth/auth-hooks\#security-model)

Sign the payload and grant permissions selectively in order to guard the integrity of the payload.

SQLHTTP

When you configure a Postgres function as a hook, Supabase will automatically apply the following grants to the function for these reasons:

- Allow the `supabase_auth_admin` role to execute the function. The `supabase_auth_admin` role is the Postgres role that is used by Supabase Auth to make requests to your database.
- Revoke permissions from other roles (e.g. `anon`, `authenticated`, `public`) to ensure the function is not accessible by Supabase Data APIs.

`
-- Grant access to function to supabase_auth_admin
grant execute
on function public.custom_access_token_hook
to supabase_auth_admin;
-- Grant access to schema to supabase_auth_admin
grant usage on schema public to supabase_auth_admin;
-- Revoke function permissions from authenticated, anon and public
revoke execute
on function public.custom_access_token_hook
from authenticated, anon, public;
`

You will need to alter your row-level security (RLS) policies to allow the `supabase_auth_admin` role to access tables that you have RLS policies on. You can read more about RLS policies [here](https://supabase.com/docs/guides/database/postgres/row-level-security).

Alternatively, you can create your Postgres function via the dashboard with the `security definer` tag. The `security definer` tag specifies that the function is to be executed with the privileges of the user that owns it.

Currently, functions created via the dashboard take on the `postgres` role. Read more about the `security definer` tag [in our database guide](https://supabase.com/docs/guides/database/functions#security-definer-vs-invoker)

## Using Hooks [\#](https://supabase.com/docs/guides/auth/auth-hooks\#using-hooks)

### Developing [\#](https://supabase.com/docs/guides/auth/auth-hooks\#developing)

Let us develop a Hook locally and then deploy it to the cloud. As a recap, here’s a list of available Hooks

| Hook | Suggested Function Name | When it is called | What it Does |
| --- | --- | --- | --- |
| Send SMS | `send_sms` | Each time an SMS is sent | Allows you to customize message content and SMS Provider |
| Send Email | `send_email` | Each time an Email is sent | Allows you to customize message content and Email Provider |
| Custom Access Token | `custom_access_token` | Each time a new JWT is created | Returns the claims you wish to be present in the JWT. |
| MFA Verification Attempt | `mfa_verification_attempt` | Each time a user tries to verify an MFA factor. | Returns a decision on whether to reject the attempt and future ones, or to allow the user to keep trying. |
| Password Verification Attempt | `password_verification_attempt` | Each time a user tries to sign in with a password. | Return a decision whether to allow the user to reject the attempt, or to allow the user to keep trying. |

Edit `config.toml` to set up the Auth Hook locally.

SQLHTTP

Modify the `auth.hook.<hook_name>` field and set `uri` to a value of `pg-functions://postgres/<schema>/<function_name>`

`
[auth.hook.<hook_name>]
enabled = true
uri = "pg-functions://...."
`

You need to assign additional permissions so that Supabase Auth can access the hook as well as the tables it interacts with.

The `supabase_auth_admin` role does not have permissions to the `public` schema. You need to grant the role permission to execute your hook:

`
grant execute
on function public.custom_access_token_hook
to supabase_auth_admin;
`

You also need to grant usage to `supabase_auth_admin`:

`
grant usage on schema public to supabase_auth_admin;
`

Also revoke permissions from the `authenticated` and `anon` roles to ensure the function is not accessible by Supabase Serverless APIs.

`
revoke execute
on function public.custom_access_token_hook
from authenticated, anon;
`

For security, we recommend against the use the `security definer` tag. The `security definer` tag specifies that the function is to be executed with the privileges of the user that owns it. When a function is created via the Supabase dashboard with the tag, it will have the extensive permissions of the `postgres` role which make it easier for undesirable actions to occur.

We recommend that you do not use any tag and explicitly grant permissions to `supabase_auth_admin` as described above.

Read more about `security definer` tag [in our database guide](https://supabase.com/docs/guides/database/functions#security-definer-vs-invoker).

There are no restrictions as to what language can be used to write a Postgres Hook. If [PL/pgSQL](https://www.postgresql.org/docs/current/plpgsql.html) is too difficult consider using the [plv8](https://supabase.com/docs/guides/database/extensions/plv8) extension which lets you use JavaScript to define functions.

Once done, save your Auth Hook as a migration in order to version the Auth Hook and share it with other team members. Run [`supabase migration new`](https://supabase.com/docs/reference/cli/supabase-migration-new) to create a migration.

If you're using the Supabase SQL Editor, there's an issue when using the `?` ( _Does the string exist as a top-level key within the JSON value?_) operator. Use a direct connection to the database if you need to use it when defining a function.

Here is an example hook signature:

`
create or replace function public.custom_access_token_hook(event jsonb)
returns jsonb
language plpgsql
as $$
declare
  -- Insert variables here
begin
  -- Insert logic here
return event;
end;
$$;
`

You can visit `SQL Editor > Templates` for hook templates.

### Deploying [\#](https://supabase.com/docs/guides/auth/auth-hooks\#deploying)

In the dashboard, navigate to [`Authentication > Hooks`](https://supabase.com/dashboard/project/_/auth/hooks) and select the appropriate function type (SQL or HTTP) from the dropdown menu.

### Error handling [\#](https://supabase.com/docs/guides/auth/auth-hooks\#error-handling)

You should return an error when facing a runtime error. Runtime errors are specific to your application and arise from specific business rules rather than programmer errors.

Runtime errors could happen when:

- The user does not have appropriate permissions
- The event payload received does not have required claims.
- The user has performed an action which violates a business rule.
- The email or phone provider used in the webhook returned an error.

SQLHTTP

The error is a JSON object and has the following properties:

- `error` An object that contains information about the error.
  - `http_code` A number indicating the HTTP code to be returned. If not set, the code is HTTP 500 Internal Server Error.
  - `message` A message to be returned in the HTTP response. Required.

Here's an example:

`
{
"error": {
    "http_code": 429,
    "message": "You can only verify a factor once every 10 seconds."
}
}
`

Errors returned from a Postgres Hook are not retry-able. When an error is returned, the error is propagated from the hook to Supabase Auth and translated into a HTTP error which is returned to your application. Supabase Auth will only take into account the error and disregard the rest of the payload.

Outside of runtime errors, both HTTP Hooks and Postgres Hooks return timeout errors. Postgres Hooks have 2 seconds to complete processing while HTTP Hooks should complete in 5 seconds. Both HTTP Hooks and Postgres Hooks are run in a transaction do limit the duration of execution to avoid delays in authentication process.

## Available Hooks [\#](https://supabase.com/docs/guides/auth/auth-hooks\#available-hooks)

Each Hook description contains an example JSON Schema which you can use in conjunction with [JSON Schema Faker](https://json-schema-faker.js.org/) in order to generate a mock payload. For HTTP Hooks, you can also use [the Standard Webhooks Testing Tool](https://www.standardwebhooks.com/simulate) to simulate a request.

[Custom Access Token\\
\\
Customize the access token issued by Supabase Auth](https://supabase.com/docs/guides/auth/auth-hooks/custom-access-token-hook)

[Send SMS\\
\\
Use a custom SMS provider to send authentication messages](https://supabase.com/docs/guides/auth/auth-hooks/send-sms-hook)

[Send Email\\
\\
Use a custom email provider to send authentication messages](https://supabase.com/docs/guides/auth/auth-hooks/send-email-hook)

[MFA Verification\\
\\
Add additional checks to the MFA verification flow](https://supabase.com/docs/guides/auth/auth-hooks/mfa-verification-hook)

[Password verification\\
\\
Add additional checks to the password verification flow](https://supabase.com/docs/guides/auth/auth-hooks/password-verification-hook)

### Is this helpful?

NoYes

### On this page

[What is a hook](https://supabase.com/docs/guides/auth/auth-hooks#what-is-a-hook) [Security model](https://supabase.com/docs/guides/auth/auth-hooks#security-model) [Using Hooks](https://supabase.com/docs/guides/auth/auth-hooks#using-hooks) [Developing](https://supabase.com/docs/guides/auth/auth-hooks#developing) [Deploying](https://supabase.com/docs/guides/auth/auth-hooks#deploying) [Error handling](https://supabase.com/docs/guides/auth/auth-hooks#error-handling) [Available Hooks](https://supabase.com/docs/guides/auth/auth-hooks#available-hooks)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings