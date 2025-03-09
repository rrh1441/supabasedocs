Auth

# Custom Access Token Hook

## Customize the access token issued by Supabase Auth

* * *

The custom access token hook runs before a token is issued and allows you to add additional claims based on the authentication method used.

Claims returned must conform to our specification. Supabase Auth will check for these claims after the hook is run and return an error if they are not present.

These are the fields currently available on an access token:

Required Claims: `iss`, `aud`, `exp`, `iat`, `sub`, `role`, `aal`, `session_id`
Optional Claims: `jti`, `nbf`, `app_metadata`, `user_metadata`, `amr`, `email`, `phone`

**Inputs**

| Field | Type | Description |
| --- | --- | --- |
| `user_id` | `string` | Unique identifier for the user attempting to sign in. |
| `claims` | `object` | Claims which are included in the access token. |
| `authentication_method` | `string` | The authentication method used to request the access token. Possible values include: `oauth`, `password`, `otp`, `totp`, `recovery`, `invite`, `sso/saml`, `magiclink`, `email/signup`, `email_change`, `token_refresh`, `anonymous`. |

JSONJSON Schema

`
{
"user_id": "8ccaa7af-909f-44e7-84cb-67cdccb56be6",
"claims": {
    "aud": "authenticated",
    "exp": 1715690221,
    "iat": 1715686621,
    "sub": "8ccaa7af-909f-44e7-84cb-67cdccb56be6",
    "email": "",
    "phone": "",
    "app_metadata": {},
    "user_metadata": {},
    "role": "authenticated",
    "aal": "aal1",
    "amr": [ { "method": "anonymous", "timestamp": 1715686621 } ],
    "session_id": "4b938a09-5372-4177-a314-cfa292099ea2",
    "is_anonymous": true
},
"authentication_method": "anonymous"
}
`

**Outputs**

Return these only if your hook processed the input without errors.

| Field | Type | Description |
| --- | --- | --- |
| `claims` | `object` | The updated claims after the hook has been run. |

SQLHTTP

Minimal JWTAdd admin roleAdd claim via plv8Restrict access to SSO users

Sometimes the size of the JWT can be a problem especially if you're using a [Server-Side Rendering framework](https://supabase.com/docs/guides/auth/server-side). Common situations where the JWT can get too large include:

- The user has a particularly large name, email address or phone number
- The default JWT has too many claims coming from OAuth providers
- A large avatar URL is included

To lower the size of the JWT you can define a Custom Access Token hook like the one below which will instruct the Auth server to issue a JWT with only the listed claims. Check the documentation above on what JWT claims must be present and cannot be removed.

Refer to the [Postgres JSON functions](https://www.postgresql.org/docs/current/functions-json.html) on how to manipulate `jsonb` objects.

`
create or replace function public.custom_access_token_hook(event jsonb)
returns jsonb
language plpgsql
as $$
declare
    original_claims jsonb;
    new_claims jsonb;
    claim text;
begin
    original_claims = event->'claims';
    new_claims = '{}'::jsonb;
    foreach claim in array array[\
      -- add claims you want to keep here\
      'iss',\
      'aud',\
      'exp',\
      'iat',\
      'sub',\
      'role',\
      'aal',\
      'session_id'\
] loop
      if original_claims ? claim then
        -- original_claims contains one of the listed claims, set it on new_claims
        new_claims = jsonb_set(new_claims, array[claim], original_claims->claim);
      end if;
    end loop;
    return jsonb_build_object('claims', new_claims);
end
$$;
`

### Is this helpful?

NoYes

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings