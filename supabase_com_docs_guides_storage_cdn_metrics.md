Storage

# Cache Metrics

* * *

Cache hits can be determined via the `metadata.response.headers.cf_cache_status` key in our [Logs Explorer](https://supabase.com/docs/guides/platform/logs#logs-explorer). Any value that corresponds to either `HIT`, `STALE`, `REVALIDATED`, or `UPDATING` is categorized as a cache hit.
The following example query will show the top cache misses from the `edge_logs`:

`
select
r.path as path,
r.search as search,
count(id) as count
from
edge_logs as f
cross join unnest(f.metadata) as m
cross join unnest(m.request) as r
cross join unnest(m.response) as res
cross join unnest(res.headers) as h
where
starts_with(r.path, '/storage/v1/object')
and r.method = 'GET'
and h.cf_cache_status in ('MISS', 'NONE/UNKNOWN', 'EXPIRED', 'BYPASS', 'DYNAMIC')
group by path, search
order by count desc
limit 50;
`

Try out [this query](https://supabase.com/dashboard/project/_/logs/explorer?q=%0Aselect%0A++r.path+as+path%2C%0A++r.search+as+search%2C%0A++count%28id%29+as+count%0Afrom%0A++edge_logs+as+f%0A++cross+join+unnest%28f.metadata%29+as+m%0A++cross+join+unnest%28m.request%29+as+r%0A++cross+join+unnest%28m.response%29+as+res%0A++cross+join+unnest%28res.headers%29+as+h%0Awhere%0A++starts_with%28r.path%2C+%27%2Fstorage%2Fv1%2Fobject%27%29%0A++and+r.method+%3D+%27GET%27%0A++and+h.cf_cache_status+in+%28%27MISS%27%2C+%27NONE%2FUNKNOWN%27%2C+%27EXPIRED%27%2C+%27BYPASS%27%2C+%27DYNAMIC%27%29%0Agroup+by+path%2C+search%0Aorder+by+count+desc%0Alimit+50%3B) in the Logs Explorer.

Your cache hit ratio over time can then be determined using the following query:

`
select
timestamp_trunc(timestamp, hour) as timestamp,
countif(h.cf_cache_status in ('HIT', 'STALE', 'REVALIDATED', 'UPDATING')) / count(f.id) as ratio
from
edge_logs as f
cross join unnest(f.metadata) as m
cross join unnest(m.request) as r
cross join unnest(m.response) as res
cross join unnest(res.headers) as h
where starts_with(r.path, '/storage/v1/object') and r.method = 'GET'
group by timestamp
order by timestamp desc;
`

Try out [this query](https://supabase.com/dashboard/project/_/logs/explorer?q=%0Aselect%0A++timestamp_trunc%28timestamp%2C+hour%29+as+timestamp%2C%0A++countif%28h.cf_cache_status+in+%28%27HIT%27%2C+%27STALE%27%2C+%27REVALIDATED%27%2C+%27UPDATING%27%29%29+%2F+count%28f.id%29+as+ratio%0Afrom%0A++edge_logs+as+f%0A++cross+join+unnest%28f.metadata%29+as+m%0A++cross+join+unnest%28m.request%29+as+r%0A++cross+join+unnest%28m.response%29+as+res%0A++cross+join+unnest%28res.headers%29+as+h%0Awhere+starts_with%28r.path%2C+%27%2Fstorage%2Fv1%2Fobject%27%29+and+r.method+%3D+%27GET%27%0Agroup+by+timestamp%0Aorder+by+timestamp+desc%3B) in the Logs Explorer.

### Is this helpful?

NoYes

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings