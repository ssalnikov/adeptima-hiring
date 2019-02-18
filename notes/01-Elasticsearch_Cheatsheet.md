# Elasticsearch REST Cheatsheet

- [Make sure it works](#make-sure-it-works)
- [Basic CRUD](#basic-crud)
- [Indexing](#indexing)
- [Querying](#querying)
- [Filtering](#filtering)
- [Examples](#examples)
- [Notes](#notes)

## Key concepts
- Cluster: a collection of ES nodes.
- Nodes: server/instance stores data or coordinate with indexing and search capabilities.
- Index: a collection of documents. Like tables in MySQL.
- Type: To ensure optimal performance, we can define mappings for data types.
- Document: a basic unit of information. Like record in SQL.
- Shards: It allows us to horizontally split a big index, and store in multiple nodes.

## Getting started
Elastic Dokku/Docker Image 5.3
- https://github.com/reactima/elasticsearch

Kibana Dokku/Docker Image 5.3
- https://github.com/reactima/elasticsearch

elasticsearch-head
- https://github.com/mobz/elasticsearch-head

Good tutorials to get started
- Querying ElasticSearch - A Tutorial and Guide, http://okfnlabs.org/blog/2013/07/01/elasticsearch-query-tutorial.html
- ElasticSearch 101 a getting started tutorial, http://joelabrahamsson.com/elasticsearch-101/
- http://bitquabit.com/post/having-fun-python-and-elasticsearch-part-1/


## Make sure it works

```json
$ curl 'localhost:9200'
{
  "status" : 200,
  "name" : "Moon Knight",
  "cluster_name" : "elasticsearch",
  "version" : {
    "number" : "1.7.2",
    "build_hash" : "e43676b1385b8125d647f593f7202acbd816e8ec",
    "build_timestamp" : "2015-09-14T09:49:53Z",
    "build_snapshot" : false,
    "lucene_version" : "4.10.4"
  },
  "tagline" : "You Know, for Search"
}
```


## Basic CRUD
Elasticsearch supports the basic CRUD operations using HTTP verbs, and supports
the Lucene query parser syntax, e.g. `fieldname:value` and wildcards `fieldname:value*`.
Creating new entries is known as "indexing", while searching is known as "querying".


Resource structure.
```bash
# <index> and <type-of-document> are required.
# <id> is optional and will be generated if one is not provided.
http://localhost:9200/<index>/<type-of-document>/[<id>]

# Examples.
http://localhost:9200/people/tweets/1
http://localhost:9200/articles/header/1
```


## Indexing
Indexing is also known as the "Create" in CRUD. PUT requests are used for their
simplicity to both create and update indices.

```bash
# Create a 'tweets' document in the 'people' index with an id of '1'.
curl -XPUT 'localhost:9200/people/tweets/1' -d '{
  "user": "mycat",
  "message": "Hello world"
}'

# Returns:
{"_index":"people","_type":"tweets","_id":"1","_version":1,"created":true}


# Running the same command again will update the index.
curl -XPUT 'localhost:9200/people/tweets/1' -d '{
  "user": "mycat",
  "message": "Gutentag"
}'

# Returns:
{"_index":"people","_type":"tweets","_id":"1","_version":2,"created":false}
```


## Querying
Querying is also known as reverse-index searching. Every field in a document is
index and efficiently stored, so searching is blazingly quick when compared to
technologies, i.e MySQL full-text search. You can search by either specifying
query parameters or by passing JSON in the request body using the
[Query String DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html)

```bash
# Search across all indices and all types.
http://localhost:9200/_search

# Search across all types in the 'articles' index.
http://localhost:9200/articles/_search

# Search explicitly in the 'article' type within the 'articles' index.
http://localhost:9200/articles/header/_search
```

Method 1. Search with query strings
```bash
# Searches for the most relevant articles with 'awesome' in the text.
curl 'localhost:9200/articles/_search?q=text:awesome'
```

Method 2. Search using the DSL syntax.
```bash
# Same as above, but using Lucene query syntax.
curl 'localhost:9200/articles/_search' -d '{
  "query": {
    "query_string": {
      "query": "text:awesome"
    }
  }
}'

# Simple query term.
curl -XGET localhost:9200/articles/_search -d '{
  "query" : {
    "term" : { "text": "awesome" }
  }
}'
```


## Filtering
Filters are a magnitude of order more efficient than Queries since
they skip the scoring process and are automatically cached. You can set
the `_cache` element on a filter to explicitly control caching. Learn more
about the [Filter DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-filters.html)

In general, use filters for:

1. Binary yes/no searches.
2. Queries on exactly values.


## Examples

Useful endpoints.
```bash
# '_search': Searches through indices, document types, or documents.
curl 'localhost:9200/_search'
curl 'localhost:9200/articles/_search'
curl 'localhost:9200/articles/header/_search'

# '_mapping': Displays the schema mapping of an index or document.
curl 'localhost:9200/articles/_mapping'

# '_stats': Displays statistics and usage report.
curl 'localhost:9200/_stats'
```


Useful query string parameters.
```bash
# 'q': Single search query.
curl 'localhost:9200/articles/_search?q=text:awesome'

# 'q': Multiple search queries.
curl 'localhost:9200/articles/_search?q=text:mayor+language:es'

# 'fields': Limits the fields returned
curl 'localhost:9200/articles/header/1?fields=slug,language'

# 'size': Specifies the number of hits returned (default: 10)
curl 'localhost:9200/articles/_search?size=1'

# 'from': Offset for result (default: 0)
curl 'localhost:9200/articles/_search?from=5'

# 'pretty': Returns formatted JSON
curl 'localhost:9200/articles/_mapping?pretty=true'

# 'sort': Sorts based on your fields.
https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-sort.html

# 'aggregations': Aggregates data.
https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html
```

