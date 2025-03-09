Home

# Branching

## Use Supabase Branches to test and preview changes

* * *

Use branching to safely experiment with changes to your Supabase project.

Supabase branches work like Git branches. They let you create and test changes like new configurations, database schemas, or features in a separate, temporary instance without affecting your production setup.

When you're ready to ship your changes, merge your branch to update your production instance with the new changes.

If you understand Git, you already understand Supabase Branching.

## How branching works [\#](https://supabase.com/docs/guides/deployment/branching\#how-branching-works)

- **Separate Environments**: Each branch is a separate environment with its own Supabase instance and API credentials.
- **Git Integration**: Branching works with Git, currently supporting GitHub repositories.
- **Preview Branches**: You can create multiple Preview Branches for testing.
- **Migrations and Seeding**: Branches run migrations from your repository and can seed data using a `seed.sql` file.

## Prerequisites [\#](https://supabase.com/docs/guides/deployment/branching\#prerequisites)

- **Supabase Project**: You need an existing Supabase project.
- **GitHub Repository**: Your project must be connected to a GitHub repository containing your Supabase directory.

You can run multiple Preview Branches for every Supabase project. Branches contain all the Supabase features with their own API credentials.

Preview Environments auto-pause after 5 minutes of inactivity. Upon receiving a new request to your database or REST API, the paused branch will automatically resume to serve the request. The implications of this architecture means

- `pg_cron` jobs will not execute in an auto-paused database.
- Larger variance in request latency due to database cold starts.

If you need higher performance guarantees on your Preview Environment, you can switch individual branches to [persistent](https://supabase.com/docs/guides/deployment/branching#persistent-branches) so they are not auto-paused.

![Each branch has a separate Supabase instance.](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fgithub-workflow-without-branching--light.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

Each Branch is a separate environment.

### Branching workflow [\#](https://supabase.com/docs/guides/deployment/branching\#branching-workflow)

Preview Branch instances contain no data by default. You must include a seed file to seed your preview instance with sample data when the Preview Branch is created. Future versions of Branching may allow for automated data seeding and cloning after we are confident that we can provide safe data masking.

## Git providers [\#](https://supabase.com/docs/guides/deployment/branching\#git-providers)

To manage code changes, your Supabase project must be connected to a Git repository. At this stage, we only support [GitHub](https://supabase.com/docs/guides/deployment/branching#branching-with-github). If you are interested in other Git providers, join the [discussion](https://github.com/orgs/supabase/discussions/18936) for GitLab, Bitbucket, and non-Git based Branching.

### Branching with GitHub [\#](https://supabase.com/docs/guides/deployment/branching\#branching-with-github)

Supabase Branching uses the Supabase GitHub integration to read files from your GitHub repository. With this integration, Supabase watches all commits, branches, and pull requests of your GitHub repository.

You can create a corresponding Preview Branch for any Git branch in your repository. Each time a new Preview Branch is created and configured based on the [`config.toml`](https://supabase.com/docs/guides/local-development/cli/config) configuration on this branch, the migrations from the corresponding Git branch are run on the Preview Branch.

The Preview Branch is also [seeded](https://supabase.com/docs/guides/local-development/seeding-your-database) with sample data based on `./supabase/seed.sql` by default, if that file exists.

Supabase Branching follows the [Trunk Based Development](https://trunkbaseddevelopment.com/) workflow, with one main Production branch and multiple development branches:

![Each GitHub branch can have its own Supabase preview branch.](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fgithub-workflow--light.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

Each GitHub branch can have its own Supabase preview branch.

When you merge your Git branch into the production branch, all new migrations will be applied to your Production environment. If you have declared Storage buckets or Edge Functions in `config.toml`, they will also be deployed automatically. All other configurations, including API, Auth, and seed files, will be ignored by default.

##### Data changes are not merged into production.

### Preparing your Git repository [\#](https://supabase.com/docs/guides/deployment/branching\#preparing-your-git-repository)

You can use the [Supabase CLI](https://supabase.com/docs/guides/cli) to manage changes inside a local `./supabase` directory:

Existing projectNew Project

1

### Initialize Supabase locally

If you don't have a `./supabase` directory, you can create one:

`
supabase init
`

2

### Pull your database migration

Pull your database changes using `supabase db pull`. You can find your database URL in your [database settings](https://supabase.com/dashboard/project/_/settings/database), under the URI tab of the Connection String settings panel. Make sure **Use connection pooling** is checked so you can use the IPv4-enabled connection pooler. (Without connection pooling, your database is only accessible over IPv6, which isn't yet supported by all network providers.)

`
supabase db pull --db-url <db_url>
# Your Database URL looks something like:
# postgres://postgres.xxxx:password@xxxx.pooler.supabase.com:6543/postgres
`

3

### Commit the \`supabase\` directory to Git

Commit the `supabase` directory to Git, and push your changes to your remote repository.

`
git add supabase
git commit -m "Initial migration"
git push
`

### Enable Supabase branching [\#](https://supabase.com/docs/guides/deployment/branching\#enable-supabase-branching)

Once your repository is [correctly prepared](https://supabase.com/docs/guides/deployment/branching#preparing-your-git-repository), you can enable branching from the Supabase dashboard.

##### Prepare your GitHub repository before enabling Branching

If your repository doesn't have all the migration files, your production branch could run an incomplete set of migrations. Make sure your [GitHub repository is prepared](https://supabase.com/docs/guides/deployment/branching#preparing-your-git-repository).

1

### Inside your Supabase project, click \`Enable branching\`

![](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fenable-supabase-branching-opt-in-popover.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

2

### Install the GitHub integration

When clicking `Enable branching` you will see the following dialog:

![](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fenable-supabase-branching-opt-in-install-github.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

If you don't have the GitHub integration installed, click `Add new project connection`. The integration is required to run migration files and the optional database seed file.

You're taken to the GitHub integration page. Click `Install`.

![](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fenable-supabase-branching-opt-in-install-github-permissions.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

Follow the instructions to link your Supabase project to its GitHub repository.

![](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fenable-supabase-branching-opt-in-install-github-integration-link.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

Return to your project and re-click `Enable branching`.

3

### You should now see a popover with the GitHub Connection details shown

Type in the branch you want to use for production. The name of the branch will be validated to make sure it exists in your GitHub repository.

##### Your production branch can't be changed while branching is enabled.

To change your production branch, you need to disable branching and re-enable it with a different branch.

![](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fenable-supabase-branching-opt-in-install-production-branch.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

4

### Click \`I understand, enable branching\`. Branching is now enabled for your project.

### Open a pull request [\#](https://supabase.com/docs/guides/deployment/branching\#open-a-pull-request)

When you open a pull request on GitHub, the Supabase integration automatically checks for a matching preview branch. If one doesn't exist, it gets created.

A comment is added to your PR with the deployment status of your preview branch. Statuses are shown separately for Database, Services, and APIs.

![GitHub view of the deployment status of your preview branch](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fdevelop-your-app-open-pull-request-github.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

Supabase GitHub integration will comment on your PR with the status of your Preview Branch, including whether migrations have successfully run.

Every time a new commit is pushed that changes the migration files in `./supabase/migrations`, the new migrations are run against the preview branch. You can check the status of these runs in the comment's Tasks table.

### Preventing migration failures [\#](https://supabase.com/docs/guides/deployment/branching\#preventing-migration-failures)

We highly recommend turning on a 'required check' for the Supabase integration. You can do this from your GitHub repository settings. This prevents PRs from being merged when migration checks fail, and stops invalid migrations from being merged into your production branch.

![Check the "Require status checks to pass before merging" option.](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fgithub-required-check.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

Check the "Require status checks to pass before merging" option.

### Manually create a preview branch [\#](https://supabase.com/docs/guides/deployment/branching\#manually-create-a-preview-branch)

Preview branches are automatically created for each pull request, but you can also manually create one.

1

### Create a new Git branch in your GitHub repository

You need at least one other branch aside from your Supabase production branch.

![](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fenable-supabase-branching-first-preview-branch-github.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)You can use the GitHub dashboard or command line to create a new branch. In this example, the new branch is called `feat/add-members`.

2

### Navigate to the Branches page in your Supabase dashboard.

In the Supabase dashboard, look for the branch dropdown on the right-hand side of the top bar. It should be set to your production branch by default. Open the dropdown and click [`Manage branches`](https://supabase.com/dashboard/project/_/branches).

![](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fenable-supabase-branching-first-preview-branch-branch-dropdown.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

3

### Create a Supabase preview branch

Click `Create preview branch`.

![](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fenable-supabase-branching-first-preview-branch-branch-action.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

Type in the branch name you want to add. Click `Create branch` to confirm.

##### Only branches from the repository can be used to create a Preview Branch

Git branches from external contributors currently can't support a Preview Branch

![](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fenable-supabase-branching-first-preview-branch-choose-branch.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

The Git integration watches for changes in the `supabase` directory. This includes:

- All SQL migration files, under the subdirectory `migrations`
- An optional `seed.sql` file, used to seed preview instances with sample data

You can create new migrations either [locally](https://supabase.com/docs/guides/deployment/branching#develop-locally) or [remotely](https://supabase.com/docs/guides/deployment/branching#develop-remotely). Local development is recommended.

Local DevelopmentRemote Development

The Supabase CLI provides two options: [manual migrations](https://supabase.com/docs/guides/deployment/database-migrations) and [generated migrations](https://supabase.com/docs/guides/deployment/database-migrations#diffing-changes) using Supabase's local studio and the `supabase db-diff` command. Let's use the latter and push the change to our Preview Branch:

1

### Make schema changes locally

Start Supabase locally:

`
supabase start
`

Then proceed to [localhost:54323](http://localhost:54323/) to access your local Supabase dashboard.

You can make changes in either the [Table Editor](http://localhost:54323/project/default/editor) or the [SQL Editor](https://supabase.com/docs/guides/deployment/(http://localhost:54323/project/default/sql)).

2

### Generate a migration file

Once you are finished making database changes, run `supabase db diff` to create a new migration file. For example:

`
supabase db diff -f "add_employees_table"
`

This will create a SQL file called `./supabase/migrations/[timestamp]add_employees_table.sql`. This file will reflect the changes that you made in your local dashboard.

If you want to continue making changes, you can manually edit this migration file, then use the `db reset` command to pick up your edits:

`
supabase db reset
`

This will reset the database and run all the migrations again. The local dashboard at [localhost:54323](http://localhost:54323/) will reflect the new changes you made.

3

### Commit your changes and push.

Commit and push your migration file to your remote GitHub repository. For example:

`
git add supabase/migrations
git commit -m "Add employees table"
git push --set-upstream origin new-employee
`

The Supabase integration detects the new migration and runs it on the remote Preview Branch. It can take up to 10 minutes for migrations to be applied. If you have a PR for your branch, errors are reflected in the GitHub check run status and in a PR comment.

If you need to reset your database to a clean state (that is, discard any changes that aren't reflected in the migration files), run `supabase db reset` locally. Then, delete the preview branch and recreate it by closing, and reopening your pull request.

### Disable branching [\#](https://supabase.com/docs/guides/deployment/branching\#disable-branching)

You can disable branching at any time. Navigate to the [Branches](https://supabase.com/dashboard/project/_/branches) page, which can be found via the Branches dropdown menu on the top navigation, then click "Manage Branches" in the menu. Click the 'Disable branching' button at the top of the Overview section.

### Persistent branches [\#](https://supabase.com/docs/guides/deployment/branching\#persistent-branches)

Persistent branches are the type of branches that will remain active even after the underlying PR is closed.
You can change any branch to be persistent on the [Branches](https://supabase.com/dashboard/project/_/branches) page by clicking the triple dots icon next to the branch you want to modify, and selecting "Switch to persistent".
All persistent branches can be toggled back to be an ephemeral branch in the exact same way.

## Migration and seeding behavior [\#](https://supabase.com/docs/guides/deployment/branching\#migration-and-seeding-behavior)

Migrations are run in sequential order. Each migration builds upon the previous one.

The preview branch has a record of which migrations have been applied, and only applies new migrations for each commit. This can create an issue when rolling back migrations.

## Branch configuration with remotes [\#](https://supabase.com/docs/guides/deployment/branching\#branch-configuration-with-remotes)

When Branching is enabled, your `config.toml` settings automatically sync to all ephemeral branches through a one-to-one mapping between your Git and Supabase branches.

### Basic configuration [\#](https://supabase.com/docs/guides/deployment/branching\#basic-configuration)

To update configuration for a Supabase branch, modify `config.toml` and push to git. The Supabase integration will detect the changes and apply them to the corresponding branch.

### Remote-specific configuration [\#](https://supabase.com/docs/guides/deployment/branching\#remote-specific-configuration)

For persistent branches that need specific settings, you can use the `[remotes]` block in your `config.toml`. Each remote configuration must reference an existing project ID.

Here's an example of configuring a separate seed script for a staging environment:

`
[remotes.staging]
project_id = "your-project-ref"
[remotes.staging.db.seed]
sql_paths = ["./seeds/staging.sql"]
`

Since the `project_id` field must reference an existing branch, you need to create the persistent branch before adding its configuration. Use the CLI to create a persistent branch first:

`
supabase --experimental branches create --persistent
# Do you want to create a branch named develop? [Y/n]
`

### Configuration merging [\#](https://supabase.com/docs/guides/deployment/branching\#configuration-merging)

When merging a PR into a persistent branch, the Supabase integration:

1. Checks for configuration changes
2. Logs the changes
3. Applies them to the target remote

If no remote is declared or the project ID is incorrect, the configuration step is skipped.

### Available configuration options [\#](https://supabase.com/docs/guides/deployment/branching\#available-configuration-options)

All standard configuration options are available in the `[remotes]` block. This includes:

- Database settings
- API configurations
- Authentication settings
- Edge Functions configuration
- And more

You can use this to maintain different configurations for different environments while keeping them all in version control.

### Rolling back migrations [\#](https://supabase.com/docs/guides/deployment/branching\#rolling-back-migrations)

You might want to roll back changes you've made in an earlier migration change. For example, you may have pushed a migration file containing schema changes you no longer want.

To fix this, push your latest changes, then delete the preview branch in Supabase and reopen it.

The new preview branch is reseeded from your `./supabase/seed.sql` file by default. Any additional data changes you made on the old preview branch are lost. This is equivalent to running `supabase db reset` locally. All migrations are rerun in sequential order.

### Seeding behavior [\#](https://supabase.com/docs/guides/deployment/branching\#seeding-behavior)

Your Preview Branches are seeded with sample data using the same as [local seeding behavior](https://supabase.com/docs/guides/local-development/seeding-your-database).

The database is only seeded once, when the preview branch is created. To rerun seeding, delete the preview branch and recreate it by closing, and reopening your pull request.

## Branching and hosting providers [\#](https://supabase.com/docs/guides/deployment/branching\#branching-and-hosting-providers)

Branching works with hosting providers that support preview deployments.

With the Supabase branching integration, you can sync the Git branch used by the hosting provider with the corresponding Supabase preview branch. This means that the preview deployment built by your hosting provider is matched to the correct database schema, edge functions, and other Supabase configurations.

### Vercel [\#](https://supabase.com/docs/guides/deployment/branching\#vercel)

Install the Vercel integration:

- From the [Vercel marketplace](https://vercel.com/integrations/supabase) or
- By clicking the blue `Deploy` button in a Supabase example app's `README` file

##### Vercel GitHub integration also required.

For branching to work with Vercel, you also need the [Vercel GitHub integration](https://vercel.com/docs/deployments/git/vercel-for-github).

And make sure you have [connected](https://supabase.com/dashboard/org/_/integrations) your Supabase project to your Vercel project.

Supabase automatically updates your Vercel project with the correct environment variables for the corresponding preview branches. The synchronization happens at the time of Pull Request being opened, not at the time of branch creation.

As branching integration is tied to the Preview Deployments feature in Vercel, there are possible race conditions between Supabase setting correct variables, and Vercel running a deployment process. Because of that, Supabase is always automatically re-deploying the most recent deployment of the given pull request.

## Other Git providers [\#](https://supabase.com/docs/guides/deployment/branching\#other-git-providers)

There are multiple alternative Git providers under consideration. If you're interested in branching for GitLab, Bitbucket, or some other provider, [join the GitHub discussion](https://github.com/orgs/supabase/discussions/18938).

## Alternatives to branching [\#](https://supabase.com/docs/guides/deployment/branching\#alternatives-to-branching)

Under the hood, you can see Supabase branching as a way to programmatically "duplicate" your Supabase project via git flow. This allows spawning a new configured (via [`config.toml`](https://supabase.com/docs/guides/local-development/cli/config)) and seeded instance of the database and the adjacent Supabase services (buckets, edge functions, etc.).

1. A new project is deployed on behalf of the user on the Supabase side as the new "branch" if it doesn't already exist. This includes the database, storage, edge-function, and all Supabase-related services.
2. The branch is cloned and the new project is configured based on the [`config.toml`](https://supabase.com/docs/guides/local-development/cli/config) committed into this project branch.
3. Migrations are applied and seeding scripts are run (the first time) for this branch.

You can make a similar setup with a distinct project for each environment. Or just have two environments, the localhost and the production one.

## Pricing [\#](https://supabase.com/docs/guides/deployment/branching\#pricing)

Branching is available on the Pro Plan and above. The price is:

- Each Preview branch costs $0.32 per day
- Each Preview branch is billed until it is removed

## Troubleshooting [\#](https://supabase.com/docs/guides/deployment/branching\#troubleshooting)

### Rolling back migrations [\#](https://supabase.com/docs/guides/deployment/branching\#rolling-back-migrations)

You might want to roll back changes you've made in an earlier migration change. For example, you may have pushed a migration file containing schema changes you no longer want.

To fix this, push the latest changes, then delete the preview branch in Supabase and reopen it.

The new preview branch is reseeded from the `./supabase/seed.sql` file by default. Any additional data changes made on the old preview branch are lost. This is equivalent to running `supabase db reset` locally. All migrations are rerun in sequential order.

### Deployment failures [\#](https://supabase.com/docs/guides/deployment/branching\#deployment-failures)

A deployment might fail for various reasons, including invalid SQL statements and schema conflicts in migrations, errors within the `config.toml` config, or something else.

To check the error message, see the Supabase workflow run for your branch under the [View logs](https://supabase.com/dashboard/project/_/branches) section.

### Schema drift between preview branches [\#](https://supabase.com/docs/guides/deployment/branching\#schema-drift-between-preview-branches)

If multiple preview branches exist, each preview branch might contain different schema changes. This is similar to Git branches, where each branch might contain different code changes.

When a preview branch is merged into the production branch, it creates a schema drift between the production branch and the preview branches that haven't been merged yet.

These conflicts can be resolved in the same way as normal Git Conflicts: merge or rebase from the production Git branch to the preview Git branch. Since migrations are applied sequentially, ensure that migration files are timestamped correctly after the rebase. Changes that build on top of earlier changes should always have later timestamps.

### Changing production branch [\#](https://supabase.com/docs/guides/deployment/branching\#changing-production-branch)

It's not possible to change the Git branch used as the Production branch for Supabase Branching. The only way to change it is to disable and re-enable branching. See [Disable Branching](https://supabase.com/docs/guides/deployment/branching#disable-branching).

## Feedback [\#](https://supabase.com/docs/guides/deployment/branching\#feedback)

Supabase branching is a new and exciting part of the Supabase development ecosystem. Feedback is welcome.

You can join the [conversation over in GitHub discussions](https://github.com/orgs/supabase/discussions/18937).

### Is this helpful?

NoYes

### On this page

[How branching works](https://supabase.com/docs/guides/deployment/branching#how-branching-works) [Prerequisites](https://supabase.com/docs/guides/deployment/branching#prerequisites) [Branching workflow](https://supabase.com/docs/guides/deployment/branching#branching-workflow) [Git providers](https://supabase.com/docs/guides/deployment/branching#git-providers) [Branching with GitHub](https://supabase.com/docs/guides/deployment/branching#branching-with-github) [Preparing your Git repository](https://supabase.com/docs/guides/deployment/branching#preparing-your-git-repository) [Enable Supabase branching](https://supabase.com/docs/guides/deployment/branching#enable-supabase-branching) [Open a pull request](https://supabase.com/docs/guides/deployment/branching#open-a-pull-request) [Preventing migration failures](https://supabase.com/docs/guides/deployment/branching#preventing-migration-failures) [Manually create a preview branch](https://supabase.com/docs/guides/deployment/branching#manually-create-a-preview-branch) [Disable branching](https://supabase.com/docs/guides/deployment/branching#disable-branching) [Persistent branches](https://supabase.com/docs/guides/deployment/branching#persistent-branches) [Migration and seeding behavior](https://supabase.com/docs/guides/deployment/branching#migration-and-seeding-behavior) [Branch configuration with remotes](https://supabase.com/docs/guides/deployment/branching#branch-configuration-with-remotes) [Basic configuration](https://supabase.com/docs/guides/deployment/branching#basic-configuration) [Remote-specific configuration](https://supabase.com/docs/guides/deployment/branching#remote-specific-configuration) [Configuration merging](https://supabase.com/docs/guides/deployment/branching#configuration-merging) [Available configuration options](https://supabase.com/docs/guides/deployment/branching#available-configuration-options) [Rolling back migrations](https://supabase.com/docs/guides/deployment/branching#rolling-back-migrations) [Seeding behavior](https://supabase.com/docs/guides/deployment/branching#seeding-behavior) [Branching and hosting providers](https://supabase.com/docs/guides/deployment/branching#branching-and-hosting-providers) [Vercel](https://supabase.com/docs/guides/deployment/branching#vercel) [Other Git providers](https://supabase.com/docs/guides/deployment/branching#other-git-providers) [Alternatives to branching](https://supabase.com/docs/guides/deployment/branching#alternatives-to-branching) [Pricing](https://supabase.com/docs/guides/deployment/branching#pricing) [Troubleshooting](https://supabase.com/docs/guides/deployment/branching#troubleshooting) [Rolling back migrations](https://supabase.com/docs/guides/deployment/branching#rolling-back-migrations) [Deployment failures](https://supabase.com/docs/guides/deployment/branching#deployment-failures) [Schema drift between preview branches](https://supabase.com/docs/guides/deployment/branching#schema-drift-between-preview-branches) [Changing production branch](https://supabase.com/docs/guides/deployment/branching#changing-production-branch) [Feedback](https://supabase.com/docs/guides/deployment/branching#feedback)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings


![Each branch has a separate Supabase instance.](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fgithub-workflow-without-branching--light.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Each GitHub branch can have its own Supabase preview branch.](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fgithub-workflow--light.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Check the "Require status checks to pass before merging" option.](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fgithub-required-check.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![GitHub view of the deployment status of your preview branch](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fbranching%2Fdevelop-your-app-open-pull-request-github.jpg%3Fv%3D1&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)