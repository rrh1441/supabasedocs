Auth

# Send Email Hook

## Use a custom email provider to send authentication messages

* * *

The Send Email Hook runs before an email is sent and allows for flexibility around email sending. You can use this hook to configure a back-up email provider or add internationalization to your emails.

## Email sending behavior [\#](https://supabase.com/docs/guides/auth/auth-hooks/send-email-hook\#email-sending-behavior)

Email sending depends on two settings: Email Provider and Auth Hook status.

| Email Provider | Auth Hook | Result |
| --- | --- | --- |
| Enabled | Enabled | Auth Hook handles email sending (SMTP not used) |
| Enabled | Disabled | SMTP handles email sending (custom if configured, default otherwise) |
| Disabled | Enabled | Email Signups Disabled |
| Disabled | Disabled | Email Signups Disabled |

**Inputs**

| Field | Type | Description |
| --- | --- | --- |
| `user` | [`User`](https://supabase.com/docs/guides/auth/users#the-user-object) | The user attempting to sign in. |
| `email` | `object` | Metadata specific to the email sending process. Includes the OTP and `token_hash`. |

JSONJSON Schema

`
{
"user": {
    "id": "8484b834-f29e-4af2-bf42-80644d154f76",
    "aud": "authenticated",
    "role": "authenticated",
    "email": "valid.email@supabase.io",
    "phone": "",
    "app_metadata": {
      "provider": "email",
      "providers": ["email"]
    },
    "user_metadata": {
      "email": "valid.email@supabase.io",
      "email_verified": false,
      "phone_verified": false,
      "sub": "8484b834-f29e-4af2-bf42-80644d154f76"
    },
    "identities": [\
      {\
        "identity_id": "bc26d70b-517d-4826-bce4-413a5ff257e7",\
        "id": "8484b834-f29e-4af2-bf42-80644d154f76",\
        "user_id": "8484b834-f29e-4af2-bf42-80644d154f76",\
        "identity_data": {\
          "email": "valid.email@supabase.io",\
          "email_verified": false,\
          "phone_verified": false,\
          "sub": "8484b834-f29e-4af2-bf42-80644d154f76"\
        },\
        "provider": "email",\
        "last_sign_in_at": "2024-05-14T12:56:33.824231484Z",\
        "created_at": "2024-05-14T12:56:33.824261Z",\
        "updated_at": "2024-05-14T12:56:33.824261Z",\
        "email": "valid.email@supabase.io"\
      }\
    ],
    "created_at": "2024-05-14T12:56:33.821567Z",
    "updated_at": "2024-05-14T12:56:33.825595Z",
    "is_anonymous": false
},
"email_data": {
    "token": "305805",
    "token_hash": "7d5b7b1964cf5d388340a7f04f1dbb5eeb6c7b52ef8270e1737a58d0",
    "redirect_to": "http://localhost:3000/",
    "email_action_type": "signup",
    "site_url": "http://localhost:9999",
    "token_new": "",
    "token_hash_new": ""
}
}
`

**Outputs**

- No outputs are required. An empty response with a status code of 200 is taken as a successful response.

SQLHTTP

Use Resend as an email providerAdd Internationalization for Email Templates

You can configure [Resend](https://resend.com/) as the custom email provider through the "Send Email" hook. This allows you to take advantage of Resend's developer-friendly APIs to send emails and leverage [React Email](https://react.email/) for managing your email templates. For a more advanced React Email tutorial, refer to [this guide](https://supabase.com/docs/guides/functions/examples/auth-send-email-hook-react-email-resend).

If you want to send emails through the Supabase Resend integration, which uses Resend's SMTP server, check out [this integration](https://supabase.com/partners/integrations/resend) instead.

Create a `.env` file with the following environment variables:

`
RESEND_API_KEY="your_resend_api_key"
SEND_EMAIL_HOOK_SECRET="v1,whsec_<base64_secret>"
`

You can generate the secret in the [Auth Hooks](https://supabase.com/dashboard/project/_/auth/hooks) section of the Supabase dashboard.

Set the secrets in your Supabase project:

`
supabase secrets set --env-file .env
`

Create a new edge function:

`
supabase functions new send-email
`

Add the following code to your edge function:

``
import { Webhook } from "https://esm.sh/standardwebhooks@1.0.0";
import { Resend } from "npm:resend";
const resend = new Resend(Deno.env.get("RESEND_API_KEY") as string);
const hookSecret = (Deno.env.get("SEND_EMAIL_HOOK_SECRET") as string).replace("v1,whsec_", "");
Deno.serve(async (req) => {
if (req.method !== "POST") {
    return new Response("not allowed", { status: 400 });
}
const payload = await req.text();
const headers = Object.fromEntries(req.headers);
const wh = new Webhook(hookSecret);
try {
    const { user, email_data } = wh.verify(payload, headers) as {
      user: {
        email: string;
      };
      email_data: {
        token: string;
        token_hash: string;
        redirect_to: string;
        email_action_type: string;
        site_url: string;
        token_new: string;
        token_hash_new: string;
      };
    };
    const { error } = await resend.emails.send({
      from: "welcome <onboarding@example.com>",
      to: [user.email],
      subject: "Welcome to my site!",
      text: `Confirm you signup with this code: ${email_data.token}`,
    });
    if (error) {
      throw error;
    }
} catch (error) {
    return new Response(
      JSON.stringify({
        error: {
          http_code: error.code,
          message: error.message,
        },
      }),
      {
        status: 401,
        headers: { "Content-Type": "application/json" },
      },
    );
}
const responseHeaders = new Headers();
responseHeaders.set("Content-Type", "application/json");
return new Response(JSON.stringify({}), {
    status: 200,
    headers: responseHeaders,
});
});
``

Deploy your edge function and [configure it as a hook](https://supabase.com/dashboard/project/_/auth/hooks):

`
supabase functions deploy send-email --no-verify-jwt
`

### Is this helpful?

NoYes

### On this page

[Email sending behavior](https://supabase.com/docs/guides/auth/auth-hooks/send-email-hook#email-sending-behavior)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings