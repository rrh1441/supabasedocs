Auth

# Send SMS Hook

## Use a custom SMS provider to send authentication messages

* * *

Runs before a message is sent. Use the hook to:

- Use a regional SMS Provider
- Use alternate messaging channels such as WhatsApp
- Adjust the message body to include platform specific fields such as the [`AppHash`](https://developers.google.com/identity/sms-retriever/overview)

**Inputs**

| Field | Type | Description |
| --- | --- | --- |
| `user` | [`User`](https://supabase.com/docs/guides/auth/users#the-user-object) | The user attempting to sign in. |
| `sms` | `object` | Metadata specific to the SMS sending process. Includes the OTP. |

JSONJSON Schema

`
{
"user": {
    "id": "6481a5c1-3d37-4a56-9f6a-bee08c554965",
    "aud": "authenticated",
    "role": "authenticated",
    "email": "",
    "phone": "+1333363128",
    "phone_confirmed_at": "2024-05-13T11:52:48.157306Z",
    "confirmation_sent_at": "2024-05-14T12:31:52.824573Z",
    "confirmed_at": "2024-05-13T11:52:48.157306Z",
    "phone_change_sent_at": "2024-05-13T11:47:02.183064Z",
    "last_sign_in_at": "2024-05-13T11:52:48.162518Z",
    "app_metadata": {
      "provider": "phone",
      "providers": ["phone"]
    },
    "user_metadata": {},
    "identities": [\
      {\
        "identity_id": "3be5e552-65aa-41d9-9db9-2a502f845459",\
        "id": "6481a5c1-3d37-4a56-9f6a-bee08c554965",\
        "user_id": "6481a5c1-3d37-4a56-9f6a-bee08c554965",\
        "identity_data": {\
          "email_verified": false,\
          "phone": "+1612341244428",\
          "phone_verified": true,\
          "sub": "6481a5c1-3d37-4a56-9f6a-bee08c554965"\
        },\
        "provider": "phone",\
        "last_sign_in_at": "2024-05-13T11:52:48.155562Z",\
        "created_at": "2024-05-13T11:52:48.155599Z",\
        "updated_at": "2024-05-13T11:52:48.159391Z"\
      }\
    ],
    "created_at": "2024-05-13T11:45:33.7738Z",
    "updated_at": "2024-05-14T12:31:52.82475Z",
    "is_anonymous": false
},
"sms": {
    "otp": "561166"
}
}
`

**Outputs**

- No outputs are required. An empty response with a status code of 200 is taken as a successful response.

SQLHTTP

Queue SMS Messages

Your company uses a worker to manage all messaging related jobs. For performance reasons, the messaging system sends messages in intervals via a job queue. Instead of sending a message immediately, messages are queued and sent in periodic intervals via `pg_cron`.

Create a table to store jobs

`
create table job_queue (
job_id uuid primary key default gen_random_uuid(),
job_data jsonb not null,
created_at timestamp default now(),
status text default 'pending',
priority int default 0,
retry_count int default 0,
max_retries int default 2,
scheduled_at timestamp default now()
);
`

Create the hook:

`
create or replace function send_sms(event jsonb) returns void as $$
declare
    job_data jsonb;
    scheduled_time timestamp;
    priority int;
begin
    -- extract phone and otp from the event json
    job_data := jsonb_build_object(
        'phone', event->'user'->>'phone',
        'otp', event->'sms'->>'otp'
    );
    -- calculate the nearest 5-minute window for scheduled_time
    scheduled_time := date_trunc('minute', now()) + interval '5 minute' * floor(extract('epoch' from (now() - date_trunc('minute', now())) / 60) / 5);
    -- assign priority dynamically (example logic: higher priority for earlier scheduled time)
    priority := extract('epoch' from (scheduled_time - now()))::int;
    -- insert the job into the job_queue table
    insert into job_queue (job_data, priority, scheduled_at, max_retries)
    values (job_data, priority, scheduled_time, 2);
end;
$$ language plpgsql;
grant all
on table public.job_queue
to supabase_auth_admin;
revoke all
on table public.job_queue
from authenticated, anon;
`

Create a function to periodically run and dequeue all jobs

`
create or replace function dequeue_and_run_jobs() returns void as $$
declare
    job record;
begin
    for job in
        select * from job_queue
        where status = 'pending'
          and scheduled_at <= now()
        order by priority desc, created_at
        for update skip locked
    loop
        begin
            -- add job processing logic here.
            -- for demonstration, we'll just update the job status to 'completed'.
            update job_queue
            set status = 'completed'
            where job_id = job.job_id;
        exception when others then
            -- handle job failure and retry logic
            if job.retry_count < job.max_retries then
                update job_queue
                set retry_count = retry_count + 1,
                    scheduled_at = now() + interval '1 minute'  -- delay retry by 1 minute
                where job_id = job.job_id;
            else
                update job_queue
                set status = 'failed'
                where job_id = job.job_id;
            end if;
        end;
    end loop;
end;
$$ language plpgsql;
grant execute
on function public.dequeue_and_run_jobs
to supabase_auth_admin;
revoke execute
on function public.dequeue_and_run_jobs
from authenticated, anon;
`

Configure `pg_cron` to run the job on an interval. You can use a tool like [crontab.guru](https://crontab.guru/) to check that your job is running on an appropriate schedule. Ensure that `pg_cron` is enabled under `Database > Extensions`

`
select
cron.schedule(
    '* * * * *', -- this cron expression means every minute.
    'select dequeue_and_run_jobs();'
);
`

### Is this helpful?

NoYes

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings