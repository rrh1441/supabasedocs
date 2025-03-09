Auth

# Rate limits

## Rate limits protect your services from abuse

* * *

Supabase Auth enforces rate limits on endpoints to prevent abuse. Some rate limits are [customizable](https://supabase.com/dashboard/project/_/auth/rate-limits).

| Endpoint | Path | Limited By | Rate Limit |
| --- | --- | --- | --- |
| All endpoints that send emails | `/auth/v1/signup` `/auth/v1/recover` `/auth/v1/user` [1](https://supabase.com/docs/guides/auth/rate-limits#user-content-fn-1) | Sum of combined requests | Defaults to 4 emails per hour as of 14th July 2023. As of 21 Oct 2023, this has been updated to 2 emails per hour. You can only change this with your own custom SMTP setup. |
| All endpoints that send One-Time-Passwords (OTP) | `/auth/v1/otp` | Sum of combined requests | Defaults to 30 OTPs per hour. Is customizable. |
| Send OTPs or magiclinks | `/auth/v1/otp` | Last request | Defaults to 60 seconds window before a new request is allowed. Is customizable. |
| Signup confirmation request | `/auth/v1/signup` | Last request | Defaults to 60 seconds window before a new request is allowed. Is customizable. |
| Password Reset Request | `/auth/v1/recover` | Last request | Defaults to 60 seconds window before a new request is allowed. Is customizable. |
| Verification requests | `/auth/v1/verify` | IP Address | 360 requests per hour (with bursts up to 30 requests) |
| Token refresh requests | `/auth/v1/token` | IP Address | 1800 requests per hour (with bursts up to 30 requests) |
| Create or Verify an MFA challenge | `/auth/v1/factors/:id/challenge` `/auth/v1/factors/:id/verify` | IP Address | 15 requests per hour (with bursts up to  requests) |
| Anonymous sign-ins | `/auth/v1/signup` [2](https://supabase.com/docs/guides/auth/rate-limits#user-content-fn-2) | IP Address | 30 requests per hour (with bursts up to 30 requests) |

## Footnotes [\#](https://supabase.com/docs/guides/auth/rate-limits\#footnote-label)

1. The rate limit is only applied on `/auth/v1/user` if this endpoint is called to update the user's email address. [↩](https://supabase.com/docs/guides/auth/rate-limits#user-content-fnref-1)

2. The rate limit is only applied on `/auth/v1/signup` if this endpoint is called without passing in an email or phone number in the request body. [↩](https://supabase.com/docs/guides/auth/rate-limits#user-content-fnref-2)


### Is this helpful?

NoYes

### On this page

[Footnotes](https://supabase.com/docs/guides/auth/rate-limits#footnote-label)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings