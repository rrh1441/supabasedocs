Database

# pgjwt: JSON Web Tokens

* * *

The [`pgjwt`](https://github.com/michelp/pgjwt) (PostgreSQL JSON Web Token) extension allows you to create and parse [JSON Web Tokens (JWTs)](https://en.wikipedia.org/wiki/JSON_Web_Token) within a PostgreSQL database. JWTs are commonly used for authentication and authorization in web applications and services.

## Enable the extension [\#](https://supabase.com/docs/guides/database/extensions/pgjwt\#enable-the-extension)

DashboardSQL

1. Go to the [Database](https://supabase.com/dashboard/project/_/database/tables) page in the Dashboard.
2. Click on **Extensions** in the sidebar.
3. Search for `pgjwt` and enable the extension.

## API [\#](https://supabase.com/docs/guides/database/extensions/pgjwt\#api)

- [`sign(payload json, secret text, algorithm text default 'HSA256')`](https://github.com/michelp/pgjwt#usage): Signs a JWT containing _payload_ with _secret_ using _algorithm_.
- [`verify(token text, secret text, algorithm text default 'HSA256')`](https://github.com/michelp/pgjwt#usage): Decodes a JWT _token_ that was signed with _secret_ using _algorithm_.

Where:

- `payload` is an encrypted JWT represented as a string.
- `secret` is the private/secret passcode which is used to sign the JWT and verify its integrity.
- `algorithm` is the method used to sign the JWT using the secret.
- `token` is an encrypted JWT represented as a string.

## Usage [\#](https://supabase.com/docs/guides/database/extensions/pgjwt\#usage)

Once the extension is installed, you can use its functions to create and parse JWTs. Here's an example of how you can use the `sign` function to create a JWT:

`
select
extensions.sign(
    payload   := '{"sub":"1234567890","name":"John Doe","iat":1516239022}',
    secret    := 'secret',
    algorithm := 'HS256'
);
`

The `pgjwt_encode` function returns a string that represents the JWT, which can then be safely transmitted between parties.

`
              sign
---------------------------------
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpX
VCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiw
ibmFtZSI6IkpvaG4gRG9lIiwiaWF0Ijo
xNTE2MjM5MDIyfQ.XbPfbIHMI6arZ3Y9
22BhjWgQzWXcXNrz0ogtVhfEd2o
(1 row)
`

To parse a JWT and extract its claims, you can use the `verify` function. Here's an example:

`
select
extensions.verify(
    token := 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiRm9vIn0.Q8hKjuadCEhnCPuqIj9bfLhTh_9QSxshTRsA5Aq4IuM',
    secret    := 'secret',
    algorithm := 'HS256'
);
`

Which returns the decoded contents and some associated metadata.

`
           header            |    payload     | valid
-----------------------------+----------------+-------
{"alg":"HS256","typ":"JWT"} | {"name":"Foo"} | t
(1 row)
`

## Resources [\#](https://supabase.com/docs/guides/database/extensions/pgjwt\#resources)

- Official [`pgjwt` documentation](https://github.com/michelp/pgjwt)

### Is this helpful?

NoYes

### On this page

[Enable the extension](https://supabase.com/docs/guides/database/extensions/pgjwt#enable-the-extension) [API](https://supabase.com/docs/guides/database/extensions/pgjwt#api) [Usage](https://supabase.com/docs/guides/database/extensions/pgjwt#usage) [Resources](https://supabase.com/docs/guides/database/extensions/pgjwt#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings