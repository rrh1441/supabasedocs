AI & Vectors

# Engineering for Scale

## Building an enterprise-grade vector architecture.

* * *

Content sources for vectors can be extremely large. As you grow you should run your Vector workloads across several secondary databases (sometimes called "pods"), which allows each collection to scale independently.

## Simple workloads [\#](https://supabase.com/docs/guides/ai/engineering-for-scale\#simple-workloads)

For small workloads, it's typical to store your data in a single database.

If you've used [Vecs](https://supabase.com/docs/guides/ai/vecs-python-client) to create 3 different collections, you can expose collections to your web or mobile application using [views](https://supabase.com/docs/guides/database/tables#views):

![single database](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fai%2Fscaling%2Fengineering-for-scale--single-database--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

For example, with 3 collections, called `docs`, `posts`, and `images`, we could expose the "docs" inside the public schema like this:

`
create view public.docs as
select
id,
embedding,
metadata, # Expose the metadata as JSON
(metadata->>'url')::text as url # Extract the URL as a string
from vector
`

You can then use any of the client libraries to access your collections within your applications:

`
const { data, error } = await supabase
.from('docs')
.select('id, embedding, metadata')
.eq('url', '/hello-world')
`

## Enterprise workloads [\#](https://supabase.com/docs/guides/ai/engineering-for-scale\#enterprise-workloads)

As you move into production, we recommend splitting your collections into separate projects. This is because it allows your vector stores to scale independently of your production data. Vectors typically grow faster than operational data, and they have different resource requirements. Running them on separate databases removes the single-point-of-failure.

![With secondaries](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fai%2Fscaling%2Fengineering-for-scale--with-secondaries--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

You can use as many secondary databases as you need to manage your collections. With this architecture, you have 2 options for accessing collections within your application:

1. Query the collections directly using Vecs.
2. Access the collections from your Primary database through a Wrapper.

You can use both of these in tandem to suit your use-case. We recommend option `1` wherever possible, as it offers the most scalability.

### Query collections using Vecs [\#](https://supabase.com/docs/guides/ai/engineering-for-scale\#query-collections-using-vecs)

Vecs provides methods for querying collections, either using a [cosine similarity function](https://supabase.github.io/vecs/api/#basic) or with [metadata filtering](https://supabase.github.io/vecs/api/#metadata-filtering).

`
# cosine similarity
docs.query(query_vector=[0.4,0.5,0.6], limit=5)
# metadata filtering
docs.query(
    query_vector=[0.4,0.5,0.6],
    limit=5,
    filters={"year": {"$eq": 2012}}, # metadata filters
)
`

### Accessing external collections using Wrappers [\#](https://supabase.com/docs/guides/ai/engineering-for-scale\#accessing-external-collections-using-wrappers)

Supabase supports [Foreign Data Wrappers](https://supabase.com/blog/postgres-foreign-data-wrappers-rust). Wrappers allow you to connect two databases together so that you can query them over the network.

This involves 2 steps: connecting to your remote database from the primary and creating a Foreign Table.

#### Connecting your remote database [\#](https://supabase.com/docs/guides/ai/engineering-for-scale\#connecting-your-remote-database)

Inside your Primary database we need to provide the credentials to access the secondary database:

`
create extension postgres_fdw;
create server docs_server
foreign data wrapper postgres_fdw
options (host 'db.xxx.supabase.co', port '5432', dbname 'postgres');
create user mapping for docs_user
server docs_server
options (user 'postgres', password 'password');
`

#### Create a foreign table [\#](https://supabase.com/docs/guides/ai/engineering-for-scale\#create-a-foreign-table)

We can now create a foreign table to access the data in our secondary project.

`
create foreign table docs (
id text not null,
embedding vector(384),
metadata jsonb,
url text
)
server docs_server
options (schema_name 'public', table_name 'docs');
`

This looks very similar to our View example above, and you can continue to use the client libraries to access your collections through the foreign table:

`
const { data, error } = await supabase
.from('docs')
.select('id, embedding, metadata')
.eq('url', '/hello-world')
`

### Enterprise architecture [\#](https://supabase.com/docs/guides/ai/engineering-for-scale\#enterprise-architecture)

This diagram provides an example architecture that allows you to access the collections either with our client libraries or using Vecs. You can add as many secondary databases as you need (in this example we only show one):

![multi database](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fai%2Fscaling%2Fengineering-for-scale--multi-database--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Simple workloads](https://supabase.com/docs/guides/ai/engineering-for-scale#simple-workloads) [Enterprise workloads](https://supabase.com/docs/guides/ai/engineering-for-scale#enterprise-workloads) [Query collections using Vecs](https://supabase.com/docs/guides/ai/engineering-for-scale#query-collections-using-vecs) [Accessing external collections using Wrappers](https://supabase.com/docs/guides/ai/engineering-for-scale#accessing-external-collections-using-wrappers) [Enterprise architecture](https://supabase.com/docs/guides/ai/engineering-for-scale#enterprise-architecture)

![single database](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fai%2Fscaling%2Fengineering-for-scale--single-database--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![With secondaries](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fai%2Fscaling%2Fengineering-for-scale--with-secondaries--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![multi database](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fai%2Fscaling%2Fengineering-for-scale--multi-database--light.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings