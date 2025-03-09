Queues

# Quickstart

## Learn how to use Supabase Queues to add and read messages

* * *

This guide is an introduction to interacting with Supabase Queues via the Dashboard and official client library. Check out [Queues API Reference](https://supabase.com/docs/guides/queues/api) for more details on our API.

## Concepts [\#](https://supabase.com/docs/guides/queues/quickstart\#concepts)

Supabase Queues is a pull-based Message Queue consisting of three main components: Queues, Messages, and Queue Types.

### Pull-Based Queue [\#](https://supabase.com/docs/guides/queues/quickstart\#pull-based-queue)

A pull-based Queue is a Message storage and delivery system where consumers actively fetch Messages when they're ready to process them - similar to constantly refreshing a webpage to display the latest updates. Our pull-based Queues process Messages in a First-In-First-Out (FIFO) manner without priority levels.

### Message [\#](https://supabase.com/docs/guides/queues/quickstart\#message)

A Message in a Queue is a JSON object that is stored until a consumer explicitly processes and removes it, like a task waiting in a to-do list until someone checks and completes it.

### Queue types [\#](https://supabase.com/docs/guides/queues/quickstart\#queue-types)

Supabase Queues offers three types of Queues:

- **Basic Queue**: A durable Queue that stores Messages in a logged table.

- **Unlogged Queue**: A transient Queue that stores Messages in an unlogged table for better performance but may result in loss of Queue Messages.

- **Partitioned Queue** ( _Coming Soon_): A durable and scalable Queue that stores Messages in multiple table partitions for better performance.


## Create Queues [\#](https://supabase.com/docs/guides/queues/quickstart\#create-queues)

To get started, navigate to the [Supabase Queues](https://supabase.com/dashboard/project/_/integrations/queues/overview) Postgres Module under Integrations in the Dashboard and enable the `pgmq` extension.

`pgmq` extension is available in Postgres version 15.6.1.143 or later.

![Supabase Dashboard Integrations page, showing the Queues Postgres Module](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fqueues-quickstart-install.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

On the [Queues page](https://supabase.com/dashboard/project/_/integrations/queues/queues):

- Click **Add a new queue** button

If you've already created a Queue click the **Create a queue** button instead.

- Name your queue

Queue names can only be lowercase and hyphens and underscores are permitted.

- Select your [Queue Type](https://supabase.com/docs/guides/queues/quickstart#queue-types)

![Create a Queue from the Supabase Dashboard](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fqueues-quickstart-create.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### What happens when you create a queue? [\#](https://supabase.com/docs/guides/queues/quickstart\#what-happens-when-you-create-a-queue)

Every new Queue creates two tables in the `pgmq` schema. These tables are `pgmq.q_<queue_name>` to store and process active messages and `pgmq.a_<queue_name>` to store any archived messages.

A "Basic Queue" will create `pgmq.q_<queue_name>` and `pgmq.a_<queue_name>` tables as logged tables.

However, an "Unlogged Queue" will create `pgmq.q_<queue_name>` as an unlogged table for better performance while sacrificing durability. The `pgmq.a_<queue_name>` table will still be created as a logged table so your archived messages remain safe and secure.

## Expose Queues to client-side consumers [\#](https://supabase.com/docs/guides/queues/quickstart\#expose-queues-to-client-side-consumers)

Queues, by default, are not exposed over Supabase Data API and are only accessible via Postgres clients.

However, you may grant client-side consumers access to your Queues by enabling the Supabase Data API and granting permissions to the Queues API, which is a collection of database functions in the `pgmq_public` schema that wraps the database functions in the `pgmq` schema.

This is to prevent direct access to the `pgmq` schema and its tables (RLS is not enabled by default on any tables) and database functions.

To get started, navigate to the Queues [Settings page](https://supabase.com/dashboard/project/_/integrations/queues/settings) and toggle on “Expose Queues via PostgREST”. Once enabled, Supabase creates and exposes a `pgmq_public` schema containing database function wrappers to a subset of `pgmq`'s database functions.

![Screenshot of Queues settings with toggle to expose to PostgREST](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fqueues-quickstart-settings.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Enable RLS on your tables in `pgmq` schema [\#](https://supabase.com/docs/guides/queues/quickstart\#enable-rls-on-your-tables-in-pgmq-schema)

For security purposes, you must enable Row Level Security (RLS) on all Queue tables (all tables in `pgmq` schema that begin with `q_`) if the Data API is enabled.

You’ll want to create RLS policies for any Queues you want your client-side consumers to interact with.

![Screenshot of creating an RLS policy from the Queues settings](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fqueues-quickstart-rls.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Grant permissions to `pgmq_public` database functions [\#](https://supabase.com/docs/guides/queues/quickstart\#grant-permissions-to-pgmqpublic-database-functions)

On top of enabling RLS and writing RLS policies on the underlying Queue tables, you must grant the correct permissions to the `pgmq_public` database functions for each Data API role.

The permissions required for each Queue API database function:

| **Operations** | **Permissions Required** |
| --- | --- |
| `send` `send_batch` | `Select` `Insert` |
| `read` `pop` | `Select` `Update` |
| `archive` `delete` | `Select` `Delete` |

To manage your queue permissions, click on the Queue Settings button.

![Screenshot of accessing queue settings](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fqueues-quickstart-queue-settings.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

Then enable the required roles permissions.

![Screenshot of configuring API access for roles from the Queues settings](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fqueues-quickstart-roles-light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

`postgres` and `service_role` roles should never be exposed client-side.

### Enqueueing and dequeueing messages [\#](https://supabase.com/docs/guides/queues/quickstart\#enqueueing-and-dequeueing-messages)

Once your Queue has been created, you can begin enqueueing and dequeueing Messages.

Here's a TypeScript example using the official Supabase client library:

`
import { createClient } from '@supabase/supabase-js'
const supabaseUrl = 'supabaseURL'
const supabaseKey = 'supabaseKey'
const supabase = createClient(supabaseUrl, supabaseKey)
const QueuesTest: React.FC = () => {
//Add a Message
const sendToQueue = async () => {
    const result = await supabase.schema('pgmq_public').rpc('send', {
      queue_name: 'foo',
      message: { hello: 'world' },
      sleep_seconds: 30,
    })
    console.log(result)
}
//Dequeue Message
const popFromQueue = async () => {
    const result = await supabase.schema('pgmq_public').rpc('pop', { queue_name: 'foo' })
    console.log(result)
}
return (
    <div className="p-6">
      <h2 className="text-2xl font-bold mb-4">Queue Test Component</h2>
      <button
        onClick={sendToQueue}
        className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 mr-4"
      >
        Add Message
      </button>
      <button
        onClick={popFromQueue}
        className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
      >
        Pop Message
      </button>
    </div>
)
}
export default QueuesTest
`

### Is this helpful?

NoYes

### On this page

[Concepts](https://supabase.com/docs/guides/queues/quickstart#concepts) [Pull-Based Queue](https://supabase.com/docs/guides/queues/quickstart#pull-based-queue) [Message](https://supabase.com/docs/guides/queues/quickstart#message) [Queue types](https://supabase.com/docs/guides/queues/quickstart#queue-types) [Create Queues](https://supabase.com/docs/guides/queues/quickstart#create-queues) [What happens when you create a queue?](https://supabase.com/docs/guides/queues/quickstart#what-happens-when-you-create-a-queue) [Expose Queues to client-side consumers](https://supabase.com/docs/guides/queues/quickstart#expose-queues-to-client-side-consumers) [Enable RLS on your tables in pgmq schema](https://supabase.com/docs/guides/queues/quickstart#enable-rls-on-your-tables-in-pgmq-schema) [Grant permissions to pgmq\_public database functions](https://supabase.com/docs/guides/queues/quickstart#grant-permissions-to-pgmqpublic-database-functions) [Enqueueing and dequeueing messages](https://supabase.com/docs/guides/queues/quickstart#enqueueing-and-dequeueing-messages)

![Create a Queue from the Supabase Dashboard](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fqueues-quickstart-create.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings