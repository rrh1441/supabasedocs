Storage

# Bandwidth & Storage Egress

## Bandwidth & Storage Egress

* * *

## Bandwidth & Storage egress [\#](https://supabase.com/docs/guides/storage/serving/bandwidth\#bandwidth--storage-egress)

Free Plan Organizations in Supabase have a limit of 5 GB of bandwidth. This limit is calculated by the sum of all the data transferred from the Supabase servers to the client. This includes all the data transferred from the database, storage, and functions.

### Checking Storage egress requests in Logs Explorer: [\#](https://supabase.com/docs/guides/storage/serving/bandwidth\#checking-storage-egress-requests-in-logs-explorer)

We have a template query that you can use to get the number of requests for each object in [Logs Explorer](https://supabase.com/dashboard/project/_/logs/explorer/templates).

`
select
r.method as http_verb,
r.path as filepath,
count(*) as num_requests
from
edge_logs
cross join unnest(metadata) as m
cross join unnest(m.request) as r
cross join unnest(r.headers) as h
where (path like '%storage/v1/object/%' or path like '%storage/v1/render/%') and r.method = 'GET'
group by r.path, r.method
order by num_requests desc
limit 100;
`

Example of the output:

`
[\
    {"filepath":"/storage/v1/object/sign/large%20bucket/20230902_200037.gif",\
    "http_verb":"GET",\
    "num_requests":100\
    },\
    {"filepath":"/storage/v1/object/public/demob/Sports/volleyball.png",\
    "http_verb":"GET",\
    "num_requests":168\
    }\
]
`

### Calculating egress: [\#](https://supabase.com/docs/guides/storage/serving/bandwidth\#calculating-egress)

If you already know the size of those files, you can calculate the egress by multiplying the number of requests by the size of the file.
You can also get the size of the file with the following cURL:

`
curl -s -w "%{size_download}\n" -o /dev/null "https://my_project.supabase.co/storage/v1/object/large%20bucket/20230902_200037.gif"
`

This will return the size of the file in bytes.
For this example, let's say that `20230902_200037.gif` has a file size of 3 megabytes and `volleyball.png` has a file size of 570 kilobytes.

Now, we have to sum all the egress for all the files to get the total egress:

`
100 * 3MB = 300MB
168 * 570KB = 95.76MB
Total Egress = 395.76MB
`

You can see that these values can get quite large, so it's important to keep track of the egress and optimize the files.

### Optimizing egress: [\#](https://supabase.com/docs/guides/storage/serving/bandwidth\#optimizing-egress)

If you are on the Pro Plan, you can use the [Supabase Image Transformations](https://supabase.com/docs/guides/storage/image-transformations) to optimize the images and reduce the egress.

### Is this helpful?

NoYes

### On this page

[Bandwidth & Storage egress](https://supabase.com/docs/guides/storage/serving/bandwidth#bandwidth--storage-egress) [Checking Storage egress requests in Logs Explorer:](https://supabase.com/docs/guides/storage/serving/bandwidth#checking-storage-egress-requests-in-logs-explorer) [Calculating egress:](https://supabase.com/docs/guides/storage/serving/bandwidth#calculating-egress) [Optimizing egress:](https://supabase.com/docs/guides/storage/serving/bandwidth#optimizing-egress)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings