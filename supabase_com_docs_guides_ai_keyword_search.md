AI & Vectors

# Keyword search

## Learn how to search by words or phrases.

* * *

Keyword search involves locating documents or records that contain specific words or phrases, primarily based on the exact match between the search terms and the text within the data. It differs from [semantic search](https://supabase.com/docs/guides/ai/semantic-search), which interprets the meaning behind the query to provide results that are contextually related, even if the exact words aren't present in the text. Semantic search considers synonyms, intent, and natural language nuances to provide a more nuanced approach to information retrieval.

In Postgres, keyword search is implemented using [full-text search](https://supabase.com/docs/guides/database/full-text-search). It supports indexing and text analysis for data retrieval, focusing on records that match the search criteria. Postgres' full-text search extends beyond simple keyword matching to address linguistic nuances, making it effective for applications that require precise text queries.

## When and why to use keyword search [\#](https://supabase.com/docs/guides/ai/keyword-search\#when-and-why-to-use-keyword-search)

Keyword search is particularly useful in scenarios where precision and specificity matter. It's more effective than semantic search when users are looking for information using exact terminology or specific identifiers. It ensures that results directly contain those terms, reducing the chance of retrieving irrelevant information that might be semantically related but not what the user seeks.

For example in technical or academic research databases, researchers often search for specific studies, compounds, or concepts identified by certain terms or codes. Searching for a specific chemical compound using its exact molecular formula or a unique identifier will yield more focused and relevant results compared to a semantic search, which could return a wide range of documents discussing the compound in different contexts. Keyword search ensures documents that explicitly mention the exact term are found, allowing users to access the precise data they need efficiently.

It's also possible to combine keyword search with semantic search to get the best of both worlds. See [Hybrid search](https://supabase.com/docs/guides/ai/hybrid-search) for more details.

## Using full-text search [\#](https://supabase.com/docs/guides/ai/keyword-search\#using-full-text-search)

For an in-depth guide to Postgres' full-text search, including how to store, index, and query records, see [Full text search](https://supabase.com/docs/guides/database/full-text-search).

## See also [\#](https://supabase.com/docs/guides/ai/keyword-search\#see-also)

- [Semantic search](https://supabase.com/docs/guides/ai/semantic-search)
- [Hybrid search](https://supabase.com/docs/guides/ai/hybrid-search)

### Is this helpful?

NoYes

### On this page

[When and why to use keyword search](https://supabase.com/docs/guides/ai/keyword-search#when-and-why-to-use-keyword-search) [Using full-text search](https://supabase.com/docs/guides/ai/keyword-search#using-full-text-search) [See also](https://supabase.com/docs/guides/ai/keyword-search#see-also)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings