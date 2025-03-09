Edge Functions

# GitHub Actions

* * *

Deploying Edge Functions with GitHub Actions - YouTube

Supabase

45.5K subscribers

[Deploying Edge Functions with GitHub Actions](https://www.youtube.com/watch?v=l2KlzGrhB6w)

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

[Watch on YouTube](https://www.youtube.com/watch?v=l2KlzGrhB6w "Watch on YouTube")

Use the Supabase CLI together with GitHub Actions to automatically deploy our Supabase Edge Functions. [View on GitHub](https://github.com/supabase/supabase/tree/master/examples/edge-functions/supabase/functions/github-action-deploy).

deploy.yaml

`
name: Deploy Function
on:
push:
    branches:
      - main
workflow_dispatch:
jobs:
deploy:
    runs-on: ubuntu-latest
    env:
      SUPABASE_ACCESS_TOKEN: YOUR_SUPABASE_ACCESS_TOKEN
      PROJECT_ID: YOUR_SUPABASE_PROJECT_ID
    steps:
      - uses: actions/checkout@v4
      - uses: supabase/setup-cli@v1
        with:
          version: latest
      - run: supabase functions deploy --project-ref $PROJECT_ID
`

Since Supabase CLI [v1.62.0](https://github.com/supabase/cli/releases/tag/v1.62.0) you can deploy all functions with a single command.

Individual function configuration like [JWT verification](https://supabase.com/docs/guides/cli/config#functions.function_name.verify_jwt) and [import map location](https://supabase.com/docs/guides/cli/config#functions.function_name.import_map) can be set via the `config.toml` file.

`
[functions.hello-world]
verify_jwt = false
`

### Is this helpful?

NoYes

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings