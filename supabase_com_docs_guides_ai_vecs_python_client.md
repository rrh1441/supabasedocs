AI & Vectors

# Python client

## Manage unstructured vector stores in PostgreSQL.

* * *

Supabase provides a Python client called [`vecs`](https://github.com/supabase/vecs) for managing unstructured vector stores. This client provides a set of useful tools for creating and querying collections in Postgres using the [pgvector](https://supabase.com/docs/guides/database/extensions/pgvector) extension.

## Quick start [\#](https://supabase.com/docs/guides/ai/vecs-python-client\#quick-start)

Let's see how Vecs works using a local database. Make sure you have the Supabase CLI [installed](https://supabase.com/docs/guides/cli#installation) on your machine.

### Initialize your project [\#](https://supabase.com/docs/guides/ai/vecs-python-client\#initialize-your-project)

Start a local Postgres instance in any folder using the `init` and `start` commands. Make sure you have Docker running!

`
# Initialize your project
supabase init
# Start Postgres
supabase start
`

### Create a collection [\#](https://supabase.com/docs/guides/ai/vecs-python-client\#create-a-collection)

Inside a Python shell, run the following commands to create a new collection called "docs", with 3 dimensions.

`
import vecs
# create vector store client
vx = vecs.create_client("postgresql://postgres:postgres@localhost:54322/postgres")
# create a collection of vectors with 3 dimensions
docs = vx.get_or_create_collection(name="docs", dimension=3)
`

### Add embeddings [\#](https://supabase.com/docs/guides/ai/vecs-python-client\#add-embeddings)

Now we can insert some embeddings into our "docs" collection using the `upsert()` command:

`
import vecs
# create vector store client
docs = vecs.get_or_create_collection(name="docs", dimension=3)
# a collection of vectors with 3 dimensions
vectors=[\
("vec0", [0.1, 0.2, 0.3], {"year": 1973}),\
("vec1", [0.7, 0.8, 0.9], {"year": 2012})\
]
# insert our vectors
docs.upsert(vectors=vectors)
`

### Query the collection [\#](https://supabase.com/docs/guides/ai/vecs-python-client\#query-the-collection)

You can now query the collection to retrieve a relevant match:

`
import vecs
docs = vecs.get_or_create_collection(name="docs", dimension=3)
# query the collection filtering metadata for "year" = 2012
docs.query(
    data=[0.4,0.5,0.6],      # required
    limit=1,                         # number of records to return
    filters={"year": {"$eq": 2012}}, # metadata filters
)
`

## Deep dive [\#](https://supabase.com/docs/guides/ai/vecs-python-client\#deep-dive)

For a more in-depth guide on `vecs` collections, see [API](https://supabase.com/docs/guides/ai/python/api).

## Resources [\#](https://supabase.com/docs/guides/ai/vecs-python-client\#resources)

- Official Vecs Documentation: [https://supabase.github.io/vecs/api](https://supabase.github.io/vecs/api)
- Source Code: [https://github.com/supabase/vecs](https://github.com/supabase/vecs)

### Is this helpful?

NoYes

### On this page

[Quick start](https://supabase.com/docs/guides/ai/vecs-python-client#quick-start) [Initialize your project](https://supabase.com/docs/guides/ai/vecs-python-client#initialize-your-project) [Create a collection](https://supabase.com/docs/guides/ai/vecs-python-client#create-a-collection) [Add embeddings](https://supabase.com/docs/guides/ai/vecs-python-client#add-embeddings) [Query the collection](https://supabase.com/docs/guides/ai/vecs-python-client#query-the-collection) [Deep dive](https://supabase.com/docs/guides/ai/vecs-python-client#deep-dive) [Resources](https://supabase.com/docs/guides/ai/vecs-python-client#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings