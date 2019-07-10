### 简介

Elasticsearch是分布式搜索和分析引擎.是Elastic Stack的核心

* 功能与特性
  * distributed
  * real-time search
  * analysis

### 基础概念

参考官方文档[Mapping concepts across SQL and Elasticsearch | Elasticsearch Reference [7.2] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/_mapping_concepts_across_sql_and_elasticsearch.html#_mapping_concepts_across_sql_and_elasticsearch)

|SQL|Elasticsearch|中文|描述|
|:-------------:|--|--|-----|
|database|cluster instance|集群实例|SQL中database表示多个tables的集合. Elasticsearch中"可用的索引的集合"被分组到一个cluster(集群)中. 一个Elasticsearch cluster(集群)包含至少一个Elasticsearch instance(实例).|
|table|index|索引|SQL中查询针对的是表(table). Elasticsearch中查询针对的是索引(index)|
|row|document|文档|SQL中的行(row)是严格的. Elasticsearch中的文档(document)更灵活松散,同时具有结构|
|column|field|字段|/|


集群:一个集群是由一个或多个节点组成的集合，集群上的节点将会存储数据并提供跨节点的索引和搜索功能.

分布式部署:通常一个Elasticsearch cluster(集群)包含了多个Elasticsearch instance.

节点:集群通过一个唯一的名称作为标识.节点通过设置"集群名称"就可以加入相应的集群.
