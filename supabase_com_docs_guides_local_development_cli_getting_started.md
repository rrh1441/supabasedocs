Local Development

# Supabase CLI

## Develop locally, deploy to the Supabase Platform, and set up CI/CD workflows

* * *

The Supabase CLI enables you to run the entire Supabase stack locally, on your machine or in a CI environment. With just two commands, you can set up and start a new local project:

1. `supabase init` to create a new local project
2. `supabase start` to launch the Supabase services

## Installing the Supabase CLI [\#](https://supabase.com/docs/guides/local-development/cli/getting-started\#installing-the-supabase-cli)

macOSWindowsLinuxnpm / Bun

Install the CLI with [Homebrew](https://brew.sh/):

`
brew install supabase/tap/supabase
`

## Updating the Supabase CLI [\#](https://supabase.com/docs/guides/local-development/cli/getting-started\#updating-the-supabase-cli)

When a new [version](https://github.com/supabase/cli/releases) is released, you can update the CLI using the same methods.

macOSWindowsLinuxnpm / Bun

`
brew upgrade supabase
`

If you have any Supabase containers running locally, stop them and delete their data volumes before proceeding with the upgrade. This ensures that Supabase managed services can apply new migrations on a clean state of the local database.

##### Backup and stop running containers

Remember to save any local schema and data changes before stopping because the `--no-backup` flag will delete them.

`
supabase db diff -f my_schema
supabase db dump --local --data-only > supabase/seed.sql
supabase stop --no-backup
`

## Running Supabase locally [\#](https://supabase.com/docs/guides/local-development/cli/getting-started\#running-supabase-locally)

The Supabase CLI uses Docker containers to manage the local development stack. Follow the official guide to install and configure [Docker Desktop](https://docs.docker.com/desktop):

macOSWindows

![Docker settings on Mac: Select Integrated, Virtualization Framework, and osxfs](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fcli%2Fdocker-mac-light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

Alternately, you can use a different container tool that offers Docker compatible APIs.

- [Rancher Desktop](https://rancherdesktop.io/) (macOS, Windows, Linux)
- [Podman](https://podman.io/) (macOS, Windows, Linux)
- [OrbStack](https://orbstack.dev/) (macOS)
- [colima](https://github.com/abiosoft/colima) (macOS)

Inside the folder where you want to create your project, run:

`
supabase init
`

This will create a new `supabase` folder. It's safe to commit this folder to your version control system.

Now, to start the Supabase stack, run:

`
supabase start
`

This takes time on your first run because the CLI needs to download the Docker images to your local machine. The CLI includes the entire Supabase toolset, and a few additional images that are useful for local development (like a local SMTP server and a database diff tool).

## Access your project's services [\#](https://supabase.com/docs/guides/local-development/cli/getting-started\#access-your-projects-services)

Once all of the Supabase services are running, you'll see output containing your local Supabase credentials. It should look like this, with urls and keys that you'll use in your local project:

`
Started supabase local development setup.
         API URL: http://localhost:54321
          DB URL: postgresql://postgres:postgres@localhost:54322/postgres
      Studio URL: http://localhost:54323
    Inbucket URL: http://localhost:54324
        anon key: eyJh......
service_role key: eyJh......
`

StudioPostgresAPI GatewayAnalytics

`
# Default URL:
http://localhost:54323
`

The local development environment includes Supabase Studio, a graphical interface for working with your database.

![Local Studio](https://supabase.com/docs/img/guides/cli/local-studio.png)

## Stopping local services [\#](https://supabase.com/docs/guides/local-development/cli/getting-started\#stopping-local-services)

When you are finished working on your Supabase project, you can stop the stack (without resetting your local database):

`
supabase stop
`

## Learn more [\#](https://supabase.com/docs/guides/local-development/cli/getting-started\#learn-more)

- [CLI configuration](https://supabase.com/docs/guides/local-development/cli/config)
- [CLI reference](https://supabase.com/docs/reference/cli)

### Is this helpful?

NoYes

### On this page

[Installing the Supabase CLI](https://supabase.com/docs/guides/local-development/cli/getting-started#installing-the-supabase-cli) [Updating the Supabase CLI](https://supabase.com/docs/guides/local-development/cli/getting-started#updating-the-supabase-cli) [Running Supabase locally](https://supabase.com/docs/guides/local-development/cli/getting-started#running-supabase-locally) [Access your project's services](https://supabase.com/docs/guides/local-development/cli/getting-started#access-your-projects-services) [Stopping local services](https://supabase.com/docs/guides/local-development/cli/getting-started#stopping-local-services) [Learn more](https://supabase.com/docs/guides/local-development/cli/getting-started#learn-more)

![Docker settings on Mac: Select Integrated, Virtualization Framework, and osxfs](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fcli%2Fdocker-mac-light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings