Auth

# JWTs

## JSON Web Tokens

* * *

A [JSON Web Token](https://jwt.io/introduction) is a type of data structure, represented as a string, that usually contains identity and authorization information about a user. It encodes information about its lifetime and is signed with a cryptographic key to make it tamper-resistant.

Supabase Access Tokens are JWTs. The JWT is sent along with every request to Supabase services. By verifying the token and inspecting the included claims, you can allow or deny access to resources. [Row Level Security](https://supabase.com/docs/guides/database/postgres/row-level-security) policies are based on the information present in JWTs.

## Encoding and signing JWTs [\#](https://supabase.com/docs/guides/auth/jwts\#encoding-and-signing-jwts)

JWTs are encoded and signed as follows.

The JSON object starts out looking something like this:

`
{
"sub": "0001",
"name": "Sam Vimes",
"iat": 1516239022,
"exp": 1518239022
}
`

`sub` is the "subject", which is usually the UUID of the user. `name` is self-explanatory, and `iat` is the Unix timestamp at which the token was created. Many JWTs will also have an `exp`, which is the date at which the token is set to expire and can no longer be used. These are some of the standard fields you may find in a JWT, but you can pretty much store whatever you want in there, for example:

`
{
"sub": "0002",
"name": "Věra Hrabánková",
"iat": 1516239022,
"exp": 1518239022,
"theme": {
      "primary" : "#D80C14",
      "secondary" : "#FFFFFF"
}
}
`

Just note that the more data you store in your token, the longer the encoded string will be.

When we want to send the JWT to the user, we first encode the data using an algorithm such as `HS256`. There are many libraries (and several different algorithms) that can be used to do this encoding/decoding, such as [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken). The signing is as simple as:

`
// from https://replit.com/@awalias/jsonwebtokens#index.js
let token = jwt.sign({ name: 'Sam Vimes' }, 'some-secret')
`

And the resulting string will look like this:

`
eyJhbGciOiJIUzI1NiJ9
.eyJzdWIiOiIwMDAxIiwibmFtZSI6IlNhbSBWaW1lcyIsImlhdCI6MTUxNjIzOTAyMiwiZXhwIjoxNTE4MjM5MDIyfQ
.zMcHjKlkGhuVsiPIkyAkB2rjXzyzJsMMgpvEGvGtjvA
`

You will notice that the string is actually made up of three components:

The first segment `eyJhbGciOiJIUzI1NiJ9` is known as the "header", and when decoded just tells us which algorithm was used to do the encoding:

`
{
"alg": "HS256"
}
`

The second segment `eyJzdWIiOiIwMDAxIiwibmFtZSI6IlNhbSBWaW1lcyIsImlhdCI6MTUxNjIzOTAyMiwiZXhwIjoxNTE4MjM5MDIyfQ` contains our original payload:

`
{
"sub": "0001",
"name": "Sam Vimes",
"iat": 1516239022,
"exp": 1518239022
}
`

The last segment `zMcHjKlkGhuVsiPIkyAkB2rjXzyzJsMMgpvEGvGtjvA` is the signature itself, which is the part used by the website or service provider to verify that a token sent by some user is legitimate. It is produced in the first instance by running the cryptographic function HS256 on the following input:

`
HMACSHA256(
base64UrlEncode(header) + "." +
base64UrlEncode(payload)
<jwt_secret>
)
`

You can test out minting your own tokens on [https://jwt.io](https://jwt.io/).

It is important to note that anyone who possesses the `jwt_secret` here can create new tokens, and also verify existing ones. More advanced JWT algorithms use two secrets: one for the creation of tokens, and a separate one to verify the validity of signed tokens.

You might wonder why JWTs are so popular all of a sudden. The answer is that with the mass adoption of microservice architecture, we were in a situation where several distinct microservices (APIs, websites, servers, etc.) want to validate that a user is who they say they are, or are in other words a "logged-in" user. Traditional session tokens are no use here, since they would require each microservice to either maintain a record of currently valid session tokens or to query a central database each time a user wants to access a resource in order to check the validity of the session token – very inefficient indeed. JWT-based auth in this sense is decentralized, since anyone with the `jwt_secret` can verify a token without needing access to a centralized database.

Note: One downside of JWTs is that they are not easily voidable, unlike session tokens. If a JWT is leaked to a malicious actor, they will be able to redeem it anywhere until the expiry date is reached – unless of course the system owner updates the `jwt_secret` (which will of course invalidate _everyone's_ existing tokens).

## JWTs in Supabase [\#](https://supabase.com/docs/guides/auth/jwts\#jwts-in-supabase)

In Supabase we issue JWTs for three different purposes:

1. `anon key`: This key is used to bypass the Supabase API gateway and can be used in your client-side code.
2. `service role key`: This key has super admin rights and can bypass your Row Level Security. Do not put it in your client-side code. Keep it private.
3. `user specific jwts`: These are tokens we issue to users who log into your project/service/website. It's the modern equivalent of a session token, and can be used by a user to access content or permissions specific to them.

The first token here, the `anon key` token, is for developers to send along with their API requests whenever they want to interact with their Supabase database.

Let's say you want to read the names of all the rows in a table `colors`. We would make a request like:

`
curl 'https://xscduanzzfseqszwzhcy.supabase.co/rest/v1/colors?select=name' \
-H "apikey: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJyb2xlIjoiYW5vbiIsImlhdCI6MTYxNDIwNTE3NCwiZXhwIjoxOTI5NzgxMTc0fQ.-NBR1WnZyQGpRLdXJfgfpszoZ0EeE6KHatJsDPLIX8c"
`

If we put this token into [https://jwt.io](https://jwt.io/), we see it decodes to:

`
{
"role": "anon",
"iss": "supabase",
"iat": 1614205174,
"exp": 1929781174
}
`

This JWT is signed by a `jwt_secret` specific to the developer's Supabase token (you can find this secret alongside this encoded "anon key" on your Dashboard under Settings > API page) and is required to get past the Supabase API gateway and access the developer's project.

The idea with this particular key is that it's safe to put into your client, meaning it's okay if your end users see this key – but _only_ if you first enable Row Level Security.

The second key, `service role key`, should only ever be used on one of your own servers or environments, and should never be shared with end users. You might use this token to do things like make batch inserts of data.

The `user access token` is the JWT issued when you call for example:

`
supabase.auth.signIn({
email: 'valid.email@supabase.io',
password: 'They_Live_1988!',
})
`

This token should be passed in addition to the `apikey` header as an `Authorization Bearer` header like:

`
curl 'https://xscduanzzfseqszwzhcy.supabase.co/rest/v1/colors?select=name' \
-H "apikey: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJyb2xlIjoiYW5vbiIsImlhdCI6MTYxNDIwNTE3NCwiZXhwIjoxOTI5NzgxMTc0fQ.-NBR1WnZyQGpRLdXJfgfpszoZ0EeE6KHatJsDPLIX8c" \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJhdXRoZW50aWNhdGVkIiwiZXhwIjoxNjE1ODI0Mzg4LCJzdWIiOiIwMzM0NzQ0YS1mMmEyLTRhYmEtOGM4YS02ZTc0OGY2MmExNzIiLCJlbWFpbCI6InNvbWVvbmVAZW1haWwuY29tIiwiYXBwX21ldGFkYXRhIjp7InByb3ZpZGVyIjoiZW1haWwifSwidXNlcl9tZXRhZGF0YSI6bnVsbCwicm9sZSI6ImF1dGhlbnRpY2F0ZWQifQ.I-_oSsJamtinGxniPETBf-ezAUwDW2sY9bJIThvdX9s"
`

You'll notice that this token is quite a bit longer, since it contains information specific to the user such as:

`
{
"aud": "authenticated",
"exp": 1615824388,
"sub": "0334744a-f2a2-4aba-8c8a-6e748f62a172",
"email": "valid.email@supabase.io",
"app_metadata": {
    "provider": "email"
},
"user_metadata": null,
"role": "authenticated"
}
`

If using the service role key, you'll need to pass it into both the `apikey` and `authorization` headers (again, only do this from a secure environment such as your own server):

`
curl "$YOUR_PROJECT_URL/rest/v1/colors?select=name" \
 -H "apikey: $YOUR_SERVICE_ROLE_KEY" \
 -H "authorization: Bearer $YOUR_SERVICE_ROLE_KEY"
`

Now that you understand what JWTs are and where they're used in Supabase, you can explore how to use them in combination with Row Level Security to start restricting access to certain tables, rows, and columns in your Postgres database.

## Resources [\#](https://supabase.com/docs/guides/auth/jwts\#resources)

- JWT debugger: [https://jwt.io/](https://jwt.io/)

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2Fv3Exg5YpJvE%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Encoding and signing JWTs](https://supabase.com/docs/guides/auth/jwts#encoding-and-signing-jwts) [JWTs in Supabase](https://supabase.com/docs/guides/auth/jwts#jwts-in-supabase) [Resources](https://supabase.com/docs/guides/auth/jwts#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings