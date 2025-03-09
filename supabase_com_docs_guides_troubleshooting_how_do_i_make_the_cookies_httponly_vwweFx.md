# How do I make the cookies HttpOnly?

Last edited: 1/17/2025

* * *

This is not necessary. Both the access token and refresh token are designed to be passed around to different components in your application. The browser-based side of your application needs access to the refresh token to properly maintain a browser session anyway.

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings