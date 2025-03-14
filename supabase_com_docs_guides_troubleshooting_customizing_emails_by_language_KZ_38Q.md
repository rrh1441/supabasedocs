# Customizing Emails by Language

Last edited: 2/4/2025

* * *

When you register a user, you can create meta-data about them.

Creating meta-data with the JS-Client's [signUp function](https://supabase.com/docs/reference/javascript/auth-signup?example=sign-up-with-additional-user-metadata)

`
const { data, error } = await supabase.auth.signUp({
email: 'email@some_[email.com](http://email.com/)',
password: 'example-password',
options: {
    data: {
      first_name: 'John',
      last_name: 'Doe',
      age: 27,
    },
},
})
`

The above example creates a user entry that includes information about their name and age. The data is stored in the auth.users table in the `auth.raw_user_meta_data` column. You can view it in the auth schema with the [SQL Editor](https://supabase.com/dashboard/project/_/editor).

It can be accessed in a project's [Email Templates](https://supabase.com/dashboard/project/_/auth/templates). Below is an example:

![image](https://supabase.com/docs/img/troubleshooting/3eeb2435-dd1c-41bc-9557-44cabff38f59.png)

If you need to update a user's meta-data, you can do so with the [`updateUser`](https://supabase.com/docs/reference/javascript/auth-updateuser?example=update-the-users-metadata) function.

The meta-data can be used to store a users language preferences. You could then use "if statements" in the email template to set the response for a specific language:

`
{{if eq .Data.langauge "en" }}
<h1>Welcome!</h1>
{{ else if eq .Data.langauge "pl" }}
<h1>Witamy!</h1>
{{ else }}
<h1>chuS'ugh, tera' je (Klingon)</h1>
{{end}}
`

Supabase uses the [Go Templating Language](https://pkg.go.dev/text/template) to render emails. It has advanced features for conditions that you may want to [explore](https://gohugo.io/templates/introduction/). For more examples, there is a [GitHub discussion](https://github.com/supabase/gotrue/issues/80#issuecomment-1552264148) that discusses advanced language templates.

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings