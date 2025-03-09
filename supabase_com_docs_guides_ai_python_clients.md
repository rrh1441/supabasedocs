AI & Vectors

# Choosing a Client

* * *

As described in [Structured & Unstructured Embeddings](https://supabase.com/docs/guides/ai/structured-unstructured), AI workloads come in many forms.

For data science or ephemeral workloads, the [Supabase Vecs](https://supabase.github.io/vecs/) client gets you started quickly. All you need is a connection string and vecs handles setting up your database to store and query vectors with associated metadata.

You can get your connection string from the [**Database Settings**](https://supabase.com/dashboard/project/_/settings/database) page in your dashboard. Make sure to check **Use connection pooling**, then copy the URI. Also, change the URI scheme from `postgres` to `postgresql`. `vecs` uses SQLAlchemy under the hood, which only supports `postgresql` as a dialect.

For production python applications with version controlled migrations, we recommend adding first class vector support to your toolchain by [registering the vector type with your ORM](https://github.com/pgvector/pgvector-python). pgvector provides bindings for the most commonly used SQL drivers/libraries including Django, SQLAlchemy, SQLModel, psycopg, asyncpg and Peewee.

### Is this helpful?

NoYes

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings