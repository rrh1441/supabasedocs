Edge Functions

# Status codes

## Edge Functions can return following status codes.

* * *

## 2XX Success [\#](https://supabase.com/docs/guides/functions/status-codes\#2xx-success)

A successful Edge Function Response

## 3XX Redirect [\#](https://supabase.com/docs/guides/functions/status-codes\#3xx-redirect)

The Edge Function has responded with a `Response.redirect` [API docs](https://developer.mozilla.org/en-US/docs/Web/API/Response/redirect_static)

## 4XX Client Errors [\#](https://supabase.com/docs/guides/functions/status-codes\#4xx-client-errors)

### 401 Unauthorized [\#](https://supabase.com/docs/guides/functions/status-codes\#401-unauthorized)

If the Edge Function has `Verify JWT` option enabled, but the request was made with an invalid JWT.

### 404 Not Found [\#](https://supabase.com/docs/guides/functions/status-codes\#404-not-found)

Requested Edge Function was not found.

### 405 Method Not Allowed [\#](https://supabase.com/docs/guides/functions/status-codes\#405-method-not-allowed)

Edge Functions only support these HTTP methods: 'POST', 'GET', 'PUT', 'PATCH', 'DELETE', 'OPTIONS'

## 5XX Server Errors [\#](https://supabase.com/docs/guides/functions/status-codes\#5xx-server-errors)

### 500 Internal Server Error [\#](https://supabase.com/docs/guides/functions/status-codes\#500-internal-server-error)

Edge Function threw an uncaught exception ( `WORKER_ERROR`). Check Edge Function logs to find the cause.

### 503 Service Unavailable [\#](https://supabase.com/docs/guides/functions/status-codes\#503-service-unavailable)

Edge Function failed to start ( `BOOT_ERROR`). Check Edge Function logs to find the cause.

### 504 Gateway Timeout [\#](https://supabase.com/docs/guides/functions/status-codes\#504-gateway-timeout)

Edge Function didn't respond before the [request idle timeout](https://supabase.com/docs/guides/functions/limits).

### 546 Resource Limit (Custom Error Code) [\#](https://supabase.com/docs/guides/functions/status-codes\#546-resource-limit-custom-error-code)

Edge Function execution was stopped due to a resource limit ( `WORKER_LIMIT`). Edge Function logs should provide which [resource limit](https://supabase.com/docs/guides/functions/limits) was exceeded.

### Is this helpful?

NoYes

### On this page

[2XX Success](https://supabase.com/docs/guides/functions/status-codes#2xx-success) [3XX Redirect](https://supabase.com/docs/guides/functions/status-codes#3xx-redirect) [4XX Client Errors](https://supabase.com/docs/guides/functions/status-codes#4xx-client-errors) [401 Unauthorized](https://supabase.com/docs/guides/functions/status-codes#401-unauthorized) [404 Not Found](https://supabase.com/docs/guides/functions/status-codes#404-not-found) [405 Method Not Allowed](https://supabase.com/docs/guides/functions/status-codes#405-method-not-allowed) [5XX Server Errors](https://supabase.com/docs/guides/functions/status-codes#5xx-server-errors) [500 Internal Server Error](https://supabase.com/docs/guides/functions/status-codes#500-internal-server-error) [503 Service Unavailable](https://supabase.com/docs/guides/functions/status-codes#503-service-unavailable) [504 Gateway Timeout](https://supabase.com/docs/guides/functions/status-codes#504-gateway-timeout) [546 Resource Limit (Custom Error Code)](https://supabase.com/docs/guides/functions/status-codes#546-resource-limit-custom-error-code)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings