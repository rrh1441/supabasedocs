Platform

# Multi-factor Authentication

## Enable multi-factor authentication (MFA) to keep your account secure.

* * *

This guide is for adding MFA to your Supabase user account. If you want to enable MFA for users in your Supabase project, refer to [**this guide**](https://supabase.com/docs/guides/auth/auth-mfa) instead.

Multi-factor authentication (MFA) adds an additional layer of security to your user account, by requiring a second factor to verify your user identity. Supabase allows users to enable MFA on their account and set it as a requirement for subsequent logins.

## Supported authentication factors [\#](https://supabase.com/docs/guides/platform/multi-factor-authentication\#supported-authentication-factors)

Currently, Supabase supports adding a unique time-based one-time password (TOTP) to your user account as an additional security factor. You can manage your TOTP factor using apps such as 1Password, Authy, Google Authenticator or Apple's Keychain.

## Enable MFA [\#](https://supabase.com/docs/guides/platform/multi-factor-authentication\#enable-mfa)

You can enable MFA for your user account under your [Supabase account settings](https://supabase.com/dashboard/account/security). Enabling MFA will result in all other user sessions to be automatically logged out and forced to sign-in again with MFA.

Supabase does not return recovery codes. Instead, we recommend that you register a backup TOTP factor to use in an event that you lose access to your primary TOTP factor. Make sure you use a different device and app, or store the secret in a secure location different than your primary one.

For security reasons, we will not be able to restore access to your account if you lose all your two-factor authentication credentials. Do register a backup factor if necessary.

## Login with MFA [\#](https://supabase.com/docs/guides/platform/multi-factor-authentication\#login-with-mfa)

Once you've enabled MFA for your Supabase user account, you will be prompted to enter your second factor challenge code as seen in your preferred TOTP app.

## Disable MFA [\#](https://supabase.com/docs/guides/platform/multi-factor-authentication\#disable-mfa)

You can disable MFA for your user account under your [Supabase account settings](https://supabase.com/dashboard/account/security). On subsequent login attempts, you will not be prompted to enter a MFA code.

We strongly recommend that you do not disable MFA to avoid unauthorized access to your user account.

### Is this helpful?

NoYes

### On this page

[Supported authentication factors](https://supabase.com/docs/guides/platform/multi-factor-authentication#supported-authentication-factors) [Enable MFA](https://supabase.com/docs/guides/platform/multi-factor-authentication#enable-mfa) [Login with MFA](https://supabase.com/docs/guides/platform/multi-factor-authentication#login-with-mfa) [Disable MFA](https://supabase.com/docs/guides/platform/multi-factor-authentication#disable-mfa)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings