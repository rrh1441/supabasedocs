AI & Vectors

# Generate image captions using Hugging Face

## Use the Hugging Face Inference API to make calls to 100,000+ Machine Learning models from Supabase Edge Functions.

* * *

We can combine Hugging Face with [Supabase Storage](https://supabase.com/storage) and [Database Webhooks](https://supabase.com/docs/guides/database/webhooks) to automatically caption for any image we upload to a storage bucket.

## About Hugging Face [\#](https://supabase.com/docs/guides/ai/examples/huggingface-image-captioning\#about-hugging-face)

[Hugging Face](https://huggingface.co/) is the collaboration platform for the machine learning community.

[Huggingface.js](https://huggingface.co/docs/huggingface.js/index) provides a convenient way to make calls to 100,000+ Machine Learning models, making it easy to incorporate AI functionality into your [Supabase Edge Functions](https://supabase.com/edge-functions).

## Setup [\#](https://supabase.com/docs/guides/ai/examples/huggingface-image-captioning\#setup)

- Open your Supabase project dashboard or [create a new project](https://supabase.com/dashboard/projects).
- [Create a new bucket](https://supabase.com/dashboard/project/_/storage/buckets) called `images`.
- Generate TypeScript types from remote Database.
- Create a new Database table called `image_caption`.
  - Create `id` column of type `uuid` which references `storage.objects.id`.
  - Create a `caption` column of type `text`.
- Regenerate TypeScript types to include new `image_caption` table.
- Deploy the function to Supabase: `supabase functions deploy huggingface-image-captioning`.
- Create the Database Webhook in the [Supabase Dashboard](https://supabase.com/dashboard/project/_/database/hooks) to trigger the `huggingface-image-captioning` function anytime a record is added to the `storage.objects` table.

## Generate TypeScript types [\#](https://supabase.com/docs/guides/ai/examples/huggingface-image-captioning\#generate-typescript-types)

To generate the types.ts file for the storage and public schemas, run the following command in the terminal:

`
supabase gen types typescript --project-id=your-project-ref --schema=storage,public > supabase/functions/huggingface-image-captioning/types.ts
`

## Code [\#](https://supabase.com/docs/guides/ai/examples/huggingface-image-captioning\#code)

Find the complete code on [GitHub](https://github.com/supabase/supabase/tree/master/examples/edge-functions/supabase/functions/huggingface-image-captioning).

``
import { serve } from 'https://deno.land/std@0.168.0/http/server.ts'
import { HfInference } from 'https://esm.sh/@huggingface/inference@2.3.2'
import { createClient } from 'jsr:@supabase/supabase-js@2'
import { Database } from './types.ts'
console.log('Hello from `huggingface-image-captioning` function!')
const hf = new HfInference(Deno.env.get('HUGGINGFACE_ACCESS_TOKEN'))
type SoRecord = Database['storage']['Tables']['objects']['Row']
interface WebhookPayload {
type: 'INSERT' | 'UPDATE' | 'DELETE'
table: string
record: SoRecord
schema: 'public'
old_record: null | SoRecord
}
serve(async (req) => {
const payload: WebhookPayload = await req.json()
const soRecord = payload.record
const supabaseAdminClient = createClient<Database>(
    // Supabase API URL - env var exported by default when deployed.
    Deno.env.get('SUPABASE_URL') ?? '',
    // Supabase API SERVICE ROLE KEY - env var exported by default when deployed.
    Deno.env.get('SUPABASE_SERVICE_ROLE_KEY') ?? ''
)
// Construct image url from storage
const { data, error } = await supabaseAdminClient.storage
    .from(soRecord.bucket_id!)
    .createSignedUrl(soRecord.path_tokens!.join('/'), 60)
if (error) throw error
const { signedUrl } = data
// Run image captioning with Huggingface
const imgDesc = await hf.imageToText({
    data: await (await fetch(signedUrl)).blob(),
    model: 'nlpconnect/vit-gpt2-image-captioning',
})
// Store image caption in Database table
await supabaseAdminClient
    .from('image_caption')
    .insert({ id: soRecord.id!, caption: imgDesc.generated_text })
    .throwOnError()
return new Response('ok')
})
``

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2FOgnYxRkxEUw%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[About Hugging Face](https://supabase.com/docs/guides/ai/examples/huggingface-image-captioning#about-hugging-face) [Setup](https://supabase.com/docs/guides/ai/examples/huggingface-image-captioning#setup) [Generate TypeScript types](https://supabase.com/docs/guides/ai/examples/huggingface-image-captioning#generate-typescript-types) [Code](https://supabase.com/docs/guides/ai/examples/huggingface-image-captioning#code)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings