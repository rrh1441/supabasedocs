# Database error saving new user

Last edited: 1/17/2025

* * *

You generally get this error when trying to invite a new user from the dashboard or when trying to insert a user into a table using the table editor in the Supabase dashboard.

This error is normally associated with a side effect of a database transaction.

**Common causes of this error:**

- You have a trigger/trigger function setup on the `auth.users` table
- You have added a constraint on the `auth.users` table which isn't being met
- You are using Prisma and it has broken all the permissions on the `auth.users` table

**Debugging this error:**

- You can use the [Auth logs explorer](https://app.supabase.com/project/_/logs/auth-logs) to find the issue with more information
- You can use the [Postgres logs explorer](https://app.supabase.com/project/_/logs/postgres-logs)

[https://user-images.githubusercontent.com/79497/225517698-b6e3ccaf-cd70-4acd-8124-ffbcee310d63.mp4](https://user-images.githubusercontent.com/79497/225517698-b6e3ccaf-cd70-4acd-8124-ffbcee310d63.mp4)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings