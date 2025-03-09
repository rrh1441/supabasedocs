# Forbidden resource error from the CLI

Last edited: 2/21/2025

* * *

This error typically occurs as a protective measure to prevent unauthorized access to critical operations.

To address this issue, we recommend following these troubleshooting steps:

- Verify Project ID: Ensure the $PROJECT\*REF variable in your commands contains the correct Project ID. You can find your Reference ID under [Project -> Settings -> General](https://supabase.com/dashboard/project/*/settings/general) in your Supabase Dashboard. A Reference ID looks something like `xvljpkujuwroxcuvossw`.
- Authorization Check: Confirm that you’ve been properly authorized. You can also generate a new Access Token in your dashboard and use it for login. Generate a new token [here](https://supabase.com/dashboard/account/tokens) and use it to [log in](https://supabase.com/docs/reference/cli/supabase-login).
- Re-link Project: Try [re-linking](https://supabase.com/docs/reference/cli/supabase-link) your project with the newly generated token.
- Owner/Admin Permissions: Make sure you have [Owner/Admin](https://supabase.com/docs/guides/platform/access-control) permissions for the project.
- CLI Version: Ensure you are using the latest version of the Supabase CLI. If not, update to the most recent version available at [Supabase CLI GitHub](https://github.com/supabase/cli).

If the issue persists, add a --debug --create-ticket flags to your command and contact [support](https://supabase.com/support) with the ticket id and debug logs, which can help in diagnosing the problem further.

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings