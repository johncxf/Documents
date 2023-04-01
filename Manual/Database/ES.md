# Elasticsearch

Elasticsearch（简称 ES） 是一个实时的分布式搜索分析引擎。

学习资料：

- 官网文档：https://www.elastic.co/guide/cn/elasticsearch/guide/current/getting-started.html

## 安装配置

ES 依赖 java 环境，需要先安装 java 环境。

### MacOS

#### Homebrew 安装

```shell
$ brew tap elastic/tap
$ brew install elastic/tap/elasticsearch-full

# 查看 es 版本
$ elasticsearch -version
```

参考：https://www.elastic.co/guide/en/elasticsearch/reference/7.17/brew.html

**目录**：

- home：/usr/local/var/homebrew/linked/elasticsearch-full
- bin：/usr/local/var/homebrew/linked/elasticsearch-full/bin
- conf：`/usr/local/etc/elasticsearch`
- data：/usr/local/var/lib/elasticsearch
- logs：/usr/local/var/log/elasticsearch
- plugins：/usr/local/var/homebrew/linked/elasticsearch/plugins

**运行**：

```shell
# 启动
$ brew services start elasticsearch-full

# 停止
$ brew services stop elasticsearch-full

# 重启
$ brew services restart elasticsearch-full
```

启动后在浏览器输入：

```
http://localhost:9200
```

返回：

```json
{
    "name": "...",
    "cluster_name": "...",
    "cluster_uuid": "...",
    "version": {
        "number": "7.17.4",
        "build_flavor": "default",
        "build_type": "tar",
        "build_hash": "79878662c54c886ae89206c685d9f1051a9d6411",
        "build_date": "2022-05-18T18:04:20.964345128Z",
        "build_snapshot": false,
        "lucene_version": "8.11.1",
        "minimum_wire_compatibility_version": "6.8.0",
        "minimum_index_compatibility_version": "6.0.0-beta1"
    },
    "tagline": "You Know, for Search"
}
```

#### 下载安装

在官网下载对应版本压缩包，并解压到自定义目录便算安装完成了

- https://www.elastic.co/cn/downloads/elasticsearch

**运行**：

```shell
# 进入根目录
$ cd elasticsearch-<version>

# 前台启动 Elasticsearch
$ ./bin/elasticsearch

# 后台启动 Elasticsearch
$ ./bin/elasticsearch -d
```

测试 Elasticsearch 是否启动成功，可以打开另一个终端，执行以下操作：

```shell
$ curl 'http://localhost:9200/?pretty'
```

## 基础入门

### 查用查询

```sh
# 列出所有索引，类似于关系型数据库中：show databases
GET /_cat/indices?v&pretty
```

### 名称解释

#### 集群

一个集群就是由一个或多个节点组织在一起， 它们共同持有你全部的数据， 并一起提供索引和搜索功能。

#### 节点

一个节点是你集群中的一个服务器，作为集群的一部分，它存储你的数据，参与集群的索引和搜索功能。

#### 索引

**索引（名词）：**

一个 *索引* 类似于传统关系数据库中的一个 *数据库* ，是一个存储关系型文档的地方。 *索引* (*index*) 的复数词为 *indices* 或 *indexes* 。

**索引（动词）：**

*索引一个文档* 就是存储一个文档到一个 *索引* （名词）中以便被检索和查询。这非常类似于 SQL 语句中的 `INSERT` 关键词，除了文档已存在时，新文档会替换旧文档情况之外。

**倒排索引：**

关系型数据库通过增加一个 *索引* 比如一个 B树（B-tree）索引 到指定的列上，以便提升数据检索速度。Elasticsearch 和 Lucene 使用了一个叫做 *倒排索引* 的结构来达到相同的目的。一个文档中的每一个属性都是 *被索引* 的（有一个倒排索引）和可搜索的。一个没有倒排索引的属性是不能被搜索到的。

#### 类型

在一个索引中，你可以定义一种或多种类型。一个类型是你的索引的一个逻辑上的分类/分区，其语义完全由你来定。一个类型类似于传统关系数据库中的一张表。

#### 文档

一个文档是一个可被索引的基础信息单元，文档以JSON格式来表示。一个文档类型关系型数据库一条记录。

### API

```shell
curl -X<VERB> '<PROTOCOL>://<HOST>:<PORT>/<PATH>?<QUERY_STRING>' -d '<BODY>'
```

| 参数           | 注释                                                         |
| -------------- | ------------------------------------------------------------ |
| `VERB`         | 适当的 HTTP *方法* 或 *谓词* : `GET`、 `POST`、 `PUT`、 `HEAD` 或者 `DELETE`。 |
| `PROTOCOL`     | `http` 或者 `https`（如果你在 Elasticsearch 前面有一个 `https` 代理） |
| `HOST`         | Elasticsearch 集群中任意节点的主机名，或者用 `localhost` 代表本地机器上的节点。 |
| `PORT`         | 运行 Elasticsearch HTTP 服务的端口号，默认是 `9200` 。       |
| `PATH`         | API 的终端路径（例如 `_count` 将返回集群中文档数量）。Path 可能包含多个组件，例如：`_cluster/stats` 和 `_nodes/stats/jvm` 。 |
| `QUERY_STRING` | 任意可选的查询字符串参数 (例如 `?pretty` 将格式化地输出 JSON 返回值，使其更容易阅读) |
| `BODY`         | 一个 JSON 格式的请求体 (如果请求需要的话)                    |

示例：

```shell
curl -XGET 'http://localhost:9200/_count?pretty' -d '
{
    "query": {
        "match_all": {}
    }
}'
```

请求返回 HTTP 信息：

```shell
curl -i -XGET 'localhost:9200/'
```

### Demo

#### 给每个员工索引一个文档

```sh
curl -H 'Content-Type: application/json' -XPUT 'http://localhost:9200/megacorp/employee/1?pretty' -d '{
    "first_name" : "John",
    "last_name" :  "Smith",
    "age" :        25,
    "about" :      "I love to go rock climbing",
    "interests": [ "sports", "music" ]
}'

curl -H 'Content-Type: application/json' -XPUT 'http://localhost:9200/megacorp/employee/2?pretty' -d '
{
    "first_name" :  "Jane",
    "last_name" :   "Smith",
    "age" :         32,
    "about" :       "I like to collect rock albums",
    "interests":  [ "music" ]
}
'

curl -H 'Content-Type: application/json' -XPUT 'http://localhost:9200/megacorp/employee/3?pretty' -d '
{
    "first_name" :  "Douglas",
    "last_name" :   "Fir",
    "age" :         35,
    "about":        "I like to build cabinets",
    "interests":  [ "forestry" ]
}
'
```

#### 检索每个员工的信息

```sh
$ curl 'http://localhost:9200/megacorp/employee/1?pretty'
```

#### 轻量搜索

搜索所有员工：

```sh
$ curl 'http://localhost:9200/megacorp/employee/_search?pretty'
```

搜索姓氏为 ``Smith`` 的员工：

```sh
$ curl 'http://localhost:9200/megacorp/employee/_search?q=last_name:Smith&pretty'
```

#### 查询表达式搜索

Elasticsearch 提供一个丰富灵活的查询语言叫做 *查询表达式* ， 它支持构建更加复杂和健壮的查询。

*领域特定语言* （DSL）， 使用 JSON 构造了一个请求。我们可以像这样重写之前的查询所有名为 Smith 的搜索 ：

```sh
$ curl -H 'Content-Type: application/json' 'http://localhost:9200/megacorp/employee/_search?pretty' -d '
{
    "query" : {
        "match" : {
            "last_name" : "Smith"
        }
    }
}
'
```

返回结果与之前的查询一样

#### 更复杂的搜索

搜索姓氏为 Smith 的员工，且年龄大于 30 的。查询需要稍作调整，使用过滤器 *filter* ，它支持高效地执行一个结构化查询

```sh
$ curl -H 'Content-Type: application/json' 'http://localhost:9200/megacorp/employee/_search?pretty' -d '
{
    "query" : {
        "bool": {
            "must": {
                "match" : {
                    "last_name" : "smith" 
                }
            },
            "filter": {
                "range" : {
                    "age" : { "gt" : 30 } 
                }
            }
        }
    }
}
'
```

#### 全文搜索

搜索下所有喜欢攀岩（rock climbing）的员工：

```sh
$ curl -H 'Content-Type: application/json' 'http://localhost:9200/megacorp/employee/_search?pretty' -d '
{
    "query" : {
        "match" : {
            "about" : "rock climbing"
        }
    }
}
'

# response：
{
  ...
  "hits" : {
    "total" : {
      "value" : 2,
      "relation" : "eq"
    },
    "max_score" : 1.4167401,
    "hits" : [
      {
        "_index" : "megacorp",
        "_type" : "employee",
        "_id" : "1",
        "_score" : 1.4167401,
        "_source" : {
          "first_name" : "John",
          "last_name" : "Smith",
          "age" : 25,
          "about" : "I love to go rock climbing",
          "interests" : [
            "sports",
            "music"
          ]
        }
      },
      {
        "_index" : "megacorp",
        "_type" : "employee",
        "_id" : "2",
        "_score" : 0.4589591,
        "_source" : {
          "first_name" : "Jane",
          "last_name" : "Smith",
          "age" : 32,
          "about" : "I like to collect rock albums",
          "interests" : [
            "music"
          ]
        }
      }
    ]
  }
}
```

Elasticsearch 默认按照相关性得分排序，即每个文档跟查询的匹配程度。第一个最高得分的结果很明显：John Smith 的 `about` 属性清楚地写着 “rock climbing” 。

但为什么 Jane Smith 也作为结果返回了呢？原因是她的 `about` 属性里提到了 “rock” 。因为只有 “rock” 而没有 “climbing” ，所以她的相关性得分低于 John 的。

#### 短语搜索

仅匹配同时包含 “rock” *和* “climbing” ，*并且* 二者以短语 “rock climbing” 的形式紧挨着的雇员记录

为此对 `match` 查询稍作调整，使用一个叫做 `match_phrase` 的查询：

```sh
$ curl -H 'Content-Type: application/json' 'http://localhost:9200/megacorp/employee/_search?pretty' -d '
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    }
}
'
```

返回结果仅有 John Smith 的文档：

```json
{
  ...
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.4167401,
    "hits" : [
      {
        "_index" : "megacorp",
        "_type" : "employee",
        "_id" : "1",
        "_score" : 1.4167401,
        "_source" : {
          "first_name" : "John",
          "last_name" : "Smith",
          "age" : 25,
          "about" : "I love to go rock climbing",
          "interests" : [
            "sports",
            "music"
          ]
        }
      }
    ]
  }
}
```

#### 高亮搜索

许多应用都倾向于在每个搜索结果中 *高亮* 部分文本片段，以便让用户知道为何该文档符合查询条件。在 Elasticsearch 中检索出高亮片段也很容易。

再次执行前面的查询，并增加一个新的 `highlight` 参数：

```sh
$ curl -H 'Content-Type: application/json' 'http://localhost:9200/megacorp/employee/_search?pretty' -d '
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    },
    "highlight": {
        "fields" : {
            "about" : {}
        }
    }
}
'
```

当执行该查询时，返回结果与之前一样，与此同时结果中还多了一个叫做 `highlight` 的部分。这个部分包含了 `about` 属性匹配的文本片段，并以 HTML 标签 `<em></em>` 封装：

```json
{
   ...
   "hits": {
      "total":      1,
      "max_score":  0.23013961,
      "hits": [
         {
            ...
            "_score":         0.23013961,
            "_source": {
               "first_name":  "John",
               "last_name":   "Smith",
               "age":         25,
               "about":       "I love to go rock climbing",
               "interests": [ "sports", "music" ]
            },
            "highlight": {
               "about": [
                  "I love to go <em>rock</em> <em>climbing</em>" 
               ]
            }
         }
      ]
   }
}
```

#### 聚合查询

Elasticsearch 有一个功能叫聚合（aggregations），允许我们基于数据生成一些精细的分析结果。聚合与 SQL 中的 `GROUP BY` 类似但更强大。

挖掘出员工中最受欢迎的兴趣爱好：

```sh
$ curl -X GET 'http://localhost:9200/megacorp/employee/_search?pretty' -H 'Content-Type: application/json' -d'
{
  "aggs": {
    "all_interests": {
      "terms": { "field": "interests.keyword" }
    }
  }
}
'
```

返回：

```json
{
   ...
   "hits": { ... },
   "aggregations": {
      "all_interests": {
         "buckets": [
            {
               "key":       "music",
               "doc_count": 2
            },
            {
               "key":       "forestry",
               "doc_count": 1
            },
            {
               "key":       "sports",
               "doc_count": 1
            }
         ]
      }
   }
}
```

可以看到，两位员工对音乐感兴趣，一位对林业感兴趣，一位对运动感兴趣

如果想知道叫 Smith 的员工中最受欢迎的兴趣爱好，可以直接构造一个组合查询：

```sh
$ curl -X GET 'http://localhost:9200/megacorp/employee/_search?pretty' -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "last_name": "smith"
    }
  },
  "aggs": {
    "all_interests": {
      "terms": {
        "field": "interests.keyword"
      }
    }
  }
}
'
```

返回：

```json
...
"all_interests": {
 "buckets": [
    {
       "key": "music",
       "doc_count": 2
    },
    {
       "key": "sports",
       "doc_count": 1
    }
 ]
}
```

聚合还支持分级汇总 。比如，查询特定兴趣爱好员工的平均年龄：

```sh
$ curl -X GET 'http://localhost:9200/megacorp/employee/_search?pretty' -H 'Content-Type: application/json' -d'
{
    "aggs" : {
        "all_interests" : {
            "terms" : { "field" : "interests.keyword" },
            "aggs" : {
                "avg_age" : {
                    "avg" : { "field" : "age" }
                }
            }
        }
    }
}
'
```

返回：

```json
...
"all_interests": {
 "buckets": [
    {
       "key": "music",
       "doc_count": 2,
       "avg_age": {
          "value": 28.5
       }
    },
    {
       "key": "forestry",
       "doc_count": 1,
       "avg_age": {
          "value": 35
       }
    },
    {
       "key": "sports",
       "doc_count": 1,
       "avg_age": {
          "value": 25
       }
    }
 ]
}
```

