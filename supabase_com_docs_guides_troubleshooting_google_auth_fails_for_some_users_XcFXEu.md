# Google Auth fails for some users

Last edited: 1/17/2025

* * *

## Google Auth fails for some users [\#](https://supabase.com/docs/guides/troubleshooting/google-auth-fails-for-some-users-XcFXEu\#google-auth-fails-for-some-users)

If you start facing either of these errors:

`
error=server_error&error_description=Error+getting+user+email+from+external+provider
Missing required authentication credential.
Expected OAuth 2 access token, login cookie or other valid authentication credential.
See https://developers.google.com/identity/sign-in/web/devconsole-project.\",\n \"status\": \"UNAUTHENTICATED\"
}
"level":"error","method":"GET","msg":"500: Error getting user email from external provider","path":"/callback","referer":"https://accounts.google.com/","remote_addr":"x.x.X.x","time":"2023-06-06T21:46:11Z","timestamp":"2023-06-06T21:46:11Z"}
`

It is happening because some Google Suite requires the explicit request of email Auth Scopes:
`https://www.googleapis.com/auth/userinfo.email`

`
const { data, error } = await supabase.auth.signInWithOAuth({
provider: 'google'
options: {
    scopes: 'https://www.googleapis.com/auth/userinfo.email'
}
})
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings