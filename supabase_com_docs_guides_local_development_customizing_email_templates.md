Local Development

# Customizing email templates

## Customizing local email templates using config.toml.

* * *

You can customize the email templates for local development [using the `config.toml` settings](https://supabase.com/docs/guides/cli/config#auth-config).

## Configuring templates [\#](https://supabase.com/docs/guides/local-development/customizing-email-templates\#configuring-templates)

You should provide a relative URL to the `content_path` parameter, pointing to an HTML file which contains the template. For example

supabase/config.toml

supabase/templates/invite.html

`
[auth.email.template.invite]
subject = "You are invited to Acme Inc"
content_path = "./supabase/templates/invite.html"
`

## Available email templates [\#](https://supabase.com/docs/guides/local-development/customizing-email-templates\#available-email-templates)

There are several Auth email templates which can be configured:

- `auth.email.template.invite`
- `auth.email.template.confirmation`
- `auth.email.template.recovery`
- `auth.email.template.magic_link`
- `auth.email.template.email_change`

## Template variables [\#](https://supabase.com/docs/guides/local-development/customizing-email-templates\#template-variables)

The templating system provides the following variables for use:

### `ConfirmationURL` [\#](https://supabase.com/docs/guides/local-development/customizing-email-templates\#confirmationurl)

Contains the confirmation URL. For example, a signup confirmation URL would look like:

`
https://project-ref.supabase.co/auth/v1/verify?token={{ .TokenHash }}&type=email&redirect_to=https://example.com/path
`

**Usage**

`
<p>Click here to confirm: {{ .ConfirmationURL }}</p>
`

### `Token` [\#](https://supabase.com/docs/guides/local-development/customizing-email-templates\#token)

Contains a 6-digit One-Time-Password (OTP) that can be used instead of the `ConfirmationURL`.

**Usage**

`
<p>Here is your one time password: {{ .Token }}</p>
`

### `TokenHash` [\#](https://supabase.com/docs/guides/local-development/customizing-email-templates\#tokenhash)

Contains a hashed version of the `Token`. This is useful for constructing your own email link in the email template.

**Usage**

`
<p>Follow this link to confirm your user:</p>
<p>
<a href="{{ .SiteURL }}/auth/confirm?token_hash={{ .TokenHash }}&type=email"
    >Confirm your email</a
>
</p>
`

### `SiteURL` [\#](https://supabase.com/docs/guides/local-development/customizing-email-templates\#siteurl)

Contains your application's Site URL. This can be configured in your project's [authentication settings](https://supabase.com/dashboard/project/_/auth/url-configuration).

**Usage**

`
<p>Visit <a href="{{ .SiteURL }}">here</a> to log in.</p>
`

### `Email` [\#](https://supabase.com/docs/guides/local-development/customizing-email-templates\#email)

Contains the user's email address.

**Usage**

`
<p>A recovery request was sent to {{ .Email }}.</p>
`

### `NewEmail` [\#](https://supabase.com/docs/guides/local-development/customizing-email-templates\#newemail)

Contains the new user's email address. This is only available in the `email_change` email template.

**Usage**

`
<p>You are requesting to update your email address to {{ .NewEmail }}.</p>
`

## Deploying email templates [\#](https://supabase.com/docs/guides/local-development/customizing-email-templates\#deploying-email-templates)

These settings are for local development. To apply the changes locally, stop and restart the Supabase containers:

`
supabase stop && supabase start
`

For hosted projects managed by Supabase, copy the templates into the [Email Templates](https://supabase.com/dashboard/project/_/auth/templates) section of the Dashboard.

### Is this helpful?

NoYes

### On this page

[Configuring templates](https://supabase.com/docs/guides/local-development/customizing-email-templates#configuring-templates) [Available email templates](https://supabase.com/docs/guides/local-development/customizing-email-templates#available-email-templates) [Template variables](https://supabase.com/docs/guides/local-development/customizing-email-templates#template-variables) [ConfirmationURL](https://supabase.com/docs/guides/local-development/customizing-email-templates#confirmationurl) [Token](https://supabase.com/docs/guides/local-development/customizing-email-templates#token) [TokenHash](https://supabase.com/docs/guides/local-development/customizing-email-templates#tokenhash) [SiteURL](https://supabase.com/docs/guides/local-development/customizing-email-templates#siteurl) [Email](https://supabase.com/docs/guides/local-development/customizing-email-templates#email) [NewEmail](https://supabase.com/docs/guides/local-development/customizing-email-templates#newemail) [Deploying email templates](https://supabase.com/docs/guides/local-development/customizing-email-templates#deploying-email-templates)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings