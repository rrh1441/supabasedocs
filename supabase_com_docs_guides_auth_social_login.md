Auth

# Social Login

* * *

Social Login (OAuth) is an open standard for authentication that allows users to log in to one website or application using their credentials from another website or application. OAuth allows users to grant third-party applications access to their online accounts without sharing their passwords.
OAuth is commonly used for things like logging in to a social media account from a third-party app. It is a secure and convenient way to authenticate users and share information between applications.

## Benefits [\#](https://supabase.com/docs/guides/auth/social-login\#benefits)

There are several reasons why you might want to add social login to your applications:

- **Improved user experience**: Users can register and log in to your application using their existing social media accounts, which can be faster and more convenient than creating a new account from scratch. This makes it easier for users to access your application, improving their overall experience.

- **Better user engagement**: You can access additional data and insights about your users, such as their interests, demographics, and social connections. This can help you tailor your content and marketing efforts to better engage with your users and provide a more personalized experience.

- **Increased security**: Social login can improve the security of your application by leveraging the security measures and authentication protocols of the social media platforms that your users are logging in with. This can help protect against unauthorized access and account takeovers.


## Set up a social provider with Supabase Auth [\#](https://supabase.com/docs/guides/auth/social-login\#set-up-a-social-provider-with-supabase-auth)

Supabase supports a suite of social providers. Follow these guides to configure a social provider for your platform.

[![Google Icon](https://supabase.com/docs/img/icons/google-icon.svg)\\
\\
**Google**](https://supabase.com/docs/guides/auth/social-login/auth-google) [![Facebook Icon](https://supabase.com/docs/img/icons/facebook-icon.svg)\\
\\
**Facebook**](https://supabase.com/docs/guides/auth/social-login/auth-facebook) [![Apple Icon](https://supabase.com/docs/img/icons/apple-icon.svg)\\
\\
**Apple**](https://supabase.com/docs/guides/auth/social-login/auth-apple) [![Azure (Microsoft) Icon](https://supabase.com/docs/img/icons/microsoft-icon.svg)\\
\\
**Azure (Microsoft)**](https://supabase.com/docs/guides/auth/social-login/auth-azure) [![Twitter Icon](https://supabase.com/docs/img/icons/twitter-icon-light.svg)\\
\\
**Twitter**](https://supabase.com/docs/guides/auth/social-login/auth-twitter) [![GitHub Icon](https://supabase.com/docs/img/icons/github-icon-light.svg)\\
\\
**GitHub**](https://supabase.com/docs/guides/auth/social-login/auth-github) [![Gitlab Icon](https://supabase.com/docs/img/icons/gitlab-icon.svg)\\
\\
**Gitlab**](https://supabase.com/docs/guides/auth/social-login/auth-gitlab) [![Bitbucket Icon](https://supabase.com/docs/img/icons/bitbucket-icon.svg)\\
\\
**Bitbucket**](https://supabase.com/docs/guides/auth/social-login/auth-bitbucket) [![Discord Icon](https://supabase.com/docs/img/icons/discord-icon.svg)\\
\\
**Discord**](https://supabase.com/docs/guides/auth/social-login/auth-discord) [![Figma Icon](https://supabase.com/docs/img/icons/figma-icon.svg)\\
\\
**Figma**](https://supabase.com/docs/guides/auth/social-login/auth-figma) [![Kakao Icon](https://supabase.com/docs/img/icons/kakao-icon.svg)\\
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
**Spotify**](https://supabase.com/docs/guides/auth/social-login/auth-spotify) [![Twitch Icon](https://supabase.com/docs/img/icons/twitch-icon.svg)\\
\\
**Twitch**](https://supabase.com/docs/guides/auth/social-login/auth-twitch) [![WorkOS Icon](https://supabase.com/docs/img/icons/workos-icon.svg)\\
\\
**WorkOS**](https://supabase.com/docs/guides/auth/social-login/auth-workos) [![Zoom Icon](https://supabase.com/docs/img/icons/zoom-icon.svg)\\
\\
**Zoom**](https://supabase.com/docs/guides/auth/social-login/auth-zoom)

## Provider tokens [\#](https://supabase.com/docs/guides/auth/social-login\#provider-tokens)

You can use the provider token and provider refresh token returned to make API calls to the OAuth provider. For example, you can use the Google provider token to access Google APIs on behalf of your user.

Supabase Auth does not manage refreshing the provider token for the user. Your application will need to use the provider refresh token to obtain a new provider token. If no provider refresh token is returned, then it could mean one of the following:

- The OAuth provider does not return a refresh token
- Additional scopes need to be specified in order for the OAuth provider to return a refresh token.

Provider tokens are intentionally not stored in your project's database. This is because provider tokens give access to potentially sensitive user data in third-party systems. Different applications have different needs, and one application's OAuth scopes may be significantly more permissive than another. If you want to use the provider token outside of the browser that completed the OAuth flow, it is recommended to send it to a trusted and secure server you control.

### Is this helpful?

NoYes

### On this page

[Benefits](https://supabase.com/docs/guides/auth/social-login#benefits) [Set up a social provider with Supabase Auth](https://supabase.com/docs/guides/auth/social-login#set-up-a-social-provider-with-supabase-auth) [Provider tokens](https://supabase.com/docs/guides/auth/social-login#provider-tokens)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings