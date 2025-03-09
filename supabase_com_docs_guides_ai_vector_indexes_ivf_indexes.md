AI & Vectors

# IVFFlat indexes

* * *

IVFFlat is a type of vector index for approximate nearest neighbor search. It is a frequently used index type that can improve performance when querying highly-dimensional vectors, like those representing embeddings.

## Choosing an index [\#](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes\#choosing-an-index)

Today `pgvector` supports two types of indexes:

- [HNSW](https://supabase.com/docs/guides/ai/vector-indexes/hnsw-indexes)
- [IVFFlat](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes)

In general we recommend using [HNSW](https://supabase.com/docs/guides/ai/vector-indexes/hnsw-indexes) because of its [performance](https://supabase.com/blog/increase-performance-pgvector-hnsw#hnsw-performance-1536-dimensions) and [robustness against changing data](https://supabase.com/docs/guides/ai/vector-indexes/hnsw-indexes#when-should-you-create-hnsw-indexes). If you have a special use case that requires IVFFlat instead, keep reading.

## Usage [\#](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes\#usage)

The way you create an IVFFlat index depends on the distance operator you are using. `pgvector` includes 3 distance operators:

| Operator | Description | [**Operator class**](https://www.postgresql.org/docs/current/sql-createopclass.html) |
| --- | --- | --- |
| `<->` | Euclidean distance | `vector_l2_ops` |
| `<#>` | negative inner product | `vector_ip_ops` |
| `<=>` | cosine distance | `vector_cosine_ops` |

Use the following SQL commands to create an IVFFlat index for the operator(s) used in your queries.

### Euclidean L2 distance ( `vector_l2_ops`) [\#](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes\#euclidean-l2-distance--vectorl2ops-)

`
create index on items using ivfflat (column_name vector_l2_ops) with (lists = 100);
`

### Inner product ( `vector_ip_ops`) [\#](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes\#inner-product--vectoripops-)

`
create index on items using ivfflat (column_name vector_ip_ops) with (lists = 100);
`

### Cosine distance ( `vector_cosine_ops`) [\#](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes\#cosine-distance--vectorcosineops-)

`
create index on items using ivfflat (column_name vector_cosine_ops) with (lists = 100);
`

Currently vectors with up to 2,000 dimensions can be indexed.

## How does IVFFlat work? [\#](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes\#how-does-ivfflat-work)

IVF stands for 'inverted file indexes'. It works by clustering your vectors in order to reduce the similarity search scope. Rather than comparing a vector to every other vector, the vector is only compared against vectors within the same cell cluster (or nearby clusters, depending on your configuration).

### Inverted lists (cell clusters) [\#](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes\#inverted-lists-cell-clusters)

When you create the index, you choose the number of inverted lists (cell clusters). Increase this number to speed up queries, but at the expense of recall.

For example, to create an index with 100 lists on a column that uses the cosine operator:

`
create index on items using ivfflat (column_name vector_cosine_ops) with (lists = 100);
`

For more info on the different operators, see [Distance operations](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes#distance-operators).

For every query, you can set the number of probes (1 by default). The number of probes corresponds to the number of nearby cells to probe for a match. Increase this for better recall at the expense of speed.

To set the number of probes for the duration of the session run:

`
set ivfflat.probes = 10;
`

To set the number of probes only for the current transaction run:

`
begin;
set local ivfflat.probes = 10;
select ...
commit;
`

If the number of probes is the same as the number of lists, exact nearest neighbor search will be performed and the planner won't use the index.

### Approximate nearest neighbor [\#](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes\#approximate-nearest-neighbor)

One important note with IVF indexes is that nearest neighbor search is approximate, since exact search on high dimensional data can't be indexed efficiently. This means that similarity results will change (slightly) after you add an index (trading recall for speed).

## When should you create IVFFlat indexes? [\#](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes\#when-should-you-create-ivfflat-indexes)

`pgvector` recommends building IVFFlat indexes only after the table has sufficient data, so that the internal IVFFlat cell clusters are based on your data's distribution. Anytime the distribution changes significantly, consider rebuilding indexes.

## Resources [\#](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes\#resources)

Read more about indexing on `pgvector`'s [GitHub page](https://github.com/pgvector/pgvector#indexing).

### Is this helpful?

NoYes

### On this page

[Choosing an index](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes#choosing-an-index) [Usage](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes#usage) [Euclidean L2 distance (vector\_l2\_ops)](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes#euclidean-l2-distance--vectorl2ops-) [Inner product (vector\_ip\_ops)](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes#inner-product--vectoripops-) [Cosine distance (vector\_cosine\_ops)](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes#cosine-distance--vectorcosineops-) [How does IVFFlat work?](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes#how-does-ivfflat-work) [Inverted lists (cell clusters)](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes#inverted-lists-cell-clusters) [Approximate nearest neighbor](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes#approximate-nearest-neighbor) [When should you create IVFFlat indexes?](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes#when-should-you-create-ivfflat-indexes) [Resources](https://supabase.com/docs/guides/ai/vector-indexes/ivf-indexes#resources)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings