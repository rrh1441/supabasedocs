AI & Vectors

# Face similarity search

## Identify the celebrities who look most similar to you using Supabase Vecs.

* * *

This guide will walk you through a ["Face Similarity Search"](https://github.com/supabase/supabase/blob/master/examples/ai/face_similarity.ipynb) example using Colab and Supabase Vecs. You will be able to identify the celebrities who look most similar to you (or any other person). You will:

1. Launch a Postgres database that uses pgvector to store embeddings
2. Launch a notebook that connects to your database
3. Load the " `ashraq/tmdb-people-image`" celebrity dataset
4. Use the `face_recognition` model to create an embedding for every celebrity photo.
5. Search for similar faces inside the dataset.

## Project setup [\#](https://supabase.com/docs/guides/ai/quickstarts/face-similarity\#project-setup)

Let's create a new Postgres database. This is as simple as starting a new Project in Supabase:

1. [Create a new project](https://database.new/) in the Supabase dashboard.
2. Enter your project details. Remember to store your password somewhere safe.

Your database will be available in less than a minute.

**Finding your credentials:**

You can find your project credentials inside the project [settings](https://supabase.com/dashboard/project/_/settings/), including:

- [Database credentials](https://supabase.com/dashboard/project/_/settings/database): connection strings and connection pooler details.
- [API credentials](https://supabase.com/dashboard/project/_/settings/database): your serverless API URL and `anon` / `service_role` keys.

## Launching a notebook [\#](https://supabase.com/docs/guides/ai/quickstarts/face-similarity\#launching-a-notebook)

Launch our [`semantic_text_deduplication`](https://github.com/supabase/supabase/blob/master/examples/ai/face_similarity.ipynb) notebook in Colab:

[![](https://supabase.com/docs/img/ai/colab-badge.svg)](https://colab.research.google.com/github/supabase/supabase/blob/master/examples/ai/face_similarity.ipynb)

At the top of the notebook, you'll see a button `Copy to Drive`. Click this button to copy the notebook to your Google Drive.

## Connecting to your database [\#](https://supabase.com/docs/guides/ai/quickstarts/face-similarity\#connecting-to-your-database)

Inside the Notebook, find the cell which specifies the `DB_CONNECTION`. It will contain some code like this:

`
import vecs
DB_CONNECTION = "postgresql://<user>:<password>@<host>:<port>/<db_name>"
# create vector store client
vx = vecs.create_client(DB_CONNECTION)
`

Replace the `DB_CONNECTION` with your own connection string for your database. You can find the Postgres connection string in the [Database Settings](https://supabase.com/dashboard/project/_/settings/database) of your Supabase project.

SQLAlchemy requires the connection string to start with `postgresql://` (instead of `postgres://`). Don't forget to rename this after copying the string from the dashboard.

You must use the "connection pooling" string (domain ending in `*.pooler.supabase.com`) with Google Colab since Colab does not support IPv6.

## Stepping through the notebook [\#](https://supabase.com/docs/guides/ai/quickstarts/face-similarity\#stepping-through-the-notebook)

Now all that's left is to step through the notebook. You can do this by clicking the "execute" button ( `ctrl+enter`) at the top left of each code cell. The notebook guides you through the process of creating a collection, adding data to it, and querying it.

You can view the inserted items in the [Table Editor](https://supabase.com/dashboard/project/_/editor/), by selecting the `vecs` schema from the schema dropdown.

![Colab documents](https://supabase.com/docs/img/ai/google-colab/colab-documents.png)

## Next steps [\#](https://supabase.com/docs/guides/ai/quickstarts/face-similarity\#next-steps)

You can now start building your own applications with Vecs. Check our [examples](https://supabase.com/docs/guides/ai#examples) for ideas.

### Is this helpful?

NoYes

### On this page

[Project setup](https://supabase.com/docs/guides/ai/quickstarts/face-similarity#project-setup) [Launching a notebook](https://supabase.com/docs/guides/ai/quickstarts/face-similarity#launching-a-notebook) [Connecting to your database](https://supabase.com/docs/guides/ai/quickstarts/face-similarity#connecting-to-your-database) [Stepping through the notebook](https://supabase.com/docs/guides/ai/quickstarts/face-similarity#stepping-through-the-notebook) [Next steps](https://supabase.com/docs/guides/ai/quickstarts/face-similarity#next-steps)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings