Auth

# Password security

## Help your users to protect their password security

* * *

A password is more secure if it is harder to guess or brute-force. In theory, a password is harder to guess if it is longer. It is also harder to guess if it uses a larger set of characters (for example, digits, lowercase and uppercase letters, and symbols).

This table shows the _minimum_ number of guesses that need to be tried to access a user's account:

| Required characters | Length | Guesses |
| --- | --- | --- |
| Digits only | 8 | ~ 227 |
| Digits and letters | 8 | ~ 241 |
| Digits, lower and uppercase letters | 8 | ~ 248 |
| Digits, lower and uppercase letters, symbols | 8 | ~ 252 |

In reality though, passwords are not always generated at random. They often contain variations of names, words, dates, and common phrases. Malicious actors can use these properties to guess a password in fewer attempts.

There are hundreds of millions (and growing!) known passwords out there. Malicious actors can use these lists of leaked passwords to automate login attempts (known as credential stuffing) and steal or access sensitive user data.

## Password strength and leaked password protection [\#](https://supabase.com/docs/guides/auth/password-security\#password-strength-and-leaked-password-protection)

To help protect your users, Supabase Auth allows you fine-grained control over the strength of the passwords used on your project. You can configure these in your project's [Auth settings](https://supabase.com/dashboard/project/_/settings/auth):

- Set a large minimum password length. Anything less than 8 characters is not recommended.
- Set the required characters that must appear at least once in a user's password. Use the strongest option of requiring digits, lowercase and uppercase letters, and symbols.
- Prevent the use of leaked passwords. Supabase Auth uses the open-source [HaveIBeenPwned.org Pwned Passwords API](https://haveibeenpwned.com/Passwords) to reject passwords that have been leaked and are known by malicious actors.

## Additional recommendations [\#](https://supabase.com/docs/guides/auth/password-security\#additional-recommendations)

In addition to choosing suitable password strength settings and preventing the use of leaked passwords, consider asking your users to:

- Use a password manager to store and generate passwords.
- Avoid password reuse across websites and apps.
- Avoid using personal information in passwords.
- Use [Multi-Factor Authentication](https://supabase.com/docs/guides/auth/auth-mfa).

## Frequently asked questions [\#](https://supabase.com/docs/guides/auth/password-security\#frequently-asked-questions)

### How are passwords stored? [\#](https://supabase.com/docs/guides/auth/password-security\#how-are-passwords-stored)

Supabase Auth uses [bcrypt](https://en.wikipedia.org/wiki/Bcrypt), a strong password hashing function, to store hashes of users' passwords. Only hashed passwords are stored. You cannot impersonate a user with the password hash. Each hash is accompanied by a randomly generated salt parameter for extra security.

The hash is stored in the `encrypted_password` column of the `auth.users` table. The column's name is a misnomer (cryptographic hashing is not encryption), but is kept for backward compatibility.

### How will strengthened password requirements affect current users? [\#](https://supabase.com/docs/guides/auth/password-security\#how-will-strengthened-password-requirements-affect-current-users)

Existing users can still sign in with their current password even if it doesn't meet the new, strengthened password requirements. However, if their password falls short of these updated standards, they will encounter a `WeakPasswordError` during the `signInWithPassword` process, explaining why it's considered weak. This change is also applicable to new users and existing users changing their passwords, ensuring everyone adheres to the enhanced security standards.

### Is this helpful?

NoYes

### On this page

[Password strength and leaked password protection](https://supabase.com/docs/guides/auth/password-security#password-strength-and-leaked-password-protection) [Additional recommendations](https://supabase.com/docs/guides/auth/password-security#additional-recommendations) [Frequently asked questions](https://supabase.com/docs/guides/auth/password-security#frequently-asked-questions) [How are passwords stored?](https://supabase.com/docs/guides/auth/password-security#how-are-passwords-stored) [How will strengthened password requirements affect current users?](https://supabase.com/docs/guides/auth/password-security#how-will-strengthened-password-requirements-affect-current-users)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings