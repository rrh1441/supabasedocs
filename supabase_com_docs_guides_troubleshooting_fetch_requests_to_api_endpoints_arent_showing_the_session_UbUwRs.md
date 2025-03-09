# Fetch requests to API endpoints aren't showing the session

Last edited: 1/18/2025

* * *

You must pass along the cookie header with the fetch request in order for your API endpoint to get access to the cookie from this request.

`
const res = await fetch('http://localhost:3000/contact', {
headers: {
    cookie: headers().get('cookie') as string,
},
})
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings