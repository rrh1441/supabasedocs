# Check usage for monthly active users (MAU)

Last edited: 1/17/2025

* * *

You can see the usage in your [projects usage page](https://app.supabase.com/project/_/settings/billing/usage).

For MAU, we rely on the [Auth Server](https://github.com/supabase/auth) logs. MAU count is relative to your billing cycle and resets whenever your billing cycle resets. You can check your Auth logs in your [projects logs & analytics section](https://supabase.com/dashboard/project/_/logs/auth-logs).

We do a distinct count of all user ids in the billing cycle. An Auth event can be a login, token refresh, logout, ... If an authenticated user does any of this, we count it towards the MAU. A user is only counted once towards MAU in a billing cycle.

The log retention for you as a user (accessible time) depends on your plan. Free plan users can access the logs of the last day, Pro plan users 7 days, Team plan users 28 days, Enterprise users 90 days. Unless you're on an Enterprise plan, you won't be able to execute the query to determine MAU yourself, as you don't have access to the logs in the past 30ish days (depending on your billing cycle).

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings