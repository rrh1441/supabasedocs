Local Development

# Managing config and secrets

* * *

The Supabase CLI uses a `config.toml` file to manage local configuration. This file is located in the `supabase` directory of your project.

## Config reference [\#](https://supabase.com/docs/guides/local-development/managing-config\#config-reference)

The `config.toml` file is automatically created when you run `supabase init`.

There are a wide variety of options available, which can be found in the [CLI Config Reference](https://supabase.com/docs/guides/cli/config).

For example, to enable the "Apple" OAuth provider for local development, you can append the following information to `config.toml`:

`
[auth.external.apple]
enabled = false
client_id = ""
secret = ""
redirect_uri = "" # Overrides the default auth redirectUrl.
`

## Using secrets inside config.toml [\#](https://supabase.com/docs/guides/local-development/managing-config\#using-secrets-inside-configtoml)

You can reference environment variables within the `config.toml` file using the `env()` function. This will detect any values stored in an `.env` file at the root of your project directory. This is particularly useful for storing sensitive information like API keys, and any other values that you don't want to check into version control.

`
.
├── .env
├── .env.example
└── supabase
    └── config.toml
`

Do NOT commit your `.env` into git. Be sure to configure your `.gitignore` to exclude this file.

For example, if your `.env` contained the following values:

`
GITHUB_CLIENT_ID=""
GITHUB_SECRET=""
`

Then you would reference them inside of our `config.toml` like this:

`
[auth.external.github]
enabled = true
client_id = "env(GITHUB_CLIENT_ID)"
secret = "env(GITHUB_SECRET)"
redirect_uri = "" # Overrides the default auth redirectUrl.
`

### Is this helpful?

NoYes

### On this page

[Config reference](https://supabase.com/docs/guides/local-development/managing-config#config-reference) [Using secrets inside config.toml](https://supabase.com/docs/guides/local-development/managing-config#using-secrets-inside-configtoml)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings