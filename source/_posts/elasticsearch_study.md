---
title: Elastic Search 学习

date: 2020-11-20

tags: 
  - Elastic Search

categories:
  - programming
---

## Elastic Search 学习


```python
from elasticsearch import Elasticsearch as ES
es = ES('http://localhost',port = 9200)
```


```python
import requests as r
import json
def url(src:str)->str:
    return f'http://localhost:9200{src}?pretty=true'
p = {"pretty":"true"}
```

>   本文档包含了从快速部署开始到一些较为高级的用法的演示和说明，使用环境是Windows，使用Power Shell作为命令行环境，在jupyter notebook中进行测试

## 快速部署（Docker）

``` powershell
# 安装Elasticsearch
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.9.3
# 运行单节点
docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.9.3 --name elastic_search
```

* 测试cluster


```python
print(r.get(url('/')).text)
```

    {
      "name" : "f0a5b2ffad4a",
      "cluster_name" : "docker-cluster",
      "cluster_uuid" : "NIdw38XdQBi5rmtiroeIVA",
      "version" : {
        "number" : "7.9.2",
        "build_flavor" : "default",
        "build_type" : "docker",
        "build_hash" : "d34da0ea4a966c4e49417f2da2f244e3e97b4e6e",
        "build_date" : "2020-09-23T00:45:33.626720Z",
        "build_snapshot" : false,
        "lucene_version" : "8.6.2",
        "minimum_wire_compatibility_version" : "6.8.0",
        "minimum_index_compatibility_version" : "6.0.0-beta1"
      },
      "tagline" : "You Know, for Search"
    }



### 安装Kibana
``` powershell
docker pull docker.elastic.co/kibana/kibana:7.9.3
# 启动容器并链接elasticsearch
docker run --link elastic_search:elasticsearch -p 5601:5601 docker.elastic.co/kibana/kibana:7.9.3
# 测试Kibana
# 浏览器能访问5601端口即可
```

## 基本概念

### Node & Cluster

Elastic 是一个分布式数据库，单个Elastic实例为一个节点（Node），若干节点组成一个集群（Cluster）。

### Index

类似于SQL数据库的database,索引（Index）是数据管理的一级单位，**index名必须小写**

Elastic索引所有字段，经过处理后产生一个反向索引（Inverted Index）用以在查询的时候搜索文档。
* 新建一个索引


```python
print(r.put(url('/todo')).text)
```

    {
      "error" : {
        "root_cause" : [
          {
            "type" : "resource_already_exists_exception",
            "reason" : "index [todo/BShbis0RR3aA-X3wdcD2cQ] already exists",
            "index_uuid" : "BShbis0RR3aA-X3wdcD2cQ",
            "index" : "todo"
          }
        ],
        "type" : "resource_already_exists_exception",
        "reason" : "index [todo/BShbis0RR3aA-X3wdcD2cQ] already exists",
        "index_uuid" : "BShbis0RR3aA-X3wdcD2cQ",
        "index" : "todo"
      },
      "status" : 400
    }



* 删除一个索引


```python
print(r.delete(url('/todo')).text)
```

    {
      "acknowledged" : true
    }



* 查阅Node下所有Index 


```python
print(r.get(url('/_cat/indices')).text)
```

    green  open .kibana-event-log-7.9.2-000001 6z4diegrSYusecVp-8YnJA 1 0     1  0   5.5kb   5.5kb
    green  open .apm-custom-link               70A0r6x_To2MvZXsyGUTlA 1 0     0  0    208b    208b
    green  open .kibana_task_manager_1         Bn7h59PXSQqqWaJT21idlw 1 0     6 68 137.9kb 137.9kb
    green  open .kibana-event-log-7.9.2-000002 KDsEyuhwR6K1pngNoseQag 1 0     1  0   5.5kb   5.5kb
    green  open .apm-agent-configuration       XhYtj6-qRpC8-TLAo85ZYg 1 0     0  0    208b    208b
    green  open kibana_sample_data_logs        0Ov2Tb15QYOHrSAhd5VbvQ 1 0 14074  0  10.3mb  10.3mb
    yellow open gb                             zgJK4tsDR96ok9yXBFfOBA 1 1     1  0     5kb     5kb
    green  open .async-search                  S6V9_mcIRO2ADOl3ZZB7kw 1 0     0  0    234b    234b
    green  open .kibana_1                      eYLHd8UzTcOupTc_QJRIEQ 1 0    73  0  10.4mb  10.4mb
    yellow open us                             w6zNSJ7uQta8gsbPnpkTUw 1 1     1  0     5kb     5kb



### Type

文档的虚拟的逻辑分组，可以用于过滤文档，一个索引下的文档应当具有相似的结构（Schema）

### Document

文档（Document）类似SQL数据库里的一个记录（Record），一个索引由多个文档组成。

## 接口和功能说明

### 文档操作演示

>   尝试建立并查询一个索引，以此加深对Index，Type和Document的概念


```python
# 新建一个doc
print(r.put(url('/todo/things/0'),json={
    'title':'待办事项A',
    'priority':0.6,
    'date':'2020-11-01',
    'assignment':['aicy','rin']
}).text)
print(r.put(url('/todo/things/1'),json={
    'title':'待办事项B_backup',
    'priority':0.5,
    'date':'2020-11-02',
    'assignment':['hanyuu','rin']
}).text)
```

    {
      "_index" : "todo",
      "_type" : "things",
      "_id" : "0",
      "_version" : 1,
      "result" : "created",
      "_shards" : {
        "total" : 2,
        "successful" : 1,
        "failed" : 0
      },
      "_seq_no" : 0,
      "_primary_term" : 1
    }
    
    {
      "_index" : "todo",
      "_type" : "things",
      "_id" : "1",
      "_version" : 1,
      "result" : "created",
      "_shards" : {
        "total" : 2,
        "successful" : 1,
        "failed" : 0
      },
      "_seq_no" : 1,
      "_primary_term" : 1
    }




```python
print(r.post(url('/todo/things/'),json={
    'title':'待办事项C',
    'priority':0.8,
    'date':'2020-11-02',
    'assignment':['aicy','rin']
}).text)
```

    {
      "_index" : "todo",
      "_type" : "things",
      "_id" : "qLE2A3YBPtTv4vAuC6e7",
      "_version" : 1,
      "result" : "created",
      "_shards" : {
        "total" : 2,
        "successful" : 1,
        "failed" : 0
      },
      "_seq_no" : 2,
      "_primary_term" : 1
    }




```python
# 查询doc
print(r.get(url('/todo/things/0')).text)
```

    {
      "_index" : "todo",
      "_type" : "things",
      "_id" : "0",
      "_version" : 1,
      "_seq_no" : 0,
      "_primary_term" : 1,
      "found" : true,
      "_source" : {
        "title" : "待办事项A",
        "priority" : 0.6,
        "date" : "2020-11-01",
        "assignment" : [
          "aicy",
          "rin"
        ]
      }
    }




```python
# 查询schema
print(r.get(url('/todo/')).text)
```

    {
      "todo" : {
        "aliases" : { },
        "mappings" : {
          "properties" : {
            "assignment" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "date" : {
              "type" : "date"
            },
            "priority" : {
              "type" : "float"
            },
            "title" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            }
          }
        },
        "settings" : {
          "index" : {
            "creation_date" : "1606371641662",
            "number_of_shards" : "1",
            "number_of_replicas" : "1",
            "uuid" : "aKIUpjivSWGDCrgYoWUy0w",
            "version" : {
              "created" : "7090299"
            },
            "provided_name" : "todo"
          }
        }
      }
    }



>   使用post可以不指定ip新增文档，elastic回返回一个随机字符串
>
>   使用PUT，POST，GET，DELETE动词进行新增，更新，查询，删除操作

### 数据查询

#### 返回所有记录


```python
print(r.get(url('/todo/things/_search')).text)
```

    {
      "took" : 3,
      "timed_out" : false,
      "_shards" : {
        "total" : 1,
        "successful" : 1,
        "skipped" : 0,
        "failed" : 0
      },
      "hits" : {
        "total" : {
          "value" : 0,
          "relation" : "eq"
        },
        "max_score" : null,
        "hits" : [ ]
      }
    }




```python
# 轻量搜索
print(r.get(url('/todo/things/_search?q=title:"待办事项B_backup"'),params = p).text)
```

    {
      "took" : 287,
      "timed_out" : false,
      "_shards" : {
        "total" : 1,
        "successful" : 1,
        "skipped" : 0,
        "failed" : 0
      },
      "hits" : {
        "total" : {
          "value" : 0,
          "relation" : "eq"
        },
        "max_score" : null,
        "hits" : [ ]
      }
    }



#### 全文搜索

>   elastic使用DSL进行索引


```python
# 搜索单字段
print(r.get(url("/todo/things/_search"),json = {
    "query": {
    "match": {
        "title": "待办事项"
    }
    },
    "from": 0,
    "size": 10
}).text)
```

    {
      "took" : 22,
      "timed_out" : false,
      "_shards" : {
        "total" : 1,
        "successful" : 1,
        "skipped" : 0,
        "failed" : 0
      },
      "hits" : {
        "total" : {
          "value" : 3,
          "relation" : "eq"
        },
        "max_score" : 0.53412557,
        "hits" : [
          {
            "_index" : "todo",
            "_type" : "things",
            "_id" : "0",
            "_score" : 0.53412557,
            "_source" : {
              "title" : "待办事项A",
              "priority" : 0.6,
              "date" : "2020-11-01",
              "assignment" : [
                "aicy",
                "rin"
              ]
            }
          },
          {
            "_index" : "todo",
            "_type" : "things",
            "_id" : "1",
            "_score" : 0.53412557,
            "_source" : {
              "title" : "待办事项B_backup",
              "priority" : 0.5,
              "date" : "2020-11-02",
              "assignment" : [
                "hanyuu",
                "rin"
              ]
            }
          },
          {
            "_index" : "todo",
            "_type" : "things",
            "_id" : "qLE2A3YBPtTv4vAuC6e7",
            "_score" : 0.53412557,
            "_source" : {
              "title" : "待办事项C",
              "priority" : 0.8,
              "date" : "2020-11-02",
              "assignment" : [
                "aicy",
                "rin"
              ]
            }
          }
        ]
      }
    }




```python
# 搜索多字段
print(r.get(url('/todo/things/_search'),json = {
    "query":{
        "bool":{
            "must":{
                "multi_match":{
                "query":"hanyuu",
                "fields":[
                    "title","assignment"
                ]
                }
            },
            "should":{
                "range":{
                    "priority":{
                        "gt":0.2
                    }
                }
            }
        }
    }
}).text)
```

    {
      "took" : 2,
      "timed_out" : false,
      "_shards" : {
        "total" : 1,
        "successful" : 1,
        "skipped" : 0,
        "failed" : 0
      },
      "hits" : {
        "total" : {
          "value" : 1,
          "relation" : "eq"
        },
        "max_score" : 1.9808291,
        "hits" : [
          {
            "_index" : "todo",
            "_type" : "things",
            "_id" : "1",
            "_score" : 1.9808291,
            "_source" : {
              "title" : "待办事项B_backup",
              "priority" : 0.5,
              "date" : "2020-11-02",
              "assignment" : [
                "hanyuu",
                "rin"
              ]
            }
          }
        ]
      }
    }



### 多条件搜索逻辑运算


```python
# 与逻辑
print(r.get(url('/todo/things/_search'),json = {
    "query":{
        "bool":{
            "must":[
                {
                    "match":{
                        "title":"待办事项"
                    },
                    "match":
                    {
                        "priority": 0.5
                    }
                }
            ]
        }
    }
}).text)
```

    {
      "took" : 6,
      "timed_out" : false,
      "_shards" : {
        "total" : 1,
        "successful" : 1,
        "skipped" : 0,
        "failed" : 0
      },
      "hits" : {
        "total" : {
          "value" : 1,
          "relation" : "eq"
        },
        "max_score" : 1.0,
        "hits" : [
          {
            "_index" : "todo",
            "_type" : "things",
            "_id" : "1",
            "_score" : 1.0,
            "_source" : {
              "title" : "待办事项B_backup",
              "priority" : 0.5,
              "date" : "2020-11-02",
              "assignment" : [
                "hanyuu",
                "rin"
              ]
            }
          }
        ]
      }
    }




```python
# SQL
print(r.post(url('/_sql'),params = {"format":"txt"},json = {
    "query":"select title,priority from todo where priority>0"
}).text)
print(r.post(url('/_sql'),json = {
    "query":"select title,priority from todo where priority>0"
}).text)
```

         title     |     priority     
    ---------------+------------------
    å¾åäºé¡¹A          |0.6000000238418579
    å¾åäºé¡¹B_backup   |0.5               
    å¾åäºé¡¹C          |0.800000011920929 
    
    {
      "columns" : [
        {
          "name" : "title",
          "type" : "text"
        },
        {
          "name" : "priority",
          "type" : "float"
        }
      ],
      "rows" : [
        [
          "待办事项A",
          0.6000000238418579
        ],
        [
          "待办事项B_backup",
          0.5
        ],
        [
          "待办事项C",
          0.800000011920929
        ]
      ]
    }



### 合法性和错误提示以及合法解释


```python

print(r.get(url('/todo/things/_validate/query'),params = {"explain":"true"},json = {
        "query":{
        "bool":{
            "must":{
                "multi_match":{
                "query":"hanyuu",
                "fields":[
                    "title","assignment"
                ]
                }
            },
            "should":{
                "range":{
                    "priority":{
                        "gt":0.2
                    }
                }
            }
        }
    }
}).text)
```

    {
      "_shards" : {
        "total" : 1,
        "successful" : 1,
        "failed" : 0
      },
      "valid" : true,
      "explanations" : [
        {
          "index" : "todo",
          "valid" : true,
          "explanation" : "+(+(assignment:hanyuu | title:hanyuu) priority:[0.20000002 TO Infinity]) #*:*"
        }
      ]
    }



### 排序与相关性

### Elastic Search中的数据结构

| 数据结构                       | 优缺点                                                       |
| ------------------------------ | ------------------------------------------------------------ |
| 排序列表Array/List             | 使用二分法查找，不平衡                                       |
| HashMap/TreeMap                | 性能高，内存消耗大，几乎是原始数据的三倍                     |
| Skip List                      | 跳跃表，可快速查找词语，在lucene、redis、Hbase等均有实现。相对于TreeMap等结构，特别适合高并发场景（[Skip List介绍](http://kenby.iteye.com/blog/1187303)） |
| Trie                           | 适合英文词典，如果系统中存在大量字符串且这些字符串基本没有公共前缀，则相应的trie树将非常消耗内存（[数据结构之trie树](http://dongxicheng.org/structure/trietree/)） |
| Double Array Trie              | 适合做中文词典，内存占用小，很多分词工具均采用此种算法（[深入双数组Trie](http://blog.csdn.net/zhoubl668/article/details/6957830)） |
| Ternary Search Tree            | 三叉树，每一个node有3个节点，兼具省空间和查询快的优点（[Ternary Search Tree](http://www.drdobbs.com/database/ternary-search-trees/184410528)） |
| Finite State Transducers (FST) | 一种有限状态转移机，Lucene 4有开源实现，并大量使用           |

### Kibana

## 参考文档

* Elasticsearch: The Definitive Guide by Clinton Gormley and Zachary Tong (O’Reilly). Copyright 2015 Elasticsearch BV, 978-1-449-35854-9。
*   [Install Elasticsearch with Docker](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)
*   [全文搜索引擎 Elasticsearch 入门教程 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/32843298)
*   [ElasticSearch中的数据结构](https://blog.csdn.net/whichard/article/details/90753727)

