AI & Vectors

# Vector search with Next.js and OpenAI

## Learn how to build a ChatGPT-style doc search powered by Next.js, OpenAI, and Supabase.

* * *

While our [Headless Vector search](https://supabase.com/docs/guides/ai/examples/headless-vector-search) provides a toolkit for generative Q&A, in this tutorial we'll go more in-depth, build a custom ChatGPT-like search experience from the ground-up using Next.js. You will:

1. Convert your markdown into embeddings using OpenAI.
2. Store you embeddings in Postgres using pgvector.
3. Deploy a function for answering your users' questions.

You can read our [Supabase Clippy](https://supabase.com/blog/chatgpt-supabase-docs) blog post for a full example.

We assume that you have a Next.js project with a collection of `.mdx` files nested inside your `pages` directory. We will start developing locally with the Supabase CLI and then push our local database changes to our hosted Supabase project. You can find the [full Next.js example on GitHub](https://github.com/supabase-community/nextjs-openai-doc-search).

## Create a project [\#](https://supabase.com/docs/guides/ai/examples/nextjs-vector-search\#create-a-project)

1. [Create a new project](https://supabase.com/dashboard) in the Supabase Dashboard.
2. Enter your project details.
3. Wait for the new database to launch.

## Prepare the database [\#](https://supabase.com/docs/guides/ai/examples/nextjs-vector-search\#prepare-the-database)

Let's prepare the database schema. We can use the "OpenAI Vector Search" quickstart in the [SQL Editor](https://supabase.com/dashboard/project/_/sql), or you can copy/paste the SQL below and run it yourself.

DashboardSQL

1. Go to the [SQL Editor](https://supabase.com/dashboard/project/_/sql) page in the Dashboard.
2. Click **OpenAI Vector Search**.
3. Click **Run**.

## Pre-process the knowledge base at build time [\#](https://supabase.com/docs/guides/ai/examples/nextjs-vector-search\#pre-process-the-knowledge-base-at-build-time)

With our database set up, we need to process and store all `.mdx` files in the `pages` directory. You can find the full script [here](https://github.com/supabase-community/nextjs-openai-doc-search/blob/main/lib/generate-embeddings.ts), or follow the steps below:

1

### Generate Embeddings

Create a new file `lib/generate-embeddings.ts` and copy the code over from [GitHub](https://github.com/supabase-community/nextjs-openai-doc-search/blob/main/lib/generate-embeddings.ts).

`
curl \
https://raw.githubusercontent.com/supabase-community/nextjs-openai-doc-search/main/lib/generate-embeddings.ts \
-o "lib/generate-embeddings.ts"
`

2

### Set up environment variables

We need some environment variables to run the script. Add them to your `.env` file and make sure your `.env` file is not committed to source control!
You can get your local Supabase credentials by running `supabase status`.

`
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
# Get your key at https://platform.openai.com/account/api-keys
OPENAI_API_KEY=
`

3

### Run script at build time

Include the script in your `package.json` script commands to enable Vercel to automatically run it at build time.

`
"scripts": {
"dev": "next dev",
"build": "pnpm run embeddings && next build",
"start": "next start",
"embeddings": "tsx lib/generate-embeddings.ts"
},
`

## Create text completion with OpenAI API [\#](https://supabase.com/docs/guides/ai/examples/nextjs-vector-search\#create-text-completion-with-openai-api)

Anytime a user asks a question, we need to create an embedding for their question, perform a similarity search, and then send a text completion request to the OpenAI API with the query and then context content merged together into a prompt.

All of this is glued together in a [Vercel Edge Function](https://vercel.com/docs/concepts/functions/edge-functions), the code for which can be found on [GitHub](https://github.com/supabase-community/nextjs-openai-doc-search/blob/main/pages/api/vector-search.ts).

1

### Create Embedding for Question

In order to perform similarity search we need to turn the question into an embedding.

``
const embeddingResponse = await fetch('https://api.openai.com/v1/embeddings', {
method: 'POST',
headers: {
    Authorization: `Bearer ${openAiKey}`,
    'Content-Type': 'application/json',
},
body: JSON.stringify({
    model: 'text-embedding-ada-002',
    input: sanitizedQuery.replaceAll('\n', ' '),
}),
})
if (embeddingResponse.status !== 200) {
throw new ApplicationError('Failed to create embedding for question', embeddingResponse)
}
const {
data: [{ embedding }],
} = await embeddingResponse.json()
``

2

### Perform similarity search

Using the `embeddingResponse` we can now perform similarity search by performing an remote procedure call (RPC) to the database function we created earlier.

`
const { error: matchError, data: pageSections } = await supabaseClient.rpc(
'match_page_sections',
{
    embedding,
    match_threshold: 0.78,
    match_count: 10,
    min_content_length: 50,
}
)
`

3

### Perform text completion request

With the relevant content for the user's question identified, we can now build the prompt and make a text completion request via the OpenAI API.

If successful, the OpenAI API will respond with a `text/event-stream` response that we can forward to the client where we'll process the event stream to smoothly print the answer to the user.

``
const prompt = codeBlock`
${oneLine`
    You are a very enthusiastic Supabase representative who loves
    to help people! Given the following sections from the Supabase
    documentation, answer the question using only that information,
    outputted in markdown format. If you are unsure and the answer
    is not explicitly written in the documentation, say
    "Sorry, I don't know how to help with that."
`}
Context sections:
${contextText}
Question: """
${sanitizedQuery}
"""
Answer as markdown (including related code snippets if available):
`
const completionOptions: CreateCompletionRequest = {
model: 'gpt-3.5-turbo-instruct',
prompt,
max_tokens: 512,
temperature: 0,
stream: true,
}
const response = await fetch('https://api.openai.com/v1/completions', {
method: 'POST',
headers: {
    Authorization: `Bearer ${openAiKey}`,
    'Content-Type': 'application/json',
},
body: JSON.stringify(completionOptions),
})
if (!response.ok) {
const error = await response.json()
throw new ApplicationError('Failed to generate completion', error)
}
// Proxy the streamed SSE response from OpenAI
return new Response(response.body, {
headers: {
    'Content-Type': 'text/event-stream',
},
})
``

## Display the answer on the frontend [\#](https://supabase.com/docs/guides/ai/examples/nextjs-vector-search\#display-the-answer-on-the-frontend)

In a last step, we need to process the event stream from the OpenAI API and print the answer to the user. The full code for this can be found on [GitHub](https://github.com/supabase-community/nextjs-openai-doc-search/blob/main/components/SearchDialog.tsx).

``
const handleConfirm = React.useCallback(
async (query: string) => {
    setAnswer(undefined)
    setQuestion(query)
    setSearch('')
    dispatchPromptData({ index: promptIndex, answer: undefined, query })
    setHasError(false)
    setIsLoading(true)
    const eventSource = new SSE(`api/vector-search`, {
      headers: {
        apikey: process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY ?? '',
        Authorization: `Bearer ${process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY}`,
        'Content-Type': 'application/json',
      },
      payload: JSON.stringify({ query }),
    })
    function handleError<T>(err: T) {
      setIsLoading(false)
      setHasError(true)
      console.error(err)
    }
    eventSource.addEventListener('error', handleError)
    eventSource.addEventListener('message', (e: any) => {
      try {
        setIsLoading(false)
        if (e.data === '[DONE]') {
          setPromptIndex((x) => {
            return x + 1
          })
          return
        }
        const completionResponse: CreateCompletionResponse = JSON.parse(e.data)
        const text = completionResponse.choices[0].text
        setAnswer((answer) => {
          const currentAnswer = answer ?? ''
          dispatchPromptData({
            index: promptIndex,
            answer: currentAnswer + text,
          })
          return (answer ?? '') + text
        })
      } catch (err) {
        handleError(err)
      }
    })
    eventSource.stream()
    eventSourceRef.current = eventSource
    setIsLoading(true)
},
[promptIndex, promptData]
)
``

## Learn more [\#](https://supabase.com/docs/guides/ai/examples/nextjs-vector-search\#learn-more)

Want to learn more about the awesome tech that is powering this?

- Read about how we built [ChatGPT for the Supabase Docs](https://supabase.com/blog/chatgpt-supabase-docs).
- Read the pgvector Docs for [Embeddings and vector similarity](https://supabase.com/docs/guides/database/extensions/pgvector)
- Watch Greg's video for a full breakdown:

ClippyGPT - How I Built Supabase’s OpenAI Doc Search (Embeddings) - YouTube

Rabbit Hole Syndrome

25.7K subscribers

[ClippyGPT - How I Built Supabase’s OpenAI Doc Search (Embeddings)](https://www.youtube.com/watch?v=Yhtjd7yGGGA)

Rabbit Hole Syndrome

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

[Watch on YouTube](https://www.youtube.com/watch?v=Yhtjd7yGGGA "Watch on YouTube")

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2FxmfNUCjszh4%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Create a project](https://supabase.com/docs/guides/ai/examples/nextjs-vector-search#create-a-project) [Prepare the database](https://supabase.com/docs/guides/ai/examples/nextjs-vector-search#prepare-the-database) [Pre-process the knowledge base at build time](https://supabase.com/docs/guides/ai/examples/nextjs-vector-search#pre-process-the-knowledge-base-at-build-time) [Create text completion with OpenAI API](https://supabase.com/docs/guides/ai/examples/nextjs-vector-search#create-text-completion-with-openai-api) [Display the answer on the frontend](https://supabase.com/docs/guides/ai/examples/nextjs-vector-search#display-the-answer-on-the-frontend) [Learn more](https://supabase.com/docs/guides/ai/examples/nextjs-vector-search#learn-more)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings