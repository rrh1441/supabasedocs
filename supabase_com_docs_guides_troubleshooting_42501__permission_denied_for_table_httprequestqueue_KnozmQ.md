# 42501 : permission denied for table http\_request\_queue

Last edited: 2/21/2025

* * *

If you're currently blocked by the above error, run the following in your Supabase SQL editor:

- Check `select * from net.http_request_queue` and make sure it's empty.
- Try `drop extension pg_net; create extension pg_net schema extensions;`
- If that doesn't work (e.g. because some objects depend on it), then [contact support](https://supabase.com/support).

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings