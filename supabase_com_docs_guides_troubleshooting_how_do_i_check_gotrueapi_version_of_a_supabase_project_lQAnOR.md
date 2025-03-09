# How do I check GoTrue/API version of a Supabase project?

Last edited: 1/17/2025

* * *

Make a `GET` request to the health check endpoint to retrieve this information. Below is an example using `curl`:

`
curl -X GET 'https://project-ref.supabase.co/auth/v1/health' -H 'apikey: ANON_KEY'
{
    "version": "v2.60.7",
    "name": "GoTrue",
    "description": "GoTrue is a user registration and authentication API"
}
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings