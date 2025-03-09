Edge Functions

# Scheduling Edge Functions

* * *

Scheduling Edge Functions - YouTube

Supabase

45.5K subscribers

[Scheduling Edge Functions](https://www.youtube.com/watch?v=-U6DJcjVvGo)

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

[Watch on YouTube](https://www.youtube.com/watch?v=-U6DJcjVvGo "Watch on YouTube")

The hosted Supabase Platform supports the [`pg_cron` extension](https://supabase.com/docs/guides/database/extensions/pgcron), a recurring job scheduler in Postgres.

In combination with the [`pg_net` extension](https://supabase.com/docs/guides/database/extensions/pgnet), this allows us to invoke Edge Functions periodically on a set schedule.

To access the auth token securely for your Edge Function call, we recommend storing them in [Supabase Vault](https://supabase.com/docs/guides/database/vault).

## Examples [\#](https://supabase.com/docs/guides/functions/schedule-functions\#examples)

### Invoke an Edge Function every minute [\#](https://supabase.com/docs/guides/functions/schedule-functions\#invoke-an-edge-function-every-minute)

Store `project_url` and `anon_key` in Supabase Vault:

`
select vault.create_secret('https://project-ref.supabase.co', 'project_url');
select vault.create_secret('YOUR_SUPABASE_ANON_KEY', 'anon_key');
`

Make a POST request to a Supabase Edge Function every minute:

`
select
cron.schedule(
    'invoke-function-every-minute',
    '* * * * *', -- every minute
    $$
    select
      net.http_post(
          url:= (select decrypted_secret from vault.decrypted_secrets where name = 'project_url') || '/functions/v1/function-name',
          headers:=jsonb_build_object(
            'Content-type', 'application/json',
            'Authorization', 'Bearer ' || (select decrypted_secret from vault.decrypted_secrets where name = 'anon_key')
          ),
          body:=concat('{"time": "', now(), '"}')::jsonb
      ) as request_id;
    $$
);
`

## Resources [\#](https://supabase.com/docs/guides/functions/schedule-functions\#resources)

- [`pg_net` extension](https://supabase.com/docs/guides/database/extensions/pgnet)
- [`pg_cron` extension](https://supabase.com/docs/guides/database/extensions/pgcron)

### Is this helpful?

NoYes

### On this page

[Examples](https://supabase.com/docs/guides/functions/schedule-functions#examples) [Invoke an Edge Function every minute](https://supabase.com/docs/guides/functions/schedule-functions#invoke-an-edge-function-every-minute) [Resources](https://supabase.com/docs/guides/functions/schedule-functions#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings