# How to check if my queries are being blocked by other queries?

Last edited: 1/17/2025

* * *

## You can set a lock monitor view to help investigate these. [\#](https://supabase.com/docs/guides/troubleshooting/how-to-check-if-my-queries-are-being-blocked-by-other-queries-NSKtR1\#you-can-set-a-lock-monitor-view-to-help-investigate-these)

Once you run the query that takes a long time to complete, you can go in the dashboard (or select from this view below) to check what are the blocks.

`
create view
public.lock_monitor as
select
coalesce(
    blockingl.relation::regclass::text,
    blockingl.locktype
) as locked_item,
now() - blockeda.query_start as waiting_duration,
blockeda.pid as blocked_pid,
blockeda.query as blocked_query,
blockedl.mode as blocked_mode,
blockinga.pid as blocking_pid,
blockinga.query as blocking_query,
blockingl.mode as blocking_mode
from
pg_locks blockedl
join pg_stat_activity blockeda on blockedl.pid = blockeda.pid
join pg_locks blockingl on (
    blockingl.transactionid = blockedl.transactionid
    or blockingl.relation = blockedl.relation
    and blockingl.locktype = blockedl.locktype
)
and blockedl.pid <> blockingl.pid
join pg_stat_activity blockinga on blockingl.pid = blockinga.pid
and blockinga.datid = blockeda.datid
where
not blockedl.granted
and blockinga.datname = current_database();
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings