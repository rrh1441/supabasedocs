Cron

# Quickstart

* * *

Job names are case sensitive and cannot be edited once created.

Attempting to create a second Job with the same name (and case) will overwrite the first Job.

## Schedule a job [\#](https://supabase.com/docs/guides/cron/quickstart\#schedule-a-job)

DashboardSQL

1. Go to the [Jobs](https://supabase.com/dashboard/project/_/integrations/cron/jobs) section to schedule your first Job.
2. Click on `Create job` button or navigate to the new Cron Job form [here](https://supabase.com/dashboard/project/_/integrations/cron/jobs?dialog-shown=true).
3. Name your Cron Job.
4. Choose a schedule for your Job by inputting cron syntax (refer to the syntax chart in the form) or natural language.
5. Input SQL snippet or select a Database function, HTTP request, or Supabase Edge Function.

![Cron Create](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fdatabase%2Fcron%2Fcron-create.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

Cron syntax

You can input seconds for your Job schedule interval as long as you're on Postgres version 15.1.1.61 or later.

## Edit a job [\#](https://supabase.com/docs/guides/cron/quickstart\#edit-a-job)

DashboardSQL

1. Go to the [Jobs](https://supabase.com/dashboard/project/_/integrations/cron/jobs) section and find the Job you'd like to edit.
2. Click on the three vertical dots menu on the right side of the Job and click `Edit cron job`.
3. Make your changes and then click `Save cron job`.

![Cron Edit](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fdatabase%2Fcron%2Fcron-edit.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

## Activate/Deactivate a job [\#](https://supabase.com/docs/guides/cron/quickstart\#activatedeactivate-a-job)

DashboardSQL

1. Go to the [Jobs](https://supabase.com/dashboard/project/_/integrations/cron/jobs) section and find the Job you'd like to unschedule.
2. Toggle the `Active`/ `Inactive` switch next to Job name.

![Cron Toggle](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fdatabase%2Fcron%2Fcron-toggle.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

## Unschedule a job [\#](https://supabase.com/docs/guides/cron/quickstart\#unschedule-a-job)

DashboardSQL

1. Go to the [Jobs](https://supabase.com/dashboard/project/_/integrations/cron/jobs) section and find the Job you'd like to delete.
2. Click on the three vertical dots menu on the right side of the Job and click `Delete cron job`.
3. Confirm deletion by entering the Job name.

![Cron Unschedule](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fdatabase%2Fcron%2Fcron-unschedule.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

## Inspecting job runs [\#](https://supabase.com/docs/guides/cron/quickstart\#inspecting-job-runs)

DashboardSQL

1. Go to the [Jobs](https://supabase.com/dashboard/project/_/integrations/cron/jobs) section and find the Job you want to see the runs of.
2. Click on the `History` button next to the Job name.

![Cron Job Runs](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fdatabase%2Fcron%2Fcron-history.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

## Examples [\#](https://supabase.com/docs/guides/cron/quickstart\#examples)

### Delete data every week [\#](https://supabase.com/docs/guides/cron/quickstart\#delete-data-every-week)

Delete old data every Saturday at 3:30AM (GMT):

`
select cron.schedule (
'saturday-cleanup', -- name of the cron job
'30 3 * * 6', -- Saturday at 3:30AM (GMT)
$$ delete from events where event_time < now() - interval '1 week' $$
);
`

### Run a vacuum every day [\#](https://supabase.com/docs/guides/cron/quickstart\#run-a-vacuum-every-day)

Vacuum every day at 3:00AM (GMT):

`
select cron.schedule('nightly-vacuum', '0 3 * * *', 'VACUUM');
`

### Call a database function every 5 minutes [\#](https://supabase.com/docs/guides/cron/quickstart\#call-a-database-function-every-5-minutes)

Create a [`hello_world()`](https://supabase.com/docs/guides/database/functions?language=sql#simple-functions) database function and then call it every 5 minutes:

`
select cron.schedule('call-db-function', '*/5 * * * *', 'SELECT hello_world()');
`

### Call a database stored procedure [\#](https://supabase.com/docs/guides/cron/quickstart\#call-a-database-stored-procedure)

To use a stored procedure, you can call it like this:

`
select cron.schedule('call-db-procedure', '*/5 * * * *', 'CALL my_procedure()');
`

### Invoke Supabase Edge Function every 30 seconds [\#](https://supabase.com/docs/guides/cron/quickstart\#invoke-supabase-edge-function-every-30-seconds)

Make a POST request to a Supabase Edge Function every 30 seconds:

`
select
cron.schedule(
    'invoke-function-every-half-minute',
    '30 seconds',
    $$
    select
      net.http_post(
          url:='https://project-ref.supabase.co/functions/v1/function-name',
          headers:=jsonb_build_object('Content-Type','application/json', 'Authorization', 'Bearer ' || 'YOUR_ANON_KEY'),
          body:=jsonb_build_object('time', now() ),
          timeout_milliseconds:=5000
      ) as request_id;
    $$
);
`

This requires the [`pg_net` extension](https://supabase.com/docs/guides/database/extensions/pg_net) to be enabled.

## Caution: Scheduling system maintenance [\#](https://supabase.com/docs/guides/cron/quickstart\#caution-scheduling-system-maintenance)

Be extremely careful when setting up Jobs for system maintenance tasks as they can have unintended consequences.

For instance, scheduling a command to terminate idle connections with `pg_terminate_backend(pid)` can disrupt critical background processes like nightly backups. Often, there is an existing Postgres setting, such as `idle_session_timeout`, that can perform these common maintenance tasks without the risk.

Reach out to [Supabase Support](https://supabase.com/support) if you're unsure if that applies to your use case.

### Is this helpful?

NoYes

### On this page

[Schedule a job](https://supabase.com/docs/guides/cron/quickstart#schedule-a-job) [Edit a job](https://supabase.com/docs/guides/cron/quickstart#edit-a-job) [Activate/Deactivate a job](https://supabase.com/docs/guides/cron/quickstart#activatedeactivate-a-job) [Unschedule a job](https://supabase.com/docs/guides/cron/quickstart#unschedule-a-job) [Inspecting job runs](https://supabase.com/docs/guides/cron/quickstart#inspecting-job-runs) [Examples](https://supabase.com/docs/guides/cron/quickstart#examples) [Delete data every week](https://supabase.com/docs/guides/cron/quickstart#delete-data-every-week) [Run a vacuum every day](https://supabase.com/docs/guides/cron/quickstart#run-a-vacuum-every-day) [Call a database function every 5 minutes](https://supabase.com/docs/guides/cron/quickstart#call-a-database-function-every-5-minutes) [Call a database stored procedure](https://supabase.com/docs/guides/cron/quickstart#call-a-database-stored-procedure) [Invoke Supabase Edge Function every 30 seconds](https://supabase.com/docs/guides/cron/quickstart#invoke-supabase-edge-function-every-30-seconds) [Caution: Scheduling system maintenance](https://supabase.com/docs/guides/cron/quickstart#caution-scheduling-system-maintenance)

![Cron Create](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fdatabase%2Fcron%2Fcron-create.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Cron Edit](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fdatabase%2Fcron%2Fcron-edit.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Cron Unschedule](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fdatabase%2Fcron%2Fcron-unschedule.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Cron Job Runs](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fdatabase%2Fcron%2Fcron-history.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Cron Toggle](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fdatabase%2Fcron%2Fcron-toggle.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings