Self-Hosting

# Self-Hosting with Docker

## Learn how to configure and deploy Supabase with Docker.

* * *

Docker is the easiest way to get started with self-hosted Supabase. It should only take you a few minutes to get up and running. This guide assumes you are running the command from the machine you intend to host from.

## Contents [\#](https://supabase.com/docs/guides/self-hosting/docker\#contents)

1. [Before you begin](https://supabase.com/docs/guides/self-hosting/docker#before-you-begin)
2. [Installing and running Supabase](https://supabase.com/docs/guides/self-hosting/docker#installing-and-running-supabase)
3. [Accessing your services](https://supabase.com/docs/guides/self-hosting/docker#accessing-supabase-studio)
4. [Updating your services](https://supabase.com/docs/guides/self-hosting/docker#updating-your-services)
5. [Securing your services](https://supabase.com/docs/guides/self-hosting/docker#securing-your-services)

## Before you begin [\#](https://supabase.com/docs/guides/self-hosting/docker\#before-you-begin)

You need the following installed in your system: [Git](https://git-scm.com/downloads) and Docker ( [Windows](https://docs.docker.com/desktop/install/windows-install/), [macOS](https://docs.docker.com/desktop/install/mac-install/), or [Linux](https://docs.docker.com/desktop/install/linux-install/)).

## Installing and running Supabase [\#](https://supabase.com/docs/guides/self-hosting/docker\#installing-and-running-supabase)

Follow these steps to start Supabase on your machine:

GeneralAdvanced

`
# Get the code
git clone --depth 1 https://github.com/supabase/supabase
# Go to the docker folder
cd supabase/docker
# Copy the fake env vars
cp .env.example .env
# Pull the latest images
docker compose pull
# Start the services (in detached mode)
docker compose up -d
`

If you are using rootless docker, edit `.env` and set `DOCKER_SOCKET_LOCATION` to your docker socket location. For example: `/run/user/1000/docker.sock`. Otherwise, you will see an error like `container supabase-vector exited (0)`.

After all the services have started you can see them running in the background:

`
docker compose ps
`

All of the services should have a status `running (healthy)`. If you see a status like `created` but not `running`, try starting that service manually with `docker compose start <service-name>`.

Your app is now running with default credentials.
[Secure your services](https://supabase.com/docs/guides/self-hosting/docker#securing-your-services) as soon as possible using the instructions below.

### Accessing Supabase Studio [\#](https://supabase.com/docs/guides/self-hosting/docker\#accessing-supabase-studio)

You can access Supabase Studio through the API gateway on port `8000`. For example: `http://<your-ip>:8000`, or [localhost:8000](http://localhost:8000/) if you are running Docker locally.

You will be prompted for a username and password. By default, the credentials are:

- Username: `supabase`
- Password: `this_password_is_insecure_and_should_be_updated`

You should change these credentials as soon as possible using the [instructions](https://supabase.com/docs/guides/self-hosting/docker#dashboard-authentication) below.

### Accessing the APIs [\#](https://supabase.com/docs/guides/self-hosting/docker\#accessing-the-apis)

Each of the APIs are available through the same API gateway:

- REST: `http://<your-ip>:8000/rest/v1/`
- Auth: `http://<your-domain>:8000/auth/v1/`
- Storage: `http://<your-domain>:8000/storage/v1/`
- Realtime: `http://<your-domain>:8000/realtime/v1/`

### Accessing your Edge Functions [\#](https://supabase.com/docs/guides/self-hosting/docker\#accessing-your-edge-functions)

Edge Functions are stored in `volumes/functions`. The default setup has a `hello` Function that you can invoke on `http://<your-domain>:8000/functions/v1/hello`.

You can add new Functions as `volumes/functions/<FUNCTION_NAME>/index.ts`. Restart the `functions` service to pick up the changes: `docker compose restart functions --no-deps`

### Accessing Postgres [\#](https://supabase.com/docs/guides/self-hosting/docker\#accessing-postgres)

By default, the Supabase stack runs the [Supavisor](https://supabase.github.io/supavisor/development/docs/) connection pooler. Supavisor provides efficient management of database connections.

You can connect to the Postgres database using the following methods:

1. For session-based connections (equivalent to direct Postgres connections):

`
psql 'postgres://postgres.your-tenant-id:your-super-secret-and-long-postgres-password@localhost:5432/postgres'
`

2. For pooled transactional connections:

`
psql 'postgres://postgres.your-tenant-id:your-super-secret-and-long-postgres-password@localhost:6543/postgres'
`

The default tenant ID is `your-tenant-id`, and the default password is `your-super-secret-and-long-postgres-password`. You should change these as soon as possible using the [instructions below](https://supabase.com/docs/guides/self-hosting/docker#update-secrets).

By default, the database is not accessible from outside the local machine but the pooler is. You can [change this](https://supabase.com/docs/guides/self-hosting/docker#exposing-your-postgres-database) by updating the `docker-compose.yml` file.

## Updating your services [\#](https://supabase.com/docs/guides/self-hosting/docker\#updating-your-services)

For security reasons, we "pin" the versions of each service in the docker-compose file (these versions are updated ~monthly). If you want to update any services immediately, you can do so by updating the version number in the docker compose file and then running `docker compose pull`. You can find all the latest docker images in the [Supabase Docker Hub](https://hub.docker.com/u/supabase).

You should update your services frequently to get the latest features and bug fixes and security patches. Note that you will need to restart the services to pick up the changes, which will result in some downtime for your services.

**Example**
You'll want to update the Studio(Dashboard) frequently to get the latest features and bug fixes. To update the Dashboard:

1. Visit the [supabase/studio](https://hub.docker.com/r/supabase/studio/tags) image in the [Supabase Docker Hub](https://hub.docker.com/u/supabase)
2. Find the latest version (tag) number. It will look something like `20241029-46e1e40`
3. Update the `image` field in the `docker-compose.yml` file to the new version. It should look like this: `image: supabase/studio:20241028-a265374`
4. Run `docker compose pull` and then `docker compose up -d` to restart the service with the new version.

## Securing your services [\#](https://supabase.com/docs/guides/self-hosting/docker\#securing-your-services)

While we provided you with some example secrets for getting started, you should NEVER deploy your Supabase setup using the defaults we have provided. Follow all of the steps in this section to ensure you have a secure setup, and then [restart all services](https://supabase.com/docs/guides/self-hosting/docker#restarting-all-services) to pick up the changes.

### Generate API keys [\#](https://supabase.com/docs/guides/self-hosting/docker\#generate-api-keys)

We need to generate secure keys for accessing your services. We'll use the `JWT Secret` to generate `anon` and `service` API keys using the form below.

1. **Obtain a Secret**: Use the 40-character secret provided, or create your own. If creating, ensure it's a strong, random string of 40 characters.
2. **Store Securely**: Save the secret in a secure location on your local machine. Don't share this secret publicly or commit it to version control.
3. **Generate a JWT**: Use the form below to generate a new `JWT` using your secret.

JWT Secret:

Key:

ANON\_KEYSERVICE\_KEY

The JWT will be generated from this info:

Generate JWT

### Update API keys [\#](https://supabase.com/docs/guides/self-hosting/docker\#update-api-keys)

Run this form twice to generate new `anon` and `service` API keys. Replace the values in the `./docker/.env` file:

- `ANON_KEY` \- replace with an `anon` key
- `SERVICE_ROLE_KEY` \- replace with a `service` key

You will need to [restart](https://supabase.com/docs/guides/self-hosting/docker#restarting-all-services) the services for the changes to take effect.

### Update secrets [\#](https://supabase.com/docs/guides/self-hosting/docker\#update-secrets)

Update the `./docker/.env` file with your own secrets. In particular, these are required:

- `POSTGRES_PASSWORD`: the password for the `postgres` role.
- `JWT_SECRET`: used by PostgREST and GoTrue, among others.
- `SITE_URL`: the base URL of your site.
- `SMTP_*`: mail server credentials. You can use any SMTP server.
- `POOLER_TENANT_ID`: the tenant-id that will be used by Supavisor pooler for your connection string

You will need to [restart](https://supabase.com/docs/guides/self-hosting/docker#restarting-all-services) the services for the changes to take effect.

### Dashboard authentication [\#](https://supabase.com/docs/guides/self-hosting/docker\#dashboard-authentication)

The Dashboard is protected with basic authentication. The default user and password MUST be updated before using Supabase in production.
Update the following values in the `./docker/.env` file:

- `DASHBOARD_USERNAME`: The default username for the Dashboard
- `DASHBOARD_PASSWORD`: The default password for the Dashboard

You can also add more credentials for multiple users in `./docker/volumes/api/kong.yml`. For example:

docker/volumes/api/kong.yml

`
basicauth_credentials:
  - consumer: DASHBOARD
    username: user_one
    password: password_one
  - consumer: DASHBOARD
    username: user_two
    password: password_two
`

To enable all dashboard features outside of `localhost`, update the following value in the `./docker/.env` file:

- `SUPABASE_PUBLIC_URL`: The URL or IP used to access the dashboard

You will need to [restart](https://supabase.com/docs/guides/self-hosting/docker#restarting-all-services) the services for the changes to take effect.

## Restarting all services [\#](https://supabase.com/docs/guides/self-hosting/docker\#restarting-all-services)

You can restart services to pick up any configuration changes by running:

`
# Stop and remove the containers
docker compose down
# Recreate and start the containers
docker compose up -d
`

Be aware that this will result in downtime. Simply restarting the services does not apply configuration changes.

## Stopping all services [\#](https://supabase.com/docs/guides/self-hosting/docker\#stopping-all-services)

You can stop Supabase by running `docker compose stop` in same directory as your `docker-compose.yml` file.

## Uninstalling [\#](https://supabase.com/docs/guides/self-hosting/docker\#uninstalling)

You can stop Supabase by running the following in same directory as your `docker-compose.yml` file:

`
# Stop docker and remove volumes:
docker compose down -v
# Remove Postgres data:
rm -rf volumes/db/data/
`

This will destroy all data in the database and storage volumes, so be careful!

## Managing your secrets [\#](https://supabase.com/docs/guides/self-hosting/docker\#managing-your-secrets)

Many components inside Supabase use secure secrets and passwords. These are listed in the self-hosting [env file](https://github.com/supabase/supabase/blob/master/docker/.env.example), but we strongly recommend using a secrets manager when deploying to production. Plain text files like dotenv lead to accidental costly leaks.

Some suggested systems include:

- [Doppler](https://www.doppler.com/)
- [Infisical](https://infisical.com/)
- [Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/general/overview) by Azure (Microsoft)
- [Secrets Manager](https://aws.amazon.com/secrets-manager/) by AWS
- [Secrets Manager](https://cloud.google.com/secret-manager) by GCP
- [Vault](https://www.hashicorp.com/products/vault) by HashiCorp

## Advanced [\#](https://supabase.com/docs/guides/self-hosting/docker\#advanced)

Everything beyond this point in the guide helps you understand how the system works and how you can modify it to suit your needs.

### Architecture [\#](https://supabase.com/docs/guides/self-hosting/docker\#architecture)

Supabase is a combination of open source tools, each specifically chosen for Enterprise-readiness.

If the tools and communities already exist, with an MIT, Apache 2, or equivalent open license, we will use and support that tool.
If the tool doesn't exist, we build and open source it ourselves.

![Diagram showing the architecture of Supabase. The Kong API gateway sits in front of 7 services: GoTrue, PostgREST, Realtime, Storage, pg_meta, Functions, and pg_graphql. All the services talk to a single Postgres instance.](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fsupabase-architecture--light.svg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

- [Kong](https://github.com/Kong/kong) is a cloud-native API gateway.
- [GoTrue](https://github.com/supabase/gotrue) is an JWT based API for managing users and issuing JWT tokens.
- [PostgREST](http://postgrest.org/) is a web server that turns your Postgres database directly into a RESTful API
- [Realtime](https://github.com/supabase/realtime) is an Elixir server that allows you to listen to Postgres inserts, updates, and deletes using WebSockets. Realtime polls Postgres' built-in replication functionality for database changes, converts changes to JSON, then broadcasts the JSON over WebSockets to authorized clients.
- [Storage](https://github.com/supabase/storage-api) provides a RESTful interface for managing Files stored in S3, using Postgres to manage permissions.
- [`postgres-meta`](https://github.com/supabase/postgres-meta) is a RESTful API for managing your Postgres, allowing you to fetch tables, add roles, and run queries, etc.
- [Postgres](https://www.postgresql.org/) is an object-relational database system with over 30 years of active development that has earned it a strong reputation for reliability, feature robustness, and performance.
- [Supavisor](https://github.com/supabase/supavisor) is a scalable connection pooler for Postgres, allowing for efficient management of database connections.

For the system to work cohesively, some services require additional configuration within the Postgres database. For example, the APIs and Auth system require several [default roles](https://supabase.com/docs/guides/database/postgres/roles) and the `pgjwt` Postgres extension.

You can find all the default extensions inside the [schema migration scripts repo](https://github.com/supabase/postgres/tree/develop/migrations). These scripts are mounted at `/docker-entrypoint-initdb.d` to run automatically when starting the database container.

### Configuring services [\#](https://supabase.com/docs/guides/self-hosting/docker\#configuring-services)

Each system has a number of configuration options which can be found in the relevant product documentation.

- [Postgres](https://hub.docker.com/_/postgres/)
- [PostgREST](https://postgrest.org/en/stable/configuration.html)
- [Realtime](https://github.com/supabase/realtime#server)
- [Auth](https://github.com/supabase/auth)
- [Storage](https://github.com/supabase/storage-api)
- [Kong](https://docs.konghq.com/gateway/latest/install/docker/)
- [Supavisor](https://supabase.github.io/supavisor/development/docs/)

These configuration items are generally added to the `env` section of each service, inside the `docker-compose.yml` section. If these configuration items are sensitive, they should be stored in a [secret manager](https://supabase.com/docs/guides/self-hosting#managing-your-secrets) or using an `.env` file and then referenced using the `${}` syntax.

docker-compose.yml

.env

`
services:
rest:
    image: postgrest/postgrest
    environment:
      PGRST_JWT_SECRET: ${JWT_SECRET}
`

### Common configuration [\#](https://supabase.com/docs/guides/self-hosting/docker\#common-configuration)

Each system can be [configured](https://supabase.com/docs/guides/self-hosting#configuration) independently. Some of the most common configuration options are listed below.

#### Configuring an email server [\#](https://supabase.com/docs/guides/self-hosting/docker\#configuring-an-email-server)

You will need to use a production-ready SMTP server for sending emails. You can configure the SMTP server by updating the following environment variables:

.env

`
SMTP_ADMIN_EMAIL=
SMTP_HOST=
SMTP_PORT=
SMTP_USER=
SMTP_PASS=
SMTP_SENDER_NAME=
`

We recommend using [AWS SES](https://aws.amazon.com/ses/). It's extremely cheap and reliable. Restart all services to pick up the new configuration.

#### Configuring S3 Storage [\#](https://supabase.com/docs/guides/self-hosting/docker\#configuring-s3-storage)

By default all files are stored locally on the server. You can configure the Storage service to use S3 by updating the following environment variables:

docker-compose.yml

`
storage:
environment: STORAGE_BACKEND=s3
    GLOBAL_S3_BUCKET=name-of-your-s3-bucket
    REGION=region-of-your-s3-bucket
`

You can find all the available options in the [storage repository](https://github.com/supabase/storage-api/blob/master/.env.sample). Restart the `storage` service to pick up the changes: `docker compose restart storage --no-deps`

#### Configuring Supabase AI Assistant [\#](https://supabase.com/docs/guides/self-hosting/docker\#configuring-supabase-ai-assistant)

Configuring the Supabase AI Assistant is optional. By adding your own `OPENAI_API_KEY`, you can enable AI services, which help with writing SQL queries, statements, and policies.

docker-compose.yml

.env

`
services:
studio:
    image: supabase/studio
    environment:
      OPENAI_API_KEY: ${OPENAI_API_KEY:-}
`

#### Setting database's `log_min_messages` [\#](https://supabase.com/docs/guides/self-hosting/docker\#setting-databases-logminmessages)

By default, `docker compose` sets the database's `log_min_messages` configuration to `fatal` to prevent redundant logs generated by Realtime. You can configure `log_min_messages` using any of the Postgres [Severity Levels](https://www.postgresql.org/docs/current/runtime-config-logging.html#RUNTIME-CONFIG-SEVERITY-LEVELS).

#### Accessing Postgres through Supavisor [\#](https://supabase.com/docs/guides/self-hosting/docker\#accessing-postgres-through-supavisor)

By default, the Postgres database is accessible through the Supavisor connection pooler. This allows for more efficient management of database connections. You can connect to the pooled database using the `POOLER_PROXY_PORT_TRANSACTION` port and `POSTGRES_PORT` for session based connections.

For more information on configuring and using Supavisor, see the [Supavisor documentation](https://supabase.github.io/supavisor/).

#### Exposing your Postgres database [\#](https://supabase.com/docs/guides/self-hosting/docker\#exposing-your-postgres-database)

If you need direct access to the Postgres database without going through Supavisor, you can expose it by updating the `docker-compose.yml` file:

docker-compose.yml

`
# Comment or remove the supavisor section of the docker-compose file
#  supavisor:
#    ports:
# ...
db:
ports:
    - ${POSTGRES_PORT}:${POSTGRES_PORT}
`

This is less-secure, so make sure you are running a firewall in front of your server.

#### File storage backend on macOS [\#](https://supabase.com/docs/guides/self-hosting/docker\#file-storage-backend-on-macos)

By default, Storage backend is set to `file`, which is to use local files as the storage backend. For macOS compatibility, you need to choose `VirtioFS` as the Docker container file sharing implementation (in Docker Desktop -> Preferences -> General).

#### Setting up logging with the Analytics server [\#](https://supabase.com/docs/guides/self-hosting/docker\#setting-up-logging-with-the-analytics-server)

Additional configuration is required for self-hosting the Analytics server. For the full setup instructions, see [Self Hosting Analytics](https://supabase.com/docs/reference/self-hosting-analytics/introduction#getting-started).

### Upgrading Analytics [\#](https://supabase.com/docs/guides/self-hosting/docker\#upgrading-analytics)

Due to the changes in the Analytics server, you will need to run the following commands to upgrade your Analytics server:

All data in analytics will be deleted when you run the commands below.

`
### Destroy analytics to transition to postgres self hosted solution without other data loss
# Enter the container and use your .env POSTGRES_PASSWORD value to login
docker exec -it $(docker ps | grep supabase-db | awk '{print $1}') psql -U supabase_admin --password
# Drop all the data in the _analytics schema
DROP PUBLICATION logflare_pub; DROP SCHEMA _analytics CASCADE; CREATE SCHEMA _analytics;\q
# Drop the analytics container
docker rm supabase-analytics
`

* * *

## Demo [\#](https://supabase.com/docs/guides/self-hosting/docker\#demo)

A minimal setup working on Ubuntu, hosted on DigitalOcean.

Self-hosting Supabase on Ubuntu with Digital Ocean - YouTube

Supabase

45.5K subscribers

[Self-hosting Supabase on Ubuntu with Digital Ocean](https://www.youtube.com/watch?v=FqiQKRKsfZE)

Supabase

Search

Info

Shopping

Tap to unmute

If playback doesn't begin shortly, try restarting your device.

You're signed out

Videos you watch may be added to the TV's watch history and influence TV recommendations. To avoid this, cancel and sign in to YouTube on your computer.

CancelConfirm

Share

Include playlist

An error occurred while retrieving sharing information. Please try again later.

Watch later

Share

Copy link

Watch on

0:00

/ •Live

•

[Watch on YouTube](https://www.youtube.com/watch?v=FqiQKRKsfZE "Watch on YouTube")

### Demo using DigitalOcean [\#](https://supabase.com/docs/guides/self-hosting/docker\#demo-using-digitalocean)

1. A DigitalOcean Droplet with 1 GB memory and 25 GB solid-state drive (SSD) is sufficient to start
2. To access the Dashboard, use the ipv4 IP address of your Droplet.
3. If you're unable to access Dashboard, run `docker compose ps` to see if the Studio service is running and healthy.

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2FFqiQKRKsfZE%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Contents](https://supabase.com/docs/guides/self-hosting/docker#contents) [Before you begin](https://supabase.com/docs/guides/self-hosting/docker#before-you-begin) [Installing and running Supabase](https://supabase.com/docs/guides/self-hosting/docker#installing-and-running-supabase) [Accessing Supabase Studio](https://supabase.com/docs/guides/self-hosting/docker#accessing-supabase-studio) [Accessing the APIs](https://supabase.com/docs/guides/self-hosting/docker#accessing-the-apis) [Accessing your Edge Functions](https://supabase.com/docs/guides/self-hosting/docker#accessing-your-edge-functions) [Accessing Postgres](https://supabase.com/docs/guides/self-hosting/docker#accessing-postgres) [Updating your services](https://supabase.com/docs/guides/self-hosting/docker#updating-your-services) [Securing your services](https://supabase.com/docs/guides/self-hosting/docker#securing-your-services) [Generate API keys](https://supabase.com/docs/guides/self-hosting/docker#generate-api-keys) [Update API keys](https://supabase.com/docs/guides/self-hosting/docker#update-api-keys) [Update secrets](https://supabase.com/docs/guides/self-hosting/docker#update-secrets) [Dashboard authentication](https://supabase.com/docs/guides/self-hosting/docker#dashboard-authentication) [Restarting all services](https://supabase.com/docs/guides/self-hosting/docker#restarting-all-services) [Stopping all services](https://supabase.com/docs/guides/self-hosting/docker#stopping-all-services) [Uninstalling](https://supabase.com/docs/guides/self-hosting/docker#uninstalling) [Managing your secrets](https://supabase.com/docs/guides/self-hosting/docker#managing-your-secrets) [Advanced](https://supabase.com/docs/guides/self-hosting/docker#advanced) [Architecture](https://supabase.com/docs/guides/self-hosting/docker#architecture) [Configuring services](https://supabase.com/docs/guides/self-hosting/docker#configuring-services) [Common configuration](https://supabase.com/docs/guides/self-hosting/docker#common-configuration) [Upgrading Analytics](https://supabase.com/docs/guides/self-hosting/docker#upgrading-analytics) [Demo](https://supabase.com/docs/guides/self-hosting/docker#demo) [Demo using DigitalOcean](https://supabase.com/docs/guides/self-hosting/docker#demo-using-digitalocean)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings