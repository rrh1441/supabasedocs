Database

# pgsodium (pending deprecation): Encryption Features

* * *

Supabase DOES NOT RECOMMEND any new usage of [`pgsodium`](https://github.com/michelp/pgsodium).

The [`pgsodium`](https://github.com/michelp/pgsodium) extension is expected to go through a deprecation cycle in the near future. We will reach out to owners of impacted projects to assist with migrations away from [`pgsodium`](https://github.com/michelp/pgsodium) once the deprecation process begins.

The [Vault extension](https://supabase.com/docs/guides/database/vault) won’t be impacted. Its internal implementation will shift away from pgsodium, but the interface and API will remain unchanged.

[`pgsodium`](https://github.com/michelp/pgsodium) is a Postgres extension which provides SQL access to [`libsodium`'s](https://doc.libsodium.org/) high-level cryptographic algorithms.

Supabase previously documented two features derived from pgsodium. Namely [Server Key Management](https://github.com/michelp/pgsodium#server-key-management) and [Transparent Column Encryption](https://github.com/michelp/pgsodium#transparent-column-encryption). At this time, we do not recommend using either on the Supabase platform due to their high level of operational complexity and misconfiguration risk.

Note that Supabase projects are encrypted at rest by default which likely is sufficient for your compliance needs e.g. SOC2 & HIPAA.

## Get the root encryption key for your Supabase project [\#](https://supabase.com/docs/guides/database/extensions/pgsodium\#get-the-root-encryption-key-for-your-supabase-project)

Encryption requires keys. Keeping the keys in the same database as the encrypted data would be unsafe. For more information about managing the `pgsodium` root encryption key on your Supabase project see **[encryption key location](https://supabase.com/docs/guides/database/vault#encryption-key-location)**. This key is required to decrypt values stored in [Supabase Vault](https://supabase.com/docs/guides/database/vault) and data encrypted with Transparent Column Encryption.

## Resources [\#](https://supabase.com/docs/guides/database/extensions/pgsodium\#resources)

- [Supabase Vault](https://supabase.com/docs/guides/database/vault)
- Read more about Supabase Vault in the [blog post](https://supabase.com/blog/vault-now-in-beta)
- [Supabase Vault on GitHub](https://github.com/supabase/vault)

## Resources [\#](https://supabase.com/docs/guides/database/extensions/pgsodium\#resources)

- Official [`pgsodium` documentation](https://github.com/michelp/pgsodium)

### Is this helpful?

NoYes

### On this page

[Get the root encryption key for your Supabase project](https://supabase.com/docs/guides/database/extensions/pgsodium#get-the-root-encryption-key-for-your-supabase-project) [Resources](https://supabase.com/docs/guides/database/extensions/pgsodium#resources) [Resources](https://supabase.com/docs/guides/database/extensions/pgsodium#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings