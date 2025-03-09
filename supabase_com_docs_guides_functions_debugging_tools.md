Edge Functions

# Local Debugging with DevTools

## How to use Chrome DevTools to debug Edge Functions.

* * *

Since [v1.171.0](https://github.com/supabase/cli/releases/tag/v1.171.0) the Supabase CLI supports debugging Edge Functions via the v8 inspector protocol, allowing for debugging via [Chrome DevTools](https://developer.chrome.com/docs/devtools/) and other Chromium-based browsers.

### Inspect with Chrome Developer Tools [\#](https://supabase.com/docs/guides/functions/debugging-tools\#inspect-with-chrome-developer-tools)

You can use the [Chrome DevTools](https://developer.chrome.com/docs/devtools/) to set breakpoints and inspect the execution of your Edge Functions.

1. Serve your functions in [inspect mode](https://supabase.com/docs/reference/cli/supabase-functions-serve): `supabase functions serve --inspect-mode brk`. This will set a breakpoint at the first line to pause script execution before any code runs.
2. In your Chrome browser navigate to `chrome://inspect`.
3. Click the "Configure..."" button to the right of the Discover network targets checkbox.
4. In the Target discovery settings dialog box that opens, enter `127.0.0.1:8083` in the blank space and click the "Done" button to exit the dialog box.
5. Click "Open dedicated DevTools for Node" to complete the preparation for debugging. The opened DevTools window will now listen to any incoming requests to edge-runtime.
6. Send a request to your function running locally, e.g. via curl or Postman. The DevTools window will now pause script execution at first line.
7. In the "Sources" tab navigate to `file://` \> `home/deno/functions/<your-function-name>/index.ts`.
8. Use the DevTools to set breakpoints and inspect the execution of your Edge Function.

![Debugging in Chrome DevTools.](https://supabase.com/docs/img/guides/functions/debug-chrome-devtools.png)

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2FsOrtcoKg5zQ%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Inspect with Chrome Developer Tools](https://supabase.com/docs/guides/functions/debugging-tools#inspect-with-chrome-developer-tools)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings