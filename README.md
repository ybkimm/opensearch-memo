# opensearch-memo

Personal note about using OpenSearch (or Elasticsearch)


# Getting Started

TBD (まだ開発端末が届いてない、、、）


# Index

Documentの集合

```
# Create new one
PUT /put_index_name_here
{
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 2 
  }
}

# Delete existing one
DELETE /put_index_name_here
```


## Shards

Indexは複数のShardを含めていて、データは分散される。

これによって、
- 負荷分散
- データ分散（物理的なストレージより大きいデータを格納することが可能）

ただ、複製ではないのでノードが死んだらそのノードに格納されているデータも消えてしまう


## Replication

Shardの複製

別ノードに作成されるため、データ可用性上昇や負荷分散、
同じノードに複数のReplicationを作成することでThroughput上昇の効果もある
ㄴ 基本的にはSingle Thread？


# Document

```
# Create new document
POST /put_index_name_here/_doc
{
  ...
}

# Replace the existing document, create new one with given ID if not exists yet
PUT /put_index_name_here/_doc/put_document_id_here
{
  ...
}

# Retrieve the document
GET /put_index_name_here/_doc/put_document_id_here

# Update the document
# Note that the document can not be **updated**, because document is immutable
# Update API just *replaces* the document.
POST /put_index_name_here/_update/put_document_id_here
{
  ...
}

# Delete the document
DELETE /put_index_name_here/_update/put_document_id_here
```


## Script

```
POST /put_index_name_here/_update/put_document_id_here
{
  "script": {
    "source": "do_something_with_script(ctx)"
  }
}

# With parameters
POST /put_index_name_here/_update/put_document_id_here
{
  "script": {
    "source": "do_something_with_script(ctx)",
    "params": {
      "foo": "bar",
    }
  }
}
```

スクリプト実行後、`result`はいつも`updated`となる

`ctx.op`を、
- `"noop"`にすると、`result`が`noop`になる
- `"delete"`にすると、スクリプト実行後Documentが削除される


## Upsert

Documentが存在しない場合、スクリプトを実行する代わりに新しいDocumentを作成することができる

```
POST /put_index_name_here/_update/put_document_id_here
{
  "script": { ... },
  "upsert": {
    // Documentの内容
  }
}
```

Indexに該当するDocumentが存在しない場合、新しいDocumentが作成される

`result`は`created`になる













