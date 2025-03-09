AI & Vectors

# Hugging Face Inference API

* * *

[Hugging Face](https://huggingface.co/) is an open source hub for AI/ML models and tools. With over 100,000 machine learning models available, Hugging Face provides a great way to integrate specialized AI & ML tasks into your application.

There are 3 ways to use Hugging Face models in your application:

1. Use the [Transformers](https://huggingface.co/docs/transformers/index) Python library to perform inference in a Python backend.
2. [Generate embeddings](https://supabase.com/docs/guides/ai/quickstarts/generate-text-embeddings) directly in Edge Functions using Transformers.js.
3. Use Hugging Face's hosted [Inference API](https://huggingface.co/inference-api) to execute AI tasks remotely on Hugging Face servers. This guide will walk you through this approach.

## AI tasks [\#](https://supabase.com/docs/guides/ai/hugging-face\#ai-tasks)

Below are some of the types of tasks you can perform with Hugging Face:

### Natural language [\#](https://supabase.com/docs/guides/ai/hugging-face\#natural-language)

- [Summarization](https://huggingface.co/tasks/summarization)
- [Text classification](https://huggingface.co/tasks/text-classification)
- [Text generation](https://huggingface.co/tasks/text-generation)
- [Translation](https://huggingface.co/tasks/translation)
- [Fill in the blank](https://huggingface.co/tasks/fill-mask)

### Computer vision [\#](https://supabase.com/docs/guides/ai/hugging-face\#computer-vision)

- [Image to text](https://huggingface.co/tasks/image-to-text)
- [Text to image](https://huggingface.co/tasks/text-to-image)
- [Image classification](https://huggingface.co/tasks/image-classification)
- [Video classification](https://huggingface.co/tasks/video-classification)
- [Object detection](https://huggingface.co/tasks/object-detection)
- [Image segmentation](https://huggingface.co/tasks/image-segmentation)

### Audio [\#](https://supabase.com/docs/guides/ai/hugging-face\#audio)

- [Text to speech](https://huggingface.co/tasks/text-to-speech)
- [Speech to text](https://huggingface.co/tasks/automatic-speech-recognition)
- [Audio classification](https://huggingface.co/tasks/audio-classification)

See a [full list of tasks](https://huggingface.co/tasks).

## Access token [\#](https://supabase.com/docs/guides/ai/hugging-face\#access-token)

First generate a Hugging Face access token for your app:

[https://huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)

Name your token based on the app its being used for and the environment. For example, if you are building an image generation app you might create 2 tokens:

- "Image Generator (Dev)"
- "Image Generator (Prod)"

Since we will be using this token for the inference API, choose the `read` role.

Though it is possible to use the Hugging Face inference API today without an access token, [you may be rate limited](https://huggingface.co/docs/huggingface.js/inference/README#usage).

To ensure you don't experience any unexpected downtime or errors, we recommend creating an access token.

## Edge Functions [\#](https://supabase.com/docs/guides/ai/hugging-face\#edge-functions)

Edge Functions are server-side TypeScript functions that run on-demand. Since Edge Functions run on a server, you can safely give them access to your Hugging Face access token.

You will need the `supabase` CLI [installed](https://supabase.com/docs/guides/cli) for the following commands to work.

To create a new Edge Function, navigate to your local project and initialize Supabase if you haven't already:

`
supabase init
`

Then create an Edge Function:

`
supabase functions new text-to-image
`

Create a file called `.env.local` to store your Hugging Face access token:

`
HUGGING_FACE_ACCESS_TOKEN=<your-token-here>
`

Let's modify the Edge Function to import Hugging Face's inference client and perform a `text-to-image` request:

`
import { serve } from 'https://deno.land/std@0.168.0/http/server.ts'
import { HfInference } from 'https://esm.sh/@huggingface/inference@2.3.2'
const hf = new HfInference(Deno.env.get('HUGGING_FACE_ACCESS_TOKEN'))
serve(async (req) => {
const { prompt } = await req.json()
const image = await hf.textToImage(
    {
      inputs: prompt,
      model: 'stabilityai/stable-diffusion-2',
    },
    {
      use_cache: false,
    }
)
return new Response(image)
})
`

1. This function creates a new instance of `HfInference` using the `HUGGING_FACE_ACCESS_TOKEN` environment variable.

2. It expects a POST request that includes a JSON request body. The JSON body should include a parameter called `prompt` that represents the text-to-image prompt that we will pass to Hugging Face's inference API.

3. Next we call `textToImage()`, passing in the user's prompt along with the model that we would like to use for the image generation. Today Hugging Face recommends `stabilityai/stable-diffusion-2`, but you can change this to any other text-to-image model. You can see a list of which models are supported for each task by navigating to their [models page](https://huggingface.co/models?pipeline_tag=text-to-image) and filtering by task.

4. We set `use_cache` to `false` so that repeat queries with the same prompt will produce new images. If the task and model you are using is deterministic (will always produce the same result based on the same input), consider setting `use_cache` to `true` for faster responses.

5. The `image` result returned from the API will be a `Blob`. We can pass the `Blob` directly into a `new Response()` which will automatically set the content type and body of the response from the `image`.


Finally let's serve the Edge Function locally to test it:

`
supabase functions serve --env-file .env.local --no-verify-jwt
`

Remember to pass in the `.env.local` file using the `--env-file` parameter so that the Edge Function can access the `HUGGING_FACE_ACCESS_TOKEN`.

For demo purposes we set `--no-verify-jwt` to make it easy to test the Edge Function without passing in a JWT token. In a real application you will need to pass the JWT as a `Bearer` token in the `Authorization` header.

At this point, you can make an API request to your Edge Function using your preferred frontend framework (Next.js, React, Expo, etc). We can also test from the terminal using `curl`:

`
curl --output result.jpg --location --request POST 'http://localhost:54321/functions/v1/text-to-image' \
  --header 'Content-Type: application/json' \
  --data '{"prompt":"Llama wearing sunglasses"}'
`

In this example, your generated image will save to `result.jpg`:

![Llama wearing sunglasses example](https://supabase.com/docs/img/ai/hugging-face/llama-sunglasses-example.png)

## Next steps [\#](https://supabase.com/docs/guides/ai/hugging-face\#next-steps)

You can now create an Edge Function that invokes a Hugging Face task using your model of choice.

Try running some other [AI tasks](https://supabase.com/docs/guides/ai/hugging-face#ai-tasks).

## Resources [\#](https://supabase.com/docs/guides/ai/hugging-face\#resources)

- Official [Hugging Face site](https://huggingface.co/).
- Official [Hugging Face JS docs](https://huggingface.co/docs/huggingface.js).
- [Generate image captions](https://supabase.com/docs/guides/ai/examples/huggingface-image-captioning) using Hugging Face.

### Is this helpful?

NoYes

### On this page

[AI tasks](https://supabase.com/docs/guides/ai/hugging-face#ai-tasks) [Natural language](https://supabase.com/docs/guides/ai/hugging-face#natural-language) [Computer vision](https://supabase.com/docs/guides/ai/hugging-face#computer-vision) [Audio](https://supabase.com/docs/guides/ai/hugging-face#audio) [Access token](https://supabase.com/docs/guides/ai/hugging-face#access-token) [Edge Functions](https://supabase.com/docs/guides/ai/hugging-face#edge-functions) [Next steps](https://supabase.com/docs/guides/ai/hugging-face#next-steps) [Resources](https://supabase.com/docs/guides/ai/hugging-face#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings