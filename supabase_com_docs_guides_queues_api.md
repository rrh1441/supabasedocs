Queues

# API

* * *

When you create a Queue in Supabase, you can choose to create helper database functions in the `pgmq_public` schema. This schema exposes operations to manage Queue Messages to consumers client-side, but does not expose functions for creating or dropping Queues.

Database functions in `pgmq_public` can be exposed via Supabase Data API so consumers client-side can call them. Visit the [Quickstart](https://supabase.com/docs/guides/queues/quickstart) for an example.

### `pgmq_public.pop(queue_name)` [\#](https://supabase.com/docs/guides/queues/api\#pgmqpublicpopqueuename)

Retrieves the next available message and deletes it from the specified Queue.

- `queue_name` ( `text`): Queue name

* * *

### `pgmq_public.send(queue_name, message, sleep_seconds)` [\#](https://supabase.com/docs/guides/queues/api\#pgmqpublicsendqueuename-message-sleepseconds)

Adds a Message to the specified Queue, optionally delaying its visibility to all consumers by a number of seconds.

- `queue_name` ( `text`): Queue name
- `message` ( `jsonb`): Message payload to send
- `sleep_seconds` ( `integer`, optional): Delay message visibility by specified seconds. Defaults to 0

* * *

### `pgmq_public.send_batch(queue_name, messages, sleep_seconds)` [\#](https://supabase.com/docs/guides/queues/api\#pgmqpublicsendbatchqueuename-messages-sleepseconds)

Adds a batch of Messages to the specified Queue, optionally delaying their availability to all consumers by a number of seconds.

- `queue_name` ( `text`): Queue name
- `messages` ( `jsonb[]`): Array of message payloads to send
- `sleep_seconds` ( `integer`, optional): Delay messages visibility by specified seconds. Defaults to 0

* * *

### `pgmq_public.archive(queue_name, message_id)` [\#](https://supabase.com/docs/guides/queues/api\#pgmqpublicarchivequeuename-messageid)

Archives a Message by moving it from the Queue table to the Queue's archive table.

- `queue_name` ( `text`): Queue name
- `message_id` ( `bigint`): ID of the Message to archive

* * *

### `pgmq_public.delete(queue_name, message_id)` [\#](https://supabase.com/docs/guides/queues/api\#pgmqpublicdeletequeuename-messageid)

Permanently deletes a Message from the specified Queue.

- `queue_name` ( `text`): Queue name
- `message_id` ( `bigint`): ID of the Message to delete

* * *

### `pgmq_public.read(queue_name, sleep_seconds, n)` [\#](https://supabase.com/docs/guides/queues/api\#pgmqpublicreadqueuename-sleepseconds-n)

Reads up to "n" Messages from the specified Queue with an optional "sleep\_seconds" (visibility timeout).

- `queue_name` ( `text`): Queue name
- `sleep_seconds` ( `integer`): Visibility timeout in seconds
- `n` ( `integer`): Maximum number of Messages to read

### Is this helpful?

NoYes

### On this page

[pgmq\_public.pop(queue\_name)](https://supabase.com/docs/guides/queues/api#pgmqpublicpopqueuename) [pgmq\_public.send(queue\_name, message, sleep\_seconds)](https://supabase.com/docs/guides/queues/api#pgmqpublicsendqueuename-message-sleepseconds) [pgmq\_public.send\_batch(queue\_name, messages, sleep\_seconds)](https://supabase.com/docs/guides/queues/api#pgmqpublicsendbatchqueuename-messages-sleepseconds) [pgmq\_public.archive(queue\_name, message\_id)](https://supabase.com/docs/guides/queues/api#pgmqpublicarchivequeuename-messageid) [pgmq\_public.delete(queue\_name, message\_id)](https://supabase.com/docs/guides/queues/api#pgmqpublicdeletequeuename-messageid) [pgmq\_public.read(queue\_name, sleep\_seconds, n)](https://supabase.com/docs/guides/queues/api#pgmqpublicreadqueuename-sleepseconds-n)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings