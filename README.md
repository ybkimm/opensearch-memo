# opensearch-memo

Personal note about using OpenSearch (or Elasticsearch)


# Getting Started

TBD (まだ開発端末が届いてない、、、）


# Document

何かを表すJSONデータ


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



