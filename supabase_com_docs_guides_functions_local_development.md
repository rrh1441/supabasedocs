Edge Functions

# Local development

## Setup local development environment for Edge Functions.

* * *

We recommend installing the Deno CLI and related tools for local development.

## Deno support [\#](https://supabase.com/docs/guides/functions/local-development\#deno-support)

You can follow the [Deno guide](https://deno.com/manual@v1.32.5/getting_started/setup_your_environment) for setting up your development environment with your favorite editor/IDE.

## Deno with Visual Studio Code [\#](https://supabase.com/docs/guides/functions/local-development\#deno-with-visual-studio-code)

When using VSCode, you should install both the Deno CLI and the the Deno language server [via this link](vscode:extension/denoland.vscode-deno) or by browsing the extensions in VSCode and choosing to install the _Deno_ extension.

The Supabase CLI can automatically create helpful Deno settings when running `supabase init`. Select `y` when prompted "Generate VS Code settings for Deno? \[y/N\]"!

## Deno support in subfolders [\#](https://supabase.com/docs/guides/functions/local-development\#deno-support-in-subfolders)

You can enable the Deno language server for specific sub-paths in a workspace, while using VSCode's built-in JavaScript/TypeScript language server for all other files.

For example if you have a project like this:

`
project
├── app
└── supabase
└── functions
`

To enable the Deno language server only for the `supabase/functions` folder, add `./supabase/functions` to the list of _Deno: Enable Paths_ in the configuration. In your `.vscode/settings.json` file add:

`
{
"deno.enablePaths": ["./supabase/functions"],
"deno.importMap": "./supabase/functions/import_map.json"
}
`

## Multi-root workspaces in VSCode [\#](https://supabase.com/docs/guides/functions/local-development\#multi-root-workspaces-in-vscode)

We recommend using `deno.enablePaths` mentioned above as it's easier to manage, however if you like [multi-root workspaces](https://code.visualstudio.com/docs/editor/workspaces#_multiroot-workspaces) you can use these as an alternative.

VS Code Workspace Configuration for Edge Functions - YouTube

Supabase

45.5K subscribers

[VS Code Workspace Configuration for Edge Functions](https://www.youtube.com/watch?v=lFhU3L8VoSQ)

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

[Watch on YouTube](https://www.youtube.com/watch?v=lFhU3L8VoSQ "Watch on YouTube")

For example, see this `edge-functions.code-workspace` configuration for a CRA (create react app) client with Supabase Edge Functions. You can find the complete example on [GitHub](https://github.com/supabase/supabase/tree/master/examples/edge-functions).

`
{
"folders": [\
    {\
      "name": "project-root",\
      "path": "./"\
    },\
    {\
      "name": "client",\
      "path": "app"\
    },\
    {\
      "name": "supabase-functions",\
      "path": "supabase/functions"\
    }\
],
"settings": {
    "files.exclude": {
      "node_modules/": true,
      "app/": true,
      "supabase/functions/": true
    },
    "deno.importMap": "./supabase/functions/import_map.json"
}
}
`

### Is this helpful?

NoYes

### On this page

[Deno support](https://supabase.com/docs/guides/functions/local-development#deno-support) [Deno with Visual Studio Code](https://supabase.com/docs/guides/functions/local-development#deno-with-visual-studio-code) [Deno support in subfolders](https://supabase.com/docs/guides/functions/local-development#deno-support-in-subfolders) [Multi-root workspaces in VSCode](https://supabase.com/docs/guides/functions/local-development#multi-root-workspaces-in-vscode)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings