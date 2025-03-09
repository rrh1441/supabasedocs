Telemetry

# Log Drains

* * *

Log drains will send all logs of the Supabase stack to one or more desired destinations. It is only available for customers on Team and Enterprise Plans. Log drains is available in the dashboard under [Project Settings > Log Drains](https://supabase.com/dashboard/project/_/settings/log-drains).

You can read about the initial announcement [here](https://supabase.com/blog/log-drains) and vote for your preferred drains in [this discussion](https://github.com/orgs/supabase/discussions/28324?sort=top).

# Supported destinations

The following table lists the supported destinations and the required setup configuration:

| Destination | Transport Method | Configuration |
| --- | --- | --- |
| Generic HTTP endpoint | HTTP | URL <br> HTTP Version <br> Gzip <br> Headers |
| DataDog | HTTP | API Key <br> Region |

HTTP requests are batched with a max of 250 logs or 1 second intervals, whichever happens first. Logs are compressed via Gzip if the destination supports it.

## Generic HTTP endpoint [\#](https://supabase.com/docs/guides/telemetry/log-drains\#generic-http-endpoint)

Logs are sent as a POST request with a JSON body. Both HTTP/1 and HTTP/2 protocols are supported.
Custom headers can optionally be configured for all requests.

Note that requests are **unsigned**.

Unsigned requests to HTTP endpoints are temporary and all requests will signed in the near future.

Edge Function Walkthrough (Uncompressed)

Edge Function Gzip Example

## DataDog logs [\#](https://supabase.com/docs/guides/telemetry/log-drains\#datadog-logs)

Logs sent to DataDog have the name of the log source set on the `service` field of the event and the source set to `Supabase`. Logs are gzipped before they are sent to DataDog.

The payload message is a JSON string of the raw log event, prefixed with the event timestamp.

To setup DataDog log drain, generate a DataDog API key [here](https://app.datadoghq.com/organization-settings/api-keys) and the location of your DataDog site.

Walkthrough

Example destination configuration

If you are interested in other log drains, upvote them [here](https://github.com/orgs/supabase/discussions/28324)

## Pricing [\#](https://supabase.com/docs/guides/telemetry/log-drains\#pricing)

For a detailed breakdown of how charges are calculated, refer to [Manage Log Drain usage](https://supabase.com/docs/guides/platform/manage-your-usage/log-drains).

### Is this helpful?

NoYes

### On this page

[Generic HTTP endpoint](https://supabase.com/docs/guides/telemetry/log-drains#generic-http-endpoint) [DataDog logs](https://supabase.com/docs/guides/telemetry/log-drains#datadog-logs) [Pricing](https://supabase.com/docs/guides/telemetry/log-drains#pricing)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings