Database

# Connecting to Metabase

* * *

[`Metabase`](https://www.metabase.com/) is an Open Source data visualization tool. You can use it to explore your data stored in Supabase.

1

### Register

Create a [Metabase account](https://store.metabase.com/checkout) or deploy locally with [Docker](https://www.docker.com/products/docker-desktop/)

Deploying with Docker:

`
docker pull metabase/metabase:latest
`

Then run:

`
docker run -d -p 3000:3000 --name metabase metabase/metabase
`

The server should be available at [`http://localhost:3000/setup`](http://localhost:3000/setup)

2

### Connect to Postgres

Connect your Postgres server to Metabase.

- On your project dashboard click on [Connect](https://supabase.com/dashboard/project/_?showConnect=true)
- View parameters under Direct Connection
- Enter your database credentials into Metabase

Example credentials:
![Name Postgres Server.](https://supabase.com/docs/img/guides/database/connecting-to-postgres/metabase/add-pg-server.png)

3

### Explore

Explore your data in Metabase

![explore data](https://supabase.com/docs/img/guides/database/connecting-to-postgres/metabase/explore.png)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings