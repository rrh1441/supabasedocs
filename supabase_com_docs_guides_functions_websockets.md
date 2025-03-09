Edge Functions

# Handling WebSockets

## How to handle WebSocket connections in Edge Functions

* * *

Edge Functions supports hosting WebSocket servers that can facilitate bi-directional communications with browser clients.

You can also establish outgoing WebSocket client connections to another server from Edge Functions (e.g., [OpenAI Realtime API](https://platform.openai.com/docs/guides/realtime/overview)). You can find an example OpenAI Realtime Relay Server implementation on the [supabase-community GitHub account](https://github.com/supabase-community/openai-realtime-console?tab=readme-ov-file#using-supabase-edge-functions-as-a-relay-server).

### Writing a WebSocket server [\#](https://supabase.com/docs/guides/functions/websockets\#writing-a-websocket-server)

Here are some basic examples of setting up WebSocket servers using Deno and Node.js APIs.

DenoNode.js

`
Deno.serve(req => {
    const upgrade = req.headers.get("upgrade") || "";
    if (upgrade.toLowerCase() != "WebSocket") {
        return new Response("request isn't trying to upgrade to WebSocket.", { status: 400 });
    }
    const { socket, response } = Deno.upgradeWebSocket(req);
    socket.onopen = () => console.log("socket opened");
    socket.onmessage = (e) => {
        console.log("socket message:", e.data);
        socket.send(new Date().toString());
    };
    socket.onerror = e => console.log("socket errored:", e.message);
    socket.onclose = () => console.log("socket closed");
    return response;
});
`

### Outbound WebSockets [\#](https://supabase.com/docs/guides/functions/websockets\#outbound-websockets)

You can also establish an outbound WebSocket connection to another server from an Edge Function.

Combining it with incoming WebSocket servers, it's possible to use Edge Functions as a WebSocket proxy, for example as a [relay server](https://github.com/supabase-community/openai-realtime-console?tab=readme-ov-file#using-supabase-edge-functions-as-a-relay-server) for the [OpenAI Realtime API](https://platform.openai.com/docs/guides/realtime/overview).

supabase/functions/relay/index.ts

``
import { createServer } from "node:http";
import { WebSocketServer } from "npm:ws";
import { RealtimeClient } from "https://raw.githubusercontent.com/openai/openai-realtime-api-beta/refs/heads/main/lib/client.js";
// ...
const OPENAI_API_KEY = Deno.env.get("OPENAI_API_KEY");
const server = createServer();
// Since we manually created the HTTP server,
// turn on the noServer mode.
const wss = new WebSocketServer({ noServer: true });
wss.on("connection", async (ws) => {
console.log("socket opened");
if (!OPENAI_API_KEY) {
    throw new Error("OPENAI_API_KEY is not set");
}
// Instantiate new client
console.log(`Connecting with key "${OPENAI_API_KEY.slice(0, 3)}..."`);
const client = new RealtimeClient({ apiKey: OPENAI_API_KEY });
// Relay: OpenAI Realtime API Event -> Browser Event
client.realtime.on("server.*", (event) => {
    console.log(`Relaying "${event.type}" to Client`);
    ws.send(JSON.stringify(event));
});
client.realtime.on("close", () => ws.close());
// Relay: Browser Event -> OpenAI Realtime API Event
// We need to queue data waiting for the OpenAI connection
const messageQueue = [];
const messageHandler = (data) => {
    try {
      const event = JSON.parse(data);
      console.log(`Relaying "${event.type}" to OpenAI`);
      client.realtime.send(event.type, event);
    } catch (e) {
      console.error(e.message);
      console.log(`Error parsing event from client: ${data}`);
    }
};
ws.on("message", (data) => {
    if (!client.isConnected()) {
      messageQueue.push(data);
    } else {
      messageHandler(data);
    }
});
ws.on("close", () => client.disconnect());
// Connect to OpenAI Realtime API
try {
    console.log(`Connecting to OpenAI...`);
    await client.connect();
} catch (e) {
    console.log(`Error connecting to OpenAI: ${e.message}`);
    ws.close();
    return;
}
console.log(`Connected to OpenAI successfully!`);
while (messageQueue.length) {
    messageHandler(messageQueue.shift());
}
});
server.on("upgrade", (req, socket, head) => {
wss.handleUpgrade(req, socket, head, (ws) => {
    wss.emit("connection", ws, req);
});
});
server.listen(8080);
``

[View source](https://github.com/supabase-community/openai-realtime-console/blob/0f93657a71670704fbf77c48cf54d6c9eb956698/supabase/functions/relay/index.ts)

### Authentication [\#](https://supabase.com/docs/guides/functions/websockets\#authentication)

WebSocket browser clients don't have the option to send custom headers. Because of this, Edge Functions won't be able to perform the usual authorization header check to verify the JWT.

You can skip the default authorization header checks by explicitly providing `--no-verify-jwt` when serving and deploying functions.

To authenticate the user making WebSocket requests, you can pass the JWT in URL query params or via a custom protocol.

Using query paramsUsing custom protocol

`
import { createClient } from "jsr:@supabase/supabase-js@2";
const supabase = createClient(
Deno.env.get("SUPABASE_URL"),
Deno.env.get("SUPABASE_SERVICE_ROLE_KEY"),
);
Deno.serve(req => {
const upgrade = req.headers.get("upgrade") || "";
    if (upgrade.toLowerCase() != "WebSocket") {
        return new Response("request isn't trying to upgrade to WebSocket.", { status: 400 });
    }
// Please be aware query params may be logged in some logging systems.
const url = new URL(req.url);
const jwt = url.searchParams.get("jwt");
if (!jwt) {
console.error("Auth token not provided");
return new Response("Auth token not provided", { status: 403 });
}
const { error, data } = await supabase.auth.getUser(jwt);
if (error) {
console.error(error);
return new Response("Invalid token provided", { status: 403 });
}
if (!data.user) {
console.error("user is not authenticated");
return new Response("User is not authenticated", { status: 403 });
}
    const { socket, response } = Deno.upgradeWebSocket(req);
    socket.onopen = () => console.log("socket opened");
    socket.onmessage = (e) => {
        console.log("socket message:", e.data);
        socket.send(new Date().toString());
    };
    socket.onerror = e => console.log("socket errored:", e.message);
    socket.onclose = () => console.log("socket closed");
    return response;
});
`

### Limits [\#](https://supabase.com/docs/guides/functions/websockets\#limits)

The maximum duration is capped based on the wall-clock, CPU, and memory limits. The Function will shutdown when it reaches one of these [limits](https://supabase.com/docs/guides/functions/limits).

### Testing WebSockets locally [\#](https://supabase.com/docs/guides/functions/websockets\#testing-websockets-locally)

When testing Edge Functions locally with Supabase CLI, the instances are terminated automatically after a request is completed. This will prevent keeping WebSocket connections open.

To prevent that, you can update the `supabase/config.toml` with the following settings:

`
[edge_runtime]
policy = "per_worker"
`

When running with `per_worker` policy, Function won't auto-reload on edits. You will need to manually restart it by running `supabase functions serve`.

### Is this helpful?

NoYes

### On this page

[Writing a WebSocket server](https://supabase.com/docs/guides/functions/websockets#writing-a-websocket-server) [Outbound WebSockets](https://supabase.com/docs/guides/functions/websockets#outbound-websockets) [Authentication](https://supabase.com/docs/guides/functions/websockets#authentication) [Limits](https://supabase.com/docs/guides/functions/websockets#limits) [Testing WebSockets locally](https://supabase.com/docs/guides/functions/websockets#testing-websockets-locally)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings