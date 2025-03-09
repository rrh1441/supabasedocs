Edge Functions

# Background Tasks

## How to run background tasks in an Edge Function outside of the request handler

* * *

Edge Function instances can process background tasks outside of the request handler. Background tasks are useful for asynchronous operations like uploading a file to Storage, updating a database, or sending events to a logging service. You can respond to the request immediately and leave the task running in the background.

### How it works [\#](https://supabase.com/docs/guides/functions/background-tasks\#how-it-works)

You can use `EdgeRuntime.waitUntil(promise)` to explicitly mark background tasks. The Function instance continues to run until the promise provided to `waitUntil` completes.

The maximum duration is capped based on the wall-clock, CPU, and memory limits. The Function will shutdown when it reaches one of these [limits](https://supabase.com/docs/guides/functions/limits).

You can listen to the `beforeunload` event handler to be notified when Function invocation is about to be shut down.

### Example [\#](https://supabase.com/docs/guides/functions/background-tasks\#example)

Here's an example of using `EdgeRuntime.waitUntil` to run a background task and using `beforeunload` event to be notified when the instance is about to be shut down.

`
async function longRunningTask() {
// do work here
}
// Mark the longRunningTask's returned promise as a background task.
// note: we are not using await because we don't want it to block.
EdgeRuntime.waitUntil(longRunningTask())
// Use beforeunload event handler to be notified when function is about to shutdown
addEventListener('beforeunload', (ev) => {
console.log('Function will be shutdown due to', ev.detail?.reason)
// save state or log the current progress
})
// Invoke the function using a HTTP request.
// This will start the background task
Deno.serve(async (req) => {
return new Response('ok')
})
`

### Starting a background task in the request handler [\#](https://supabase.com/docs/guides/functions/background-tasks\#starting-a-background-task-in-the-request-handler)

You can call `EdgeRuntime.waitUntil` in the request handler too. This will not block the request.

`
async function fetchAndLog(url: string) {
const response = await fetch(url)
console.log(response)
}
Deno.serve(async (req) => {
// this will not block the request,
// instead it will run in the background
EdgeRuntime.waitUntil(fetchAndLog('https://httpbin.org/json'))
return new Response('ok')
})
`

### Testing background tasks locally [\#](https://supabase.com/docs/guides/functions/background-tasks\#testing-background-tasks-locally)

When testing Edge Functions locally with Supabase CLI, the instances are terminated automatically after a request is completed. This will prevent background tasks from running to completion.

To prevent that, you can update the `supabase/config.toml` with the following settings:

`
[edge_runtime]
policy = "per_worker"
`

When running with `per_worker` policy, Function won't auto-reload on edits. You will need to manually restart it by running `supabase functions serve`.

### Is this helpful?

NoYes

### On this page

[How it works](https://supabase.com/docs/guides/functions/background-tasks#how-it-works) [Example](https://supabase.com/docs/guides/functions/background-tasks#example) [Starting a background task in the request handler](https://supabase.com/docs/guides/functions/background-tasks#starting-a-background-task-in-the-request-handler) [Testing background tasks locally](https://supabase.com/docs/guides/functions/background-tasks#testing-background-tasks-locally)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings