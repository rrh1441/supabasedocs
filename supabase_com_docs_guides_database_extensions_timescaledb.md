Database

# timescaledb: Time-Series data

* * *

[`timescaledb`](https://docs.timescale.com/timescaledb/latest/) is a Postgres extension designed for improved handling of time-series data. It provides a scalable, high-performance solution for storing and querying time-series data on top of a standard Postgres database.

`timescaledb` uses a time-series-aware storage model and indexing techniques to improve performance of Postgres in working with time-series data. The extension divides data into chunks based on time intervals, allowing it to scale efficiently, especially for large data sets. The data is then compressed, optimized for write-heavy workloads, and partitioned for parallel processing. `timescaledb` also includes a set of functions, operators, and indexes that work with time-series data to reduce query times, and make data easier to work with.

Supabase projects come with [TimescaleDB Apache 2 Edition](https://docs.timescale.com/about/latest/timescaledb-editions/#timescaledb-apache-2-edition). Functionality only available under the Community Edition is not available.

## Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/timescaledb\#enable-the-extension)

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Extensions** in the sidebar.
3. Search for `timescaledb` and enable the extension.

## Usage [\#](https://supabase.com/docs/guides/database/extensions/timescaledb\#usage)

To demonstrate how `timescaledb` works, let's consider a simple example where we have a table that stores temperature data from different sensors. We will create a table named "temperatures" and store data for two sensors.

First we create a hypertable, which is a virtual table that is partitioned into chunks based on time intervals. The hypertable acts as a proxy for the actual table and makes it easy to query and manage time-series data.

`
create table temperatures (
time timestamptz not null,
sensor_id int not null,
temperature double precision not null
);
select create_hypertable('temperatures', 'time');
`

Next, we can populate some values

`
insert into temperatures (time, sensor_id, temperature)
values
    ('2023-02-14 09:00:00', 1, 23.5),
    ('2023-02-14 09:00:00', 2, 21.2),
    ('2023-02-14 09:05:00', 1, 24.5),
    ('2023-02-14 09:05:00', 2, 22.3),
    ('2023-02-14 09:10:00', 1, 25.1),
    ('2023-02-14 09:10:00', 2, 23.9),
    ('2023-02-14 09:15:00', 1, 24.9),
    ('2023-02-14 09:15:00', 2, 22.7),
    ('2023-02-14 09:20:00', 1, 24.7),
    ('2023-02-14 09:20:00', 2, 23.5);
`

And finally we can query the table using `timescaledb`'s `time_bucket` function to divide the time-series into intervals of the specified size (in this case, 1 hour) averaging the `temperature` reading within each group.

`
select
    time_bucket('1 hour', time) AS hour,
    avg(temperature) AS average_temperature
from
    temperatures
where
    sensor_id = 1
    and time > NOW() - interval '1 hour'
group by
    hour;
`

## Resources [\#](https://supabase.com/docs/guides/database/extensions/timescaledb\#resources)

- Official [`timescaledb` documentation](https://docs.timescale.com/timescaledb/latest/)

### Is this helpful?

NoYes

### On this page

[Enable the extension](https://supabase.com/docs/guides/database/extensions/timescaledb#enable-the-extension) [Usage](https://supabase.com/docs/guides/database/extensions/timescaledb#usage) [Resources](https://supabase.com/docs/guides/database/extensions/timescaledb#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings