Realtime

# Realtime Quotas

* * *

Our cluster supports millions of concurrent connections and message throughput for production workloads.

Upgrade your plan to increase your quotas. Without a spend cap, or on an Enterprise plan, some quotas are still in place to protect budgets. All quotas are configurable per project. [Contact support](https://supabase.com/dashboard/support/new) if you need your quotas increased.

## Quotas by plan [\#](https://supabase.com/docs/guides/realtime/quotas\#quotas-by-plan)

|  | Free | Pro | Pro (no spend cap) | Team | Enterprise |
| --- | --- | --- | --- | --- | --- |
| **Concurrent connections** | 200 | 500 | 10,000 | 10,000 | 10,000+ |
| **Messages per second** | 100 | 500 | 2,500 | 2,500 | 2,500+ |
| **Channel joins per second** | 100 | 500 | 2,500 | 2,500 | 2,500+ |
| **Channels per connection** | 100 | 100 | 100 | 100 | 100+ |
| **Presence keys per object** | 10 | 10 | 10 | 10 | 10+ |
| **Presence messages per second** | 20 | 50 | 1,000 | 1,000 | 1,000+ |
| **Broadcast payload size KB** | 256 | 3,000 | 3,000 | 3,000 | 3,000+ |
| **Postgres change payload size KB ( [**read more**](https://supabase.com/docs/guides/realtime/quotas#postgres-changes-payload-quota))** | 1,024 | 1,024 | 1,024 | 1,024 | 1,024+ |

Beyond the Free and Pro Plan you can customize your quotas by [contacting support](https://supabase.com/dashboard/support/new).

## Quota errors [\#](https://supabase.com/docs/guides/realtime/quotas\#quota-errors)

When you exceed a quota, errors will appear in the backend logs and client-side messages in the WebSocket connection.

- **Logs**: check the [Realtime logs](https://supabase.com/dashboard/project/_/database/realtime-logs) inside your project Dashboard.
- **WebSocket errors**: Use your browser's developer tools to find the WebSocket initiation request and view individual messages.

##### Realtime Inspector

You can use the [Realtime Inspector](https://realtime.supabase.com/inspector/new) to reproduce an error and share those connection details with Supabase support.

Some quotas can cause a Channel join to be refused. Realtime will reply with one of the following WebSocket messages:

### `too_many_channels` [\#](https://supabase.com/docs/guides/realtime/quotas\#toomanychannels)

Too many channels currently joined for a single connection.

### `too_many_connections` [\#](https://supabase.com/docs/guides/realtime/quotas\#toomanyconnections)

Too many total concurrent connections for a project.

### `too_many_joins` [\#](https://supabase.com/docs/guides/realtime/quotas\#toomanyjoins)

Too many Channel joins per second.

### `tenant_events` [\#](https://supabase.com/docs/guides/realtime/quotas\#tenantevents)

Connections will be disconnected if your project is generating too many messages per second. `supabase-js` will reconnect automatically when the message throughput decreases below your plan quota. An `event` is a WebSocket message delivered to, or sent from a client.

## Postgres changes payload quota [\#](https://supabase.com/docs/guides/realtime/quotas\#postgres-changes-payload-quota)

When this quota is reached, the `new` and `old` record payloads only include the fields with a value size of less than or equal to 64 bytes.

### Is this helpful?

NoYes

### On this page

[Quotas by plan](https://supabase.com/docs/guides/realtime/quotas#quotas-by-plan) [Quota errors](https://supabase.com/docs/guides/realtime/quotas#quota-errors) [too\_many\_channels](https://supabase.com/docs/guides/realtime/quotas#toomanychannels) [too\_many\_connections](https://supabase.com/docs/guides/realtime/quotas#toomanyconnections) [too\_many\_joins](https://supabase.com/docs/guides/realtime/quotas#toomanyjoins) [tenant\_events](https://supabase.com/docs/guides/realtime/quotas#tenantevents) [Postgres changes payload quota](https://supabase.com/docs/guides/realtime/quotas#postgres-changes-payload-quota)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings