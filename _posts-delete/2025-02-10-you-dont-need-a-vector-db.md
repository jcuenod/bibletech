---
layout: post
title:  "You Don't Need a Vector DB"
subtitle:  "That is to say, 'RAG != Vector Search'"
date:   2025-02-10 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-06.jpg"
---

RAG (Retrieval-Augmented Generation) is all the rage, and everyone's jumping to vector databases like they're the only way to make it work. But before you spin up your new vector database, ask yourself: **do I *actually* need one?**

For a lot of RAG use cases, the answer is probably **no**.

## Vectors Aren't Always the Best for _Retrieval_

Embedding text and finding "semantically similar" stuff is powerful. But retrieval in RAG doesn't *have* to be all about semantic similarity.

**BM25.** Remember BM25? It's been around the block, but this classic ranking function is still a powerhouse for keyword-based search. If your documents are relatively well-structured and keyword-rich, BM25 can often give you surprisingly good retrieval results without any of the vector embedding overhead. Seriously, give it a try before you jump to vectors.

**Direct Lookup.** Even simpler, if you already have a system for indexing your content – think dictionaries, structured data, even just well-organized files – you might be able to directly retrieve the relevant information without any semantic search at all. Why generate embeddings and do a vector search if you can just grab what you need with a key? It's faster, simpler, and sometimes, that's all you need.

Both of these are much lighter computational workloads than semantic similarity searches. They don't require you to compute embeddings for your lookup, and they don't require expensive scans across your table (or even an embedding index).

## Custom DB?  Maybe Not. Your Old Faithful Might Already Do Vectors.

Maybe you *do* need vectors for your RAG setup after all. I won't hold it against you. But that still doesn't mean you have to reach for a dedicated vector database like Chroma or LanceDB. I promise, you can still be "doing AI" without them.

Turns out, a bunch of databases you might already be using have vector support hiding in plain sight:

* **SQLite**: With the [`sqlite-vec`](https://github.com/asg017/sqlite-vec) extension, SQLite gains support for vector operations.
* **DuckDB**: The analytics darling, _natively_ supports [`array_cosine_similarity`](https://duckdb.org/docs/sql/functions/array.html) among other distance functions. And with the [`vss`](https://duckdb.org/docs/extensions/vss.html) extension, it can index vectors (HNSW) as well.
* **Postgres**: For the defacto industry standard, [`pgvector`](https://github.com/pgvector/pgvector) is your friend, adding support for both distance functions and indexing.
* **ClickHouse**: The next on the list of dbs that I go to is ClickHouse. It's probably less known than the first three, but it's incredibly powerful and super fast for analytics workloads. And, you guessed it, Clickhouse also has native [distance functions](https://clickhouse.com/docs/en/sql-reference/functions/distance-functions).

One big advantage of all these RDBMS options is that they also store all your other data right next to your vectors.

## The Catch

The big reason vector DBs exist is that they index your vectors to allow lookups to be faster (all the distance functions are relatively slow to compute across your db). So researchers and groups like Meta and Spotify have released ways of indexing vectors like [HNSW](https://en.wikipedia.org/wiki/Hierarchical_navigable_small_world), [FAISS](https://ai.meta.com/tools/faiss/), and [ANNOY](https://github.com/spotify/annoy) (but many algorithms exist). Each of these approaches deserves an explainer of its own, but suffice it so that they all make searching across vectors faster.

But does that acceleration matter for your workload? For massive datasets and performance-critical applications, a dedicated vector database with specialized indexing is absolutely the way to go. Are you building on of those, or is "fast enough", well, _fast enough_?

##  So, What's the Takeaway?

But RAG doesn't necessarily mean vector DBs or even vectors. Think about your data, your use case, and your existing infrastructure. There are a lot of advantages to having a battle tested tool like sqlite in your pipeline rather than something that popped up in the last two years.
