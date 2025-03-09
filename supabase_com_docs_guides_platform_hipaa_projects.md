Platform

# HIPAA Projects

* * *

You can use Supabase to store and process Protected Health Information (PHI). If you want to start developing healthcare apps on Supabase, reach out to the Supabase team [here](https://forms.supabase.com/hipaa2) to sign the Business Associate Agreement (BAA).

Organizations must have a signed BAA with Supabase and have the Health Insurance Portability and Accountability Act (HIPAA) add-on enabled when dealing with PHI.

## Configuring a HIPAA project [\#](https://supabase.com/docs/guides/platform/hipaa-projects\#configuring-a-hipaa-project)

When the HIPAA add-on is enabled on an organization, projects within the organization can be configured as _High Compliance_. This configuration can be found in the [General Project Settings page](https://supabase.com/dashboard/project/_/settings) of the dashboard.
Once enabled, additional security checks will be run against the project to ensure the deployed configuration is compliant. These checks are performed on a continual basis and security warnings will appear in the [Security Advisor](https://supabase.com/dashboard/project/_/advisors/security) if a non-compliant setting is detected.

The required project configuration is outlined in the [shared responsibility model](https://supabase.com/docs/guides/deployment/shared-responsibility-model#managing-healthcare-data) for managing healthcare data.

These include:

- Enabling [Point in Time Recovery](https://supabase.com/docs/guides/platform/backups#point-in-time-recovery) which requires at least a [small compute add-on](https://supabase.com/docs/guides/platform/compute-add-ons).
- Turning on [SSL Enforcement](https://supabase.com/docs/guides/platform/ssl-enforcement).
- Enabling [Network Restrictions](https://supabase.com/docs/guides/platform/network-restrictions).

Additional security checks and controls will be added as the security advisor is extended and additional security controls are made available.

### Is this helpful?

NoYes

### On this page

[Configuring a HIPAA project](https://supabase.com/docs/guides/platform/hipaa-projects#configuring-a-hipaa-project)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings