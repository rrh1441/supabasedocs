Local Development

# Local Development & CLI

## Learn how to develop locally and use the Supabase CLI

* * *

Develop locally while running the Supabase stack on your machine.

## Quickstart [\#](https://supabase.com/docs/guides/local-development\#quickstart)

1. Install the Supabase CLI:



npmyarnpnpmbrew







`
npm install supabase --save-dev
`

2. In your repo, initialize the Supabase project:



npmyarnpnpmbrew







`
npx supabase init
`

3. Start the Supabase stack:



npmyarnpnpmbrew







`
npx supabase start
`

4. View your local Supabase instance at [http://localhost:54323](http://localhost:54323/).


## Local development [\#](https://supabase.com/docs/guides/local-development\#local-development)

Local development with Supabase allows you to work on your projects in a self-contained environment on your local machine. Working locally has several advantages:

1. Faster development: You can make changes and see results instantly without waiting for remote deployments.
2. Offline work: You can continue development even without an internet connection.
3. Cost-effective: Local development is free and doesn't consume your project's quota.
4. Enhanced privacy: Sensitive data remains on your local machine during development.
5. Easy testing: You can experiment with different configurations and features without affecting your production environment.

To get started with local development, you'll need to install the [Supabase CLI](https://supabase.com/docs/guides/local-development#cli) and Docker. The Supabase CLI allows you to start and manage your local Supabase stack, while Docker is used to run the necessary services.

Once set up, you can initialize a new Supabase project, start the local stack, and begin developing your application using local Supabase services. This includes access to a local Postgres database, Auth, Storage, and other Supabase features.

## CLI [\#](https://supabase.com/docs/guides/local-development\#cli)

The Supabase CLI is a powerful tool that enables developers to manage their Supabase projects directly from the terminal. It provides a suite of commands for various tasks, including:

- Setting up and managing local development environments
- Generating TypeScript types for your database schema
- Handling database migrations
- Managing environment variables and secrets
- Deploying your project to the Supabase platform

With the CLI, you can streamline your development workflow, automate repetitive tasks, and maintain consistency across different environments. It's an essential tool for both local development and CI/CD pipelines.

See the [CLI Getting Started guide](https://supabase.com/docs/guides/local-development/cli/getting-started) for more information.

### Is this helpful?

NoYes

### On this page

[Quickstart](https://supabase.com/docs/guides/local-development#quickstart) [Local development](https://supabase.com/docs/guides/local-development#local-development) [CLI](https://supabase.com/docs/guides/local-development#cli)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings