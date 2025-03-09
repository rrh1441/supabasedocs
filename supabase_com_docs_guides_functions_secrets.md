Edge Functions

# Managing Secrets (Environment Variables)

## Managing secrets and environment variables.

* * *

It's common that you will need to use environment variables or other sensitive information Edge Functions. You can manage secrets using the CLI or the Dashboard.

You can access these using Deno's built-in handler

`
Deno.env.get('MY_SECRET_NAME')
`

## Default secrets [\#](https://supabase.com/docs/guides/functions/secrets\#default-secrets)

Edge Functions have access to these secrets by default:

- `SUPABASE_URL`: The API gateway for your Supabase project.
- `SUPABASE_ANON_KEY`: The `anon` key for your Supabase API. This is safe to use in a browser when you have [Row Level Security](https://supabase.com/docs/guides/database/postgres/row-level-security) enabled.
- `SUPABASE_SERVICE_ROLE_KEY`: The `service_role` key for your Supabase API. This is safe to use in Edge Functions, but it should NEVER be used in a browser. This key will bypass [Row Level Security](https://supabase.com/docs/guides/database/postgres/row-level-security).
- `SUPABASE_DB_URL`: The URL for your [Postgres database](https://supabase.com/docs/guides/database). You can use this to connect directly to your database.

## Local secrets [\#](https://supabase.com/docs/guides/functions/secrets\#local-secrets)

You can load environment variables in two ways:

1. Through an `.env` file placed at `supabase/functions/.env`, which is automatically loaded on `supabase start`
2. Through the `--env-file` option for `supabase functions serve`, for example: `supabase functions serve --env-file ./path/to/.env-file`

Let's create a local file for storing our secrets, and inside it we can store a secret `MY_NAME`:

`
echo "MY_NAME=Yoda" >> ./supabase/.env.local
`

This creates a new file `./supabase/.env.local` for storing your local development secrets.

Never check your .env files into Git!

Now let's access this environment variable `MY_NAME` inside our Function. Anywhere in your function, add this line:

`
console.log(Deno.env.get('MY_NAME'))
`

Now we can invoke our function locally, by serving it with our new `.env.local` file:

`
supabase functions serve --env-file ./supabase/.env.local
`

When the function starts you should see the name “Yoda” output to the terminal.

## Production secrets [\#](https://supabase.com/docs/guides/functions/secrets\#production-secrets)

You will also need to set secrets for your production Edge Functions. You can do this via the Dashboard or using the CLI.

### Using the Dashboard [\#](https://supabase.com/docs/guides/functions/secrets\#using-the-dashboard)

1. Visit [Edge Function Secrets Management](https://supabase.com/dashboard/project/_/settings/functions) page in your Dashboard.
2. Add the Key and Value for your secret and press Save.
3. Note that you can paste multiple secrets at a time.

![Edge Functions Secrets Management](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fedge-functions-secrets--light.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Using the CLI [\#](https://supabase.com/docs/guides/functions/secrets\#using-the-cli)

Let's create a `.env` to help us deploy our secrets to production. In this case we'll just use the same as our local secrets:

`
cp ./supabase/.env.local ./supabase/.env
`

This creates a new file `./supabase/.env` for storing your production secrets.

Never check your `.env` files into Git! You only use the `.env` file to help deploy your secrets to production. Don't commit it to your repository.

Let's push all the secrets from the `.env` file to our remote project using [`supabase secrets set`](https://supabase.com/docs/reference/cli/usage#supabase-secrets-set):

`
supabase secrets set --env-file ./supabase/.env
# You can also set secrets individually using:
supabase secrets set MY_NAME=Chewbacca
`

You don't need to re-deploy after setting your secrets.

To see all the secrets which you have set remotely, use [`supabase secrets list`](https://supabase.com/docs/reference/cli/usage#supabase-secrets-list):

`
supabase secrets list
`

### Is this helpful?

NoYes

### On this page

[Default secrets](https://supabase.com/docs/guides/functions/secrets#default-secrets) [Local secrets](https://supabase.com/docs/guides/functions/secrets#local-secrets) [Production secrets](https://supabase.com/docs/guides/functions/secrets#production-secrets) [Using the Dashboard](https://supabase.com/docs/guides/functions/secrets#using-the-dashboard) [Using the CLI](https://supabase.com/docs/guides/functions/secrets#using-the-cli)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings