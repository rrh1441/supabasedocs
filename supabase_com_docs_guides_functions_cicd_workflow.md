Edge Functions

# Deploying with CI / CD pipelines

## Use GitHub Actions, Bitbucket, and GitLab CI to deploy your Edge Functions.

* * *

You can use popular CI / CD tools like GitHub Actions, Bitbucket, and GitLab CI to automate Edge Function deployments.

## GitHub Actions [\#](https://supabase.com/docs/guides/functions/cicd-workflow\#github-actions)

You can use the official [`setup-cli` GitHub Action](https://github.com/marketplace/actions/supabase-cli-action) to run Supabase CLI commands in your GitHub Actions.

The following GitHub Action deploys all Edge Functions any time code is merged into the `main` branch:

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
      SUPABASE_ACCESS_TOKEN: ${{ secrets.SUPABASE_ACCESS_TOKEN }}
      PROJECT_ID: your-project-id
    steps:
      - uses: actions/checkout@v3
      - uses: supabase/setup-cli@v1
        with:
          version: latest
      - run: supabase functions deploy --project-ref $PROJECT_ID
`

## GitLab CI [\#](https://supabase.com/docs/guides/functions/cicd-workflow\#gitlab-ci)

Here is the sample pipeline configuration to deploy via GitLab CI.

`
image: node:20
# List of stages for jobs, and their order of execution
stages:
  - setup
  - deploy
# This job runs in the setup stage, which runs first.
setup-npm:
stage: setup
script:
    - npm i supabase
cache:
    paths:
      - node_modules/
artifacts:
    paths:
      - node_modules/
# This job runs in the deploy stage, which only starts when the job in the build stage completes successfully.
deploy-function:
stage: deploy
script:
    - npx supabase init
    - npx supabase functions deploy --debug
services:
    - docker:dind
variables:
    DOCKER_HOST: tcp://docker:2375
`

## Bitbucket Pipelines [\#](https://supabase.com/docs/guides/functions/cicd-workflow\#bitbucket-pipelines)

Here is the sample pipeline configuration to deploy via Bitbucket.

`
image: node:20
pipelines:
default:
    - step:
        name: Setup
        caches:
          - node
        script:
          - npm i supabase
    - parallel:
        - step:
            name: Functions Deploy
            script:
              - npx supabase init
              - npx supabase functions deploy --debug
            services:
              - docker
`

## Declarative configuration [\#](https://supabase.com/docs/guides/functions/cicd-workflow\#declarative-configuration)

Individual function configuration like [JWT verification](https://supabase.com/docs/guides/cli/config#functions.function_name.verify_jwt) and [import map location](https://supabase.com/docs/guides/cli/config#functions.function_name.import_map) can be set via the `config.toml` file.

`
[functions.hello-world]
verify_jwt = false
`

## Resources [\#](https://supabase.com/docs/guides/functions/cicd-workflow\#resources)

- See the [example on GitHub](https://github.com/supabase/supabase/blob/master/examples/edge-functions/.github/workflows/deploy.yaml).

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2F6OMVWiiycLs%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[GitHub Actions](https://supabase.com/docs/guides/functions/cicd-workflow#github-actions) [GitLab CI](https://supabase.com/docs/guides/functions/cicd-workflow#gitlab-ci) [Bitbucket Pipelines](https://supabase.com/docs/guides/functions/cicd-workflow#bitbucket-pipelines) [Declarative configuration](https://supabase.com/docs/guides/functions/cicd-workflow#declarative-configuration) [Resources](https://supabase.com/docs/guides/functions/cicd-workflow#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings