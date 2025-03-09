AI & Vectors

# Video Search with Mixpeek Multimodal Embeddings

## Implement video search with the Mixpeek Multimodal Embed API and Supabase Vector.

* * *

The [Mixpeek Embed API](https://docs.mixpeek.com/api-documentation/inference/embed) allows you to generate embeddings for various types of content, including videos and text. You can use these embeddings for:

- Text-to-Video / Video-To-Text / Video-to-Video / Text-to-Text Search
- Fine-tuning on your own video and text data

This guide demonstrates how to implement video search using Mixpeek Embed for video processing and embedding, and Supabase Vector for storing and querying embeddings.

You can find the full application code as a Python Poetry project on [GitHub](https://github.com/yourusername/your-repo-name).

## Create a new Python project with Poetry [\#](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search\#create-a-new-python-project-with-poetry)

[Poetry](https://python-poetry.org/) provides packaging and dependency management for Python. If you haven't already, install poetry via pip:

`
pip install poetry
`

Then initialize a new project:

`
poetry new video-search
`

## Setup Supabase project [\#](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search\#setup-supabase-project)

If you haven't already, [install the Supabase CLI](https://supabase.com/docs/guides/cli), then initialize Supabase in the root of your newly created poetry project:

`
supabase init
`

Next, start your local Supabase stack:

`
supabase start
`

This will start up the Supabase stack locally and print out a bunch of environment details, including your local `DB URL`. Make a note of that for later use.

## Install the dependencies [\#](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search\#install-the-dependencies)

Add the following dependencies to your project:

- [`supabase`](https://github.com/supabase-community/supabase-py): Supabase Python Client
- [`mixpeek`](https://github.com/mixpeek/python-client): Mixpeek Python Client for embedding generation

`
poetry add supabase mixpeek
`

## Import the necessary dependencies [\#](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search\#import-the-necessary-dependencies)

At the top of your main Python script, import the dependencies and store your environment variables:

`
from supabase import create_client, Client
from mixpeek import Mixpeek
import os
SUPABASE_URL = os.getenv("SUPABASE_URL")
SUPABASE_KEY = os.getenv("SUPABASE_API_KEY")
MIXPEEK_API_KEY = os.getenv("MIXPEEK_API_KEY")
`

## Create embeddings for your videos [\#](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search\#create-embeddings-for-your-videos)

Next, create a `seed` method, which will create a new Supabase table, generate embeddings for your video chunks, and insert the embeddings into your database:

`
def seed():
    # Initialize Supabase and Mixpeek clients
    supabase: Client = create_client(SUPABASE_URL, SUPABASE_KEY)
    mixpeek = Mixpeek(MIXPEEK_API_KEY)
    # Create a table for storing video chunk embeddings
    supabase.table("video_chunks").create({
        "id": "text",
        "start_time": "float8",
        "end_time": "float8",
        "embedding": "vector(768)",
        "metadata": "jsonb"
    })
    # Process and embed video
    video_url = "https://example.com/your_video.mp4"
    processed_chunks = mixpeek.tools.video.process(
        video_source=video_url,
        chunk_interval=1,  # 1 second intervals
        resolution=[720, 1280]
    )
    for chunk in processed_chunks:
        print(f"Processing video chunk: {chunk['start_time']}")
        # Generate embedding using Mixpeek
        embed_response = mixpeek.embed.video(
            model_id="vuse-generic-v1",
            input=chunk['base64_chunk'],
            input_type="base64"
        )
        # Insert into Supabase
        supabase.table("video_chunks").insert({
            "id": f"chunk_{chunk['start_time']}",
            "start_time": chunk["start_time"],
            "end_time": chunk["end_time"],
            "embedding": embed_response['embedding'],
            "metadata": {"video_url": video_url}
        }).execute()
    print("Video processed and embeddings inserted")
    # Create index for fast search performance
    supabase.query("CREATE INDEX ON video_chunks USING ivfflat (embedding vector_cosine_ops) WITH (lists = 100)").execute()
    print("Created index")
`

Add this method as a script in your `pyproject.toml` file:

`
[tool.poetry.scripts]
seed = "video_search.main:seed"
search = "video_search.main:search"
`

After activating the virtual environment with `poetry shell`, you can now run your seed script via `poetry run seed`. You can inspect the generated embeddings in your local database by visiting the local Supabase dashboard at [localhost:54323](http://localhost:54323/project/default/editor).

## Perform a video search from a text query [\#](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search\#perform-a-video-search-from-a-text-query)

With Supabase Vector, you can query your embeddings. You can use either a video clip as search input or alternatively, you can generate an embedding from a string input and use that as the query input:

`
def search():
    # Initialize Supabase and Mixpeek clients
    supabase: Client = create_client(SUPABASE_URL, SUPABASE_KEY)
    mixpeek = Mixpeek(MIXPEEK_API_KEY)
    # Generate embedding for text query
    query_string = "a car chase scene"
    text_emb = mixpeek.embed.video(
        model_id="vuse-generic-v1",
        input=query_string,
        input_type="text"
    )
    # Query the collection
    results = supabase.rpc(
        'match_video_chunks',
        {
            'query_embedding': text_emb['embedding'],
            'match_threshold': 0.8,
            'match_count': 5
        }
    ).execute()
    # Display the results
    if results.data:
        for result in results.data:
            print(f"Matched chunk from {result['start_time']} to {result['end_time']} seconds")
            print(f"Video URL: {result['metadata']['video_url']}")
            print(f"Similarity: {result['similarity']}")
            print("---")
    else:
        print("No matching video chunks found")
`

This query will return the top 5 most similar video chunks from your database.

You can now test it out by running `poetry run search`, and you will be presented with the most relevant video chunks to the query "a car chase scene".

## Conclusion [\#](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search\#conclusion)

With just a couple of Python scripts, you are able to implement video search as well as reverse video search using Mixpeek Embed and Supabase Vector. This approach allows for powerful semantic search capabilities that can be integrated into various applications, enabling you to search through video content using both text and video queries.

### Is this helpful?

NoYes

### On this page

[Create a new Python project with Poetry](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search#create-a-new-python-project-with-poetry) [Setup Supabase project](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search#setup-supabase-project) [Install the dependencies](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search#install-the-dependencies) [Import the necessary dependencies](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search#import-the-necessary-dependencies) [Create embeddings for your videos](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search#create-embeddings-for-your-videos) [Perform a video search from a text query](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search#perform-a-video-search-from-a-text-query) [Conclusion](https://supabase.com/docs/guides/ai/examples/mixpeek-video-search#conclusion)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings