Auth

# Password Verification Hook

* * *

Your company wishes to increase security beyond the requirements of the default password implementation in order to fulfill security or compliance requirements. You plan to track the status of a password sign-in attempt and take action via an email or a restriction on logins where necessary.

As this hook runs on unauthenticated requests, malicious users can abuse the hook by calling it multiple times. Pay extra care when using the hook as you can unintentionally block legitimate users from accessing your application.

Check if a password is valid prior to taking any additional action to ensure the user is legitimate. Where possible, send an email or notification instead of blocking the user.

**Inputs**

| Field | Type | Description |
| --- | --- | --- |
| `user_id` | `string` | Unique identifier for the user attempting to sign in. Correlate this to the `auth.users` table. |
| `valid` | `boolean` | Whether the password verification attempt was valid. |

JSONJSON Schema

`
{
"user_id": "3919cb6e-4215-4478-a960-6d3454326cec",
"valid": true
}
`

**Outputs**

Return these only if your hook processed the input without errors.

| Field | Type | Description |
| --- | --- | --- |
| `decision` | `string` | The decision on whether to allow authentication to move forward. Use `reject` to deny the verification attempt and log the user out of all active sessions. Use `continue` to use the default Supabase Auth behavior. |
| `message` | `string` | The message to show the user if the decision was `reject`. |
| `should_logout_user` | `boolean` | Whether to log out the user if a `reject` decision is issued. Has no effect when a `continue` decision is issued. |

`
{
"decision": "reject",
"message": "You have exceeded maximum number of password sign-in attempts.",
"should_logout_user": "false"
}
`

SQL

Limit failed password verification attemptsSend email notification on failed password attempts

As part of new security measures within the company, users can only input an incorrect password every 10 seconds and not more than that. You want to write a hook to enforce this.

Create a table to record each user's last incorrect password verification attempt.

`
create table public.password_failed_verification_attempts (
user_id uuid not null,
last_failed_at timestamp not null default now(),
primary key (user_id)
);
`

Create a hook to read and write information to this table. For example:

`
create function public.hook_password_verification_attempt(event jsonb)
returns jsonb
language plpgsql
as $$
declare
    last_failed_at timestamp;
begin
    if event->'valid' is true then
      -- password is valid, accept it
      return jsonb_build_object('decision', 'continue');
    end if;
    select last_failed_at into last_failed_at
      from public.password_failed_verification_attempts
      where
        user_id = event->'user_id';
    if last_failed_at is not null and now() - last_failed_at < interval '10 seconds' then
      -- last attempt was done too quickly
      return jsonb_build_object(
        'error', jsonb_build_object(
          'http_code', 429,
          'message',   'Please wait a moment before trying again.'
        )
      );
    end if;
    -- record this failed attempt
    insert into public.password_failed_verification_attempts
      (
        user_id,
        last_failed_at
      )
      values
      (
        event->'user_id',
        now()
      )
      on conflict do update
        set last_failed_at = now();
    -- finally let Supabase Auth do the default behavior for a failed attempt
    return jsonb_build_object('decision', 'continue');
end;
$$;
-- Assign appropriate permissions
grant all
on table public.password_failed_verification_attempts
to supabase_auth_admin;
revoke all
on table public.password_failed_verification_attempts
from authenticated, anon, public;
`

### Is this helpful?

NoYes

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings