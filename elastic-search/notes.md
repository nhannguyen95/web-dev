Apache Lucene is a open source, high performance indexing and search library.

Elastic Search is an open source, search and analytics engine, written in Java and built on Apache Lucene.

Elastic Search is:
- Distributed: scales to thousands of nodes.
- High availability: Multiple copies of data.
- Restful API.
- Powerful Query DSL: Express complex queries simply.
- Schemaless: Index data without explicit schema.
- Near realtime search: low latency, ~1 second form the time a document is indexed until it becomes searchable.

Elasticsearch can run on multiple machines within a **cluster**, a single server in that cluster is called a **node**. Every node within a cluster performs the **indexing** operations to index all documents. All nodes take part in search and analysis operations. Any **search query** will be run on multiple nodes in parallel. Every node within a cluster has a **unique id** and name.

A cluster can scale to hundreds or thousands of nodes and it holds the entire indexed data. A cluster is identified by an **unique name**. The machines on a cluster have to be in a same network.

A cluster can have any number of **indices**, each is identified by a **unique name****. An index is like a database in a relational database, it is some type of data organization mechanism, allowing the user to partition data in a certain way.

An index can contain multiple **document types**, types are like tables in a database, each type hold multiple **documents**. Another saying, documents are logically grouped into types.

For example, you can have a `Factory` index, this index can have 3 types:
- `People` document type.
- `Cars` document type.
- `Spare_Parts` document type.

Each type then contains documents that correspond to that type, for example Ferrari doc lives inside of `Cars` type, thid doc contains all details about that particular car.

A document is a basic unit of information to be indexed, it is simply a container of text that needs to be searchable. Documents in ElasticSearch are expressed in JSON.

An index can be too larged to fit in the hard disk of one node, a single node can be too slow to serve all search requests. The solution is to have your index split up to multiple **shards** in across multiple machines (nodes) in your cluster. This process is called **sharding**, each individual node contains 1 shard of an index.

Setting up **replicas** of your index helps increase the availability of your cluster, every shard will have 0 or more corrresponding replica. An index in Elasticsearch have 5 shards and 1 replica by default.
 
ElasticSearch assign every document in an index a **unique ID**, we can also specify that ID. Every document also has a **version** number associated with it, this version increases everytime we update the doc.

Elasticsearch uses REST API to administer the cluster, perform CRUD operations, search etc.

Each document is added into an index under JSON like format, each field that presents in the JSON is indexed. The document can be retrieved/updated as a whole or partially.

The Elasticsearch Query DSL (Domain Specific Language) is the language that you use to specify what you want to search for.

Searchs within Elasticsearch operate under 2 contexts and try to answer 2 different questions:
- How well does this document match this query? => Run in the **Query Context**.
- Does this document match this query clause? => Run in the **Filter Context**.

Query Context components:
- Document included or not?
- Relevance score.
- High score, more relevant.

Filter Context components:
- Document included or not?
- No scoring.
- Used when we want to filter unstructure data: Exact matches, range queries.
- Queries run in filter context are faster / more performant than in query context.

The Elasticsearch query server is stateless.

**term search**: term should have an exact match in the inverted index.

Full text queries using:
- match: not exactly an exact match.
- match_phrase
- match_phrase_prefix

Relevance in Elasticsearch is represented by the **_score** field in every search result, and is computed as a function of the document and the search term. The query clause is directly responsible for a relavant score of a particular document.

Elastic search also allows **fuzzy search**, that look at how similar the search term is to the word present in the document (opposite to exact match).

The core relevance algorithm that elasticsearch relies on is **TF/IDF - Term Frequency/Inverse Document Frequency**:
- Term frequency: how often does the term appear in a field of the document: more often, more relevant.
- Inverse document frequency: how often does the term appear in the index: more often, less relevant (for example stop words as "the", "this").
- Field-length norm: how long is the field which was searched?: Longer field, less relevant - a term appear in a longer field is one of a larger set, so less relevant.

Relevance in Elasticsearch is calculated using TF/IDF in combination with other factors.

**Compound Queries** is a more complex query built out of relatively simple ones.

We can also execute analytic queries via **aggregations**, 4 types of them:
- Metric:
  - Aggregation over a set of documents: either all documents in a search result or documents withing a logical group.
  - Include sum, average, count, mix, max, etc.
- Bucketing
  - Is analogous to the "group by" in SQL.
  - Logically group documents based on search query.
  - A document falls into a bucket if the criteria matches.
  - Each bucket associated with a key.
- Matrix
  - Operates on multiple fields and produces a matrix result.
- Pipeline
  - Aggregations that work on the output of other aggregations.
