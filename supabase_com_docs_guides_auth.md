Auth

# Auth

## Use Supabase to authenticate and authorize your users.

* * *

Supabase Auth makes it easy to implement authentication and authorization in your app. We provide client SDKs and API endpoints to help you create and manage users.

Your users can use many popular Auth methods, including password, magic link, one-time password (OTP), social login, and single sign-on (SSO).

## About authentication and authorization [\#](https://supabase.com/docs/guides/auth\#about-authentication-and-authorization)

Authentication and authorization are the core responsibilities of any Auth system.

- **Authentication** means checking that a user is who they say they are.
- **Authorization** means checking what resources a user is allowed to access.

Supabase Auth uses [JSON Web Tokens (JWTs)](https://supabase.com/docs/guides/auth/jwts) for authentication. Auth integrates with Supabase's database features, making it easy to use [Row Level Security (RLS)](https://supabase.com/docs/guides/database/postgres/row-level-security) for authorization.

## The Supabase ecosystem [\#](https://supabase.com/docs/guides/auth\#the-supabase-ecosystem)

You can use Supabase Auth as a standalone product, but it's also built to integrate with the Supabase ecosystem.

Auth uses your project's Postgres database under the hood, storing user data and other Auth information in a special schema. You can connect this data to your own tables using triggers and foreign key references.

Auth also enables access control to your database's automatically generated [REST API](https://supabase.com/docs/guides/api). When using Supabase SDKs, your data requests are automatically sent with the user's Auth Token. The Auth Token scopes database access on a row-by-row level when used along with [RLS policies](https://supabase.com/docs/guides/database/postgres/row-level-security).

## Providers [\#](https://supabase.com/docs/guides/auth\#providers)

Supabase Auth works with many popular Auth methods, including Social and Phone Auth using third-party providers. See the following sections for a list of supported third-party providers.

### Social Auth [\#](https://supabase.com/docs/guides/auth\#social-auth)

[![Apple Icon](https://supabase.com/docs/img/icons/apple-icon.svg)\\
\\
**Apple**](https://supabase.com/docs/guides/auth/social-login/auth-apple) [![Azure (Microsoft) Icon](https://supabase.com/docs/img/icons/microsoft-icon.svg)\\
\\
**Azure (Microsoft)**](https://supabase.com/docs/guides/auth/social-login/auth-azure) [![Bitbucket Icon](https://supabase.com/docs/img/icons/bitbucket-icon.svg)\\
\\
**Bitbucket**](https://supabase.com/docs/guides/auth/social-login/auth-bitbucket) [![Discord Icon](https://supabase.com/docs/img/icons/discord-icon.svg)\\
\\
**Discord**](https://supabase.com/docs/guides/auth/social-login/auth-discord) [![Facebook Icon](https://supabase.com/docs/img/icons/facebook-icon.svg)\\
\\
**Facebook**](https://supabase.com/docs/guides/auth/social-login/auth-facebook) [![Figma Icon](https://supabase.com/docs/img/icons/figma-icon.svg)\\
\\
**Figma**](https://supabase.com/docs/guides/auth/social-login/auth-figma) [![GitHub Icon](https://supabase.com/docs/img/icons/github-icon-light.svg)\\
\\
**GitHub**](https://supabase.com/docs/guides/auth/social-login/auth-github) [![GitLab Icon](https://supabase.com/docs/img/icons/gitlab-icon.svg)\\
\\
**GitLab**](https://supabase.com/docs/guides/auth/social-login/auth-gitlab) [![Google Icon](https://supabase.com/docs/img/icons/google-icon.svg)\\
\\
**Google**](https://supabase.com/docs/guides/auth/social-login/auth-google) [![Kakao Icon](https://supabase.com/docs/img/icons/kakao-icon.svg)\\
\\
**Kakao**](https://supabase.com/docs/guides/auth/social-login/auth-kakao) [![Keycloak Icon](https://supabase.com/docs/img/icons/keycloak-icon.svg)\\
\\
**Keycloak**](https://supabase.com/docs/guides/auth/social-login/auth-keycloak) [![LinkedIn Icon](https://supabase.com/docs/img/icons/linkedin-icon.svg)\\
\\
**LinkedIn**](https://supabase.com/docs/guides/auth/social-login/auth-linkedin) [![Notion Icon](https://supabase.com/docs/img/icons/notion-icon.svg)\\
\\
**Notion**](https://supabase.com/docs/guides/auth/social-login/auth-notion) [![Slack Icon](https://supabase.com/docs/img/icons/slack-icon.svg)\\
\\
**Slack**](https://supabase.com/docs/guides/auth/social-login/auth-slack) [![Spotify Icon](https://supabase.com/docs/img/icons/spotify-icon.svg)\\
\\
**Spotify**](https://supabase.com/docs/guides/auth/social-login/auth-spotify) [![Twitter Icon](https://supabase.com/docs/img/icons/twitter-icon-light.svg)\\
\\
**Twitter**](https://supabase.com/docs/guides/auth/social-login/auth-twitter) [![Twitch Icon](https://supabase.com/docs/img/icons/twitch-icon.svg)\\
\\
**Twitch**](https://supabase.com/docs/guides/auth/social-login/auth-twitch) [![WorkOS Icon](https://supabase.com/docs/img/icons/workos-icon.svg)\\
\\
**WorkOS**](https://supabase.com/docs/guides/auth/social-login/auth-workos) [![Zoom Icon](https://supabase.com/docs/img/icons/zoom-icon.svg)\\
\\
**Zoom**](https://supabase.com/docs/guides/auth/social-login/auth-zoom)

### Phone Auth [\#](https://supabase.com/docs/guides/auth\#phone-auth)

[![MessageBird Icon](https://supabase.com/docs/img/icons/messagebird-icon.svg)\\
\\
**MessageBird**](https://supabase.com/docs/guides/auth/phone-login?showSmsProvider=MessageBird) [![Twilio Icon](https://supabase.com/docs/img/icons/twilio-icon.svg)\\
\\
**Twilio**](https://supabase.com/docs/guides/auth/phone-login?showSmsProvider=Twilio) [![Vonage Icon](https://supabase.com/docs/img/icons/vonage-icon-light.svg)\\
\\
**Vonage**](https://supabase.com/docs/guides/auth/phone-login?showSmsProvider=Vonage)

## Pricing [\#](https://supabase.com/docs/guides/auth\#pricing)

Charges apply to Monthly Active Users (MAU), Monthly Active Third-Party Users (Third-Party MAU), and Monthly Active SSO Users (SSO MAU) and Advanced MFA Add-ons. For a detailed breakdown of how these charges are calculated, refer to the following pages:

- [Pricing MAU](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users)
- [Pricing Third-Party MAU](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-third-party)
- [Pricing SSO MAU](https://supabase.com/docs/guides/platform/manage-your-usage/monthly-active-users-sso)
- [Advanced MFA - Phone](https://supabase.com/docs/guides/platform/manage-your-usage/advanced-mfa-phone)

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2F6ow_jW4epf8%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[About authentication and authorization](https://supabase.com/docs/guides/auth#about-authentication-and-authorization) [The Supabase ecosystem](https://supabase.com/docs/guides/auth#the-supabase-ecosystem) [Providers](https://supabase.com/docs/guides/auth#providers) [Social Auth](https://supabase.com/docs/guides/auth#social-auth) [Phone Auth](https://supabase.com/docs/guides/auth#phone-auth) [Pricing](https://supabase.com/docs/guides/auth#pricing)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings