AI & Vectors

# Vector indexes

* * *

Once your vector table starts to grow, you will likely want to add an index to speed up queries. Without indexes, you'll be performing a sequential scan which can be a resource-intensive operation when you have many records.

## Choosing an index [\#](https://supabase.com/docs/guides/ai/vector-indexes\#choosing-an-index)

Today `pgvector` supports two types of indexes:

- [HNSW](https://supabase.com/docs/guides/ai/vector-indexes/hnsw-indexes)
- [IVFFlat](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes)

In general we recommend using [HNSW](https://supabase.com/docs/guides/ai/vector-indexes/hnsw-indexes) because of its [performance](https://supabase.com/blog/increase-performance-pgvector-hnsw#hnsw-performance-1536-dimensions) and [robustness against changing data](https://supabase.com/docs/guides/ai/vector-indexes/hnsw-indexes#when-should-you-create-hnsw-indexes).

## Distance operators [\#](https://supabase.com/docs/guides/ai/vector-indexes\#distance-operators)

Indexes can be used to improve performance of nearest neighbor search using various distance measures. `pgvector` includes 3 distance operators:

| Operator | Description | [**Operator class**](https://www.postgresql.org/docs/current/sql-createopclass.html) |
| --- | --- | --- |
| `<->` | Euclidean distance | `vector_l2_ops` |
| `<#>` | negative inner product | `vector_ip_ops` |
| `<=>` | cosine distance | `vector_cosine_ops` |

Currently vectors with up to 2,000 dimensions can be indexed.

## Resources [\#](https://supabase.com/docs/guides/ai/vector-indexes\#resources)

Read more about indexing on `pgvector`'s [GitHub page](https://github.com/pgvector/pgvector#indexing).

### Is this helpful?

NoYes

### On this page

[Choosing an index](https://supabase.com/docs/guides/ai/vector-indexes#choosing-an-index) [Distance operators](https://supabase.com/docs/guides/ai/vector-indexes#distance-operators) [Resources](https://supabase.com/docs/guides/ai/vector-indexes#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings