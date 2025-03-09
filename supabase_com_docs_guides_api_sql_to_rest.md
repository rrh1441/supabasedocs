REST API

# SQL to REST API Translator

## Translate SQL queries to HTTP requests and Supabase client code

* * *

Sometimes it's challenging to translate SQL queries to the equivalent [PostgREST](https://postgrest.org/) request or Supabase client code. Use this tool to help with this translation.

PostgREST supports a subset of SQL, so not all SQL queries will translate.

Enter SQL to translate

1

select

Choose language to translate to

cURLHTTPJavaScript

```curl

curl -G http://localhost:54321/rest/v1/books \

  -d "select=title,description" \

  -d "description=ilike.*cheese*" \

  -d "order=title.desc" \

  -d "limit=5" \

  -d "offset=10"
```

### FAQs

What is `curl`?

What do `-G` and `-d` do?

Why is `%` getting converted to `*`?

### Is this helpful?

NoYes

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings