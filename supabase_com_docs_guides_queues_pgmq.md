Queues

# PGMQ Extension

* * *

pgmq is a lightweight message queue built on Postgres.

## Features [\#](https://supabase.com/docs/guides/queues/pgmq\#features)

- Lightweight - No background worker or external dependencies, just Postgres functions packaged in an extension
- "exactly once" delivery of messages to a consumer within a visibility timeout
- API parity with AWS SQS and RSMQ
- Messages stay in the queue until explicitly removed
- Messages can be archived, instead of deleted, for long-term retention and replayability

## Enable the extension [\#](https://supabase.com/docs/guides/queues/pgmq\#enable-the-extension)

`
create extension pgmq;
`

## Usage [\#](https://supabase.com/docs/guides/queues/pgmq\#get-usage)

### Queue management [\#](https://supabase.com/docs/guides/queues/pgmq\#queue-management)

#### `create` [\#](https://supabase.com/docs/guides/queues/pgmq\#create)

Create a new queue.

`
pgmq.create(queue_name text)
returns void
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| queue\_name | text | The name of the queue |

Example:

`
select from pgmq.create('my_queue');
create
--------
`

#### `create_unlogged` [\#](https://supabase.com/docs/guides/queues/pgmq\#createunlogged)

Creates an unlogged table. This is useful when write throughput is more important than durability.
See Postgres documentation for [unlogged tables](https://www.postgresql.org/docs/current/sql-createtable.html#SQL-CREATETABLE-UNLOGGED) for more information.

`
pgmq.create_unlogged(queue_name text)
returns void
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| queue\_name | text | The name of the queue |

Example:

`
select pgmq.create_unlogged('my_unlogged');
create_unlogged
-----------------
`

* * *

#### `detach_archive` [\#](https://supabase.com/docs/guides/queues/pgmq\#detacharchive)

Drop the queue's archive table as a member of the PGMQ extension. Useful for preventing the queue's archive table from being drop when `drop extension pgmq` is executed.
This does not prevent the further archives() from appending to the archive table.

`
pgmq.detach_archive(queue_name text)
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| queue\_name | text | The name of the queue |

Example:

`
select * from pgmq.detach_archive('my_queue');
detach_archive
----------------
`

* * *

#### `drop_queue` [\#](https://supabase.com/docs/guides/queues/pgmq\#dropqueue)

Deletes a queue and its archive table.

`
pgmq.drop_queue(queue_name text)
returns boolean
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| queue\_name | text | The name of the queue |

Example:

`
select * from pgmq.drop_queue('my_unlogged');
drop_queue
------------
t
`

### Sending messages [\#](https://supabase.com/docs/guides/queues/pgmq\#sending-messages)

#### `send` [\#](https://supabase.com/docs/guides/queues/pgmq\#send)

Send a single message to a queue.

`
pgmq.send(
    queue_name text,
    msg jsonb,
    delay integer default 0
)
returns setof bigint
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| `queue_name` | `text` | The name of the queue |
| `msg` | `jsonb` | The message to send to the queue |
| `delay` | `integer` | Time in seconds before the message becomes visible. Defaults to 0. |

Example:

`
select * from pgmq.send('my_queue', '{"hello": "world"}');
send
------
    4
`

* * *

#### `send_batch` [\#](https://supabase.com/docs/guides/queues/pgmq\#sendbatch)

Send 1 or more messages to a queue.

`
pgmq.send_batch(
    queue_name text,
    msgs jsonb[],
    delay integer default 0
)
returns setof bigint
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| `queue_name` | `text` | The name of the queue |
| `msgs` | `jsonb[]` | Array of messages to send to the queue |
| `delay` | `integer` | Time in seconds before the messages becomes visible. Defaults to 0. |

`
select * from pgmq.send_batch(
    'my_queue',
    array[\
      '{"hello": "world_0"}'::jsonb,\
      '{"hello": "world_1"}'::jsonb\
    ]
);
send_batch
------------
          1
          2
`

* * *

### Reading messages [\#](https://supabase.com/docs/guides/queues/pgmq\#reading-messages)

#### `read` [\#](https://supabase.com/docs/guides/queues/pgmq\#read)

Read 1 or more messages from a queue. The VT specifies the delay in seconds between reading and the message becoming invisible to other consumers.

`
pgmq.read(
    queue_name text,
    vt integer,
    qty integer
)
returns setof pgmq.message_record
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| `queue_name` | `text` | The name of the queue |
| `vt` | `integer` | Time in seconds that the message become invisible after reading |
| `qty` | `integer` | The number of messages to read from the queue. Defaults to 1 |

Example:

`
select * from pgmq.read('my_queue', 10, 2);
msg_id | read_ct |          enqueued_at          |              vt               |       message
--------+---------+-------------------------------+-------------------------------+----------------------
      1 |       1 | 2023-10-28 19:14:47.356595-05 | 2023-10-28 19:17:08.608922-05 | {"hello": "world_0"}
      2 |       1 | 2023-10-28 19:14:47.356595-05 | 2023-10-28 19:17:08.608974-05 | {"hello": "world_1"}
(2 rows)
`

* * *

#### `read_with_poll` [\#](https://supabase.com/docs/guides/queues/pgmq\#readwithpoll)

Same as read(). Also provides convenient long-poll functionality.
When there are no messages in the queue, the function call will wait for `max_poll_seconds` in duration before returning.
If messages reach the queue during that duration, they will be read and returned immediately.

`
pgmq.read_with_poll(
    queue_name text,
    vt integer,
    qty integer,
    max_poll_seconds integer default 5,
    poll_interval_ms integer default 100
)
returns setof pgmq.message_record
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| `queue_name` | `text` | The name of the queue |
| `vt` | `integer` | Time in seconds that the message become invisible after reading. |
| `qty` | `integer` | The number of messages to read from the queue. Defaults to 1. |
| `max_poll_seconds` | `integer` | Time in seconds to wait for new messages to reach the queue. Defaults to 5. |
| `poll_interval_ms` | `integer` | Milliseconds between the internal poll operations. Defaults to 100. |

Example:

`
select * from pgmq.read_with_poll('my_queue', 1, 1, 5, 100);
msg_id | read_ct |          enqueued_at          |              vt               |      message
--------+---------+-------------------------------+-------------------------------+--------------------
      1 |       1 | 2023-10-28 19:09:09.177756-05 | 2023-10-28 19:27:00.337929-05 | {"hello": "world"}
`

* * *

#### `pop` [\#](https://supabase.com/docs/guides/queues/pgmq\#pop)

Reads a single message from a queue and deletes it upon read.

Note: utilization of pop() results in at-most-once delivery semantics if the consuming application does not guarantee processing of the message.

`
pgmq.pop(queue_name text)
returns setof pgmq.message_record
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| queue\_name | text | The name of the queue |

Example:

`
pgmq=# select * from pgmq.pop('my_queue');
msg_id | read_ct |          enqueued_at          |              vt               |      message
--------+---------+-------------------------------+-------------------------------+--------------------
      1 |       2 | 2023-10-28 19:09:09.177756-05 | 2023-10-28 19:27:00.337929-05 | {"hello": "world"}
`

* * *

### Deleting/Archiving messages [\#](https://supabase.com/docs/guides/queues/pgmq\#deletingarchiving-messages)

#### `delete` (single) [\#](https://supabase.com/docs/guides/queues/pgmq\#delete-single)

Deletes a single message from a queue.

`
pgmq.delete (queue_name text, msg_id: bigint)
returns boolean
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| `queue_name` | `text` | The name of the queue |
| `msg_id` | `bigint` | Message ID of the message to delete |

Example:

`
select pgmq.delete('my_queue', 5);
delete
--------
t
`

* * *

#### `delete` (batch) [\#](https://supabase.com/docs/guides/queues/pgmq\#delete-batch)

Delete one or many messages from a queue.

`
pgmq.delete (queue_name text, msg_ids: bigint[])
returns setof bigint
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| `queue_name` | `text` | The name of the queue |
| `msg_ids` | `bigint[]` | Array of message IDs to delete |

Examples:

Delete two messages that exist.

`
select * from pgmq.delete('my_queue', array[2, 3]);
delete
--------
      2
      3
`

Delete two messages, one that exists and one that does not. Message `999` does not exist.

`
select * from pgmq.delete('my_queue', array[6, 999]);
delete
--------
      6
`

* * *

#### `purge_queue` [\#](https://supabase.com/docs/guides/queues/pgmq\#purgequeue)

Permanently deletes all messages in a queue. Returns the number of messages that were deleted.

`
purge_queue(queue_name text)
returns bigint
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| queue\_name | text | The name of the queue |

Example:

Purge the queue when it contains 8 messages;

`
select * from pgmq.purge_queue('my_queue');
purge_queue
-------------
           8
`

* * *

#### `archive` (single) [\#](https://supabase.com/docs/guides/queues/pgmq\#archive-single)

Removes a single requested message from the specified queue and inserts it into the queue's archive.

`
pgmq.archive(queue_name text, msg_id bigint)
returns boolean
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| `queue_name` | `text` | The name of the queue |
| `msg_id` | `bigint` | Message ID of the message to archive |

Returns
Boolean value indicating success or failure of the operation.

Example; remove message with ID 1 from queue `my_queue` and archive it:

`
select * from pgmq.archive('my_queue', 1);
archive
---------
       t
`

* * *

#### `archive` (batch) [\#](https://supabase.com/docs/guides/queues/pgmq\#archive-batch)

Deletes a batch of requested messages from the specified queue and inserts them into the queue's archive.
Returns an array of message ids that were successfully archived.

`
pgmq.archive(queue_name text, msg_ids bigint[])
RETURNS SETOF bigint
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| `queue_name` | `text` | The name of the queue |
| `msg_ids` | `bigint[]` | Array of message IDs to archive |

Examples:

Delete messages with ID 1 and 2 from queue `my_queue` and move to the archive.

`
select * from pgmq.archive('my_queue', array[1, 2]);
archive
---------
       1
       2
`

Delete messages 4, which exists and 999, which does not exist.

`
select * from pgmq.archive('my_queue', array[4, 999]);
archive
---------
       4
`

* * *

### Utilities [\#](https://supabase.com/docs/guides/queues/pgmq\#utilities)

#### `set_vt` [\#](https://supabase.com/docs/guides/queues/pgmq\#setvt)

Sets the visibility timeout of a message to a specified time duration in the future. Returns the record of the message that was updated.

`
pgmq.set_vt(
    queue_name text,
    msg_id bigint,
    vt_offset integer
)
returns pgmq.message_record
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| `queue_name` | `text` | The name of the queue |
| `msg_id` | `bigint` | ID of the message to set visibility time |
| `vt_offset` | `integer` | Duration from now, in seconds, that the message's VT should be set to |

Example:

Set the visibility timeout of message 1 to 30 seconds from now.

`
select * from pgmq.set_vt('my_queue', 11, 30);
msg_id | read_ct |          enqueued_at          |              vt               |       message
--------+---------+-------------------------------+-------------------------------+----------------------
     1 |       0 | 2023-10-28 19:42:21.778741-05 | 2023-10-28 19:59:34.286462-05 | {"hello": "world_0"}
`

* * *

#### `list_queues` [\#](https://supabase.com/docs/guides/queues/pgmq\#listqueues)

List all the queues that currently exist.

`
list_queues()
RETURNS TABLE(
    queue_name text,
    created_at timestamp with time zone,
    is_partitioned boolean,
    is_unlogged boolean
)
`

Example:

`
select * from pgmq.list_queues();
      queue_name      |          created_at           | is_partitioned | is_unlogged
----------------------+-------------------------------+----------------+-------------
my_queue             | 2023-10-28 14:13:17.092576-05 | f              | f
my_partitioned_queue | 2023-10-28 19:47:37.098692-05 | t              | f
my_unlogged          | 2023-10-28 20:02:30.976109-05 | f              | t
`

* * *

#### `metrics` [\#](https://supabase.com/docs/guides/queues/pgmq\#metrics)

Get metrics for a specific queue.

`
pgmq.metrics(queue_name: text)
returns table(
    queue_name text,
    queue_length bigint,
    newest_msg_age_sec integer,
    oldest_msg_age_sec integer,
    total_messages bigint,
    scrape_time timestamp with time zone
)
`

**Parameters:**

| Parameter | Type | Description |
| :-- | :-- | :-- |
| queue\_name | text | The name of the queue |

**Returns:**

\| Attribute \| Type \| Description \|
\| :\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| :\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| :\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| `queue_name` \| `text` \| The name of the queue \|
\| `queue_length` \| `bigint` \| Number of messages currently in the queue \|
\| `newest_msg_age_sec` \| `integer                   | null` \| Age of the newest message in the queue, in seconds \|
\| `oldest_msg_age_sec` \| `integer                   | null` \| Age of the oldest message in the queue, in seconds \|
\| `total_messages` \| `bigint` \| Total number of messages that have passed through the queue over all time \|
\| `scrape_time` \| `timestamp with time zone` \| The current timestamp \|

Example:

`
select * from pgmq.metrics('my_queue');
queue_name | queue_length | newest_msg_age_sec | oldest_msg_age_sec | total_messages |          scrape_time
------------+--------------+--------------------+--------------------+----------------+-------------------------------
my_queue   |           16 |               2445 |               2447 |             35 | 2023-10-28 20:23:08.406259-05
`

* * *

#### `metrics_all` [\#](https://supabase.com/docs/guides/queues/pgmq\#metricsall)

Get metrics for all existing queues.

`
pgmq.metrics_all()
RETURNS TABLE(
    queue_name text,
    queue_length bigint,
    newest_msg_age_sec integer,
    oldest_msg_age_sec integer,
    total_messages bigint,
    scrape_time timestamp with time zone
)
`

**Returns:**

\| Attribute \| Type \| Description \|
\| :\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| :\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| :\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| `queue_name` \| `text` \| The name of the queue \|
\| `queue_length` \| `bigint` \| Number of messages currently in the queue \|
\| `newest_msg_age_sec` \| `integer                   | null` \| Age of the newest message in the queue, in seconds \|
\| `oldest_msg_age_sec` \| `integer                   | null` \| Age of the oldest message in the queue, in seconds \|
\| `total_messages` \| `bigint` \| Total number of messages that have passed through the queue over all time \|
\| `scrape_time` \| `timestamp with time zone` \| The current timestamp \|

`
select * from pgmq.metrics_all();
      queue_name      | queue_length | newest_msg_age_sec | oldest_msg_age_sec | total_messages |          scrape_time
----------------------+--------------+--------------------+--------------------+----------------+-------------------------------
my_queue             |           16 |               2563 |               2565 |             35 | 2023-10-28 20:25:07.016413-05
my_partitioned_queue |            1 |                 11 |                 11 |              1 | 2023-10-28 20:25:07.016413-05
my_unlogged          |            1 |                  3 |                  3 |              1 | 2023-10-28 20:25:07.016413-05
`

### Types [\#](https://supabase.com/docs/guides/queues/pgmq\#types)

#### `message_record` [\#](https://supabase.com/docs/guides/queues/pgmq\#messagerecord)

The complete representation of a message in a queue.

| Attribute Name | Type | Description |
| :-- | :-- | :-- |
| `msg_id` | `bigint` | Unique ID of the message |
| `read_ct` | `bigint` | Number of times the message has been read. Increments on read(). |
| `enqueued_at` | `timestamp with time zone` | time that the message was inserted into the queue |
| `vt` | `timestamp with time zone` | Timestamp when the message will become available for consumers to read |
| `message` | `jsonb` | The message payload |

Example:

`
msg_id | read_ct |          enqueued_at          |              vt               |      message
--------+---------+-------------------------------+-------------------------------+--------------------
      1 |       1 | 2023-10-28 19:06:19.941509-05 | 2023-10-28 19:06:27.419392-05 | {"hello": "world"}
`

## Resources [\#](https://supabase.com/docs/guides/queues/pgmq\#resources)

- Official Docs: [pgmq/api](https://tembo.io/pgmq/#creating-a-queue)

### Is this helpful?

NoYes

### On this page

[Features](https://supabase.com/docs/guides/queues/pgmq#features) [Enable the extension](https://supabase.com/docs/guides/queues/pgmq#enable-the-extension) [Usage](https://supabase.com/docs/guides/queues/pgmq#get-usage) [Queue management](https://supabase.com/docs/guides/queues/pgmq#queue-management) [Sending messages](https://supabase.com/docs/guides/queues/pgmq#sending-messages) [Reading messages](https://supabase.com/docs/guides/queues/pgmq#reading-messages) [Deleting/Archiving messages](https://supabase.com/docs/guides/queues/pgmq#deletingarchiving-messages) [Utilities](https://supabase.com/docs/guides/queues/pgmq#utilities) [Types](https://supabase.com/docs/guides/queues/pgmq#types) [Resources](https://supabase.com/docs/guides/queues/pgmq#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings