

## ES 里术语及基本概念

es 的数据是如何组织的

逻辑设计

索引和搜索的基本单位是文档，文档以类型来分组，类型包含若干文档，一个或多个类型存在于同一个索引中

索引->类型->文档

物理设计

每个索引划分为分片



## 4搜索数据



### 查询方式和回复结构

#### 查询结构

query

get 方法

q

post 方法

请求示例：

{{es}}/staging_solola_task/solola/_search

指定 from->offset 和 size->limit

```json
{
    "query": {
      "match_all":{}
    },
  "from":10,
  "size":10
}
```

可以指定"_source",即你期望返回的字段，可以使用通配符

```json
{
  "query":{
    "match_all":{}
  },
  "_source":{
    "include":["location.*","date"],
    "exclude":["location.geolocation"]
  }
}
```

"location.*" -> 以"location"开头的字段

结果排序

```json
{
  "query":{
    "match_all":{}
  },
  "sort":[
    {"created_on":"asc"},
    {"name":"desc"},
    "_score"
  ]
}
```

综上可以构建出如下一个涵盖了基本搜索功能的查询：

```json
{
  "query":{
    "match_all":{}
  },
  "from":0,
  "to":10,
  "_source":["name","organizer","description"],
  "sort":[
    {"created_on":"asc"}
  ]
}
```



#### 回复结构

```json
{
    "took": 52,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": 10,
        "max_score": 3,
        "hits": [
            {
                "_index": "staging_solola_task",
                "_type": "solola",
                "_id": "830113324998556133",
                "_score": 3,
                "_source": {
                    "date": "2020-08-04",
                  	"title":" introduction to elasticsearch"
                }
            }
        ]
    }
}
```



- took:查询所使用毫秒数
- time_out:是否有分片超时，即是否只返回了部分结果
- _shards：成功响应的分片数量
- hits：命中的文档数组
  - totla:命中总数
  - max_score:搜索结果中的最大得分



### 查询和过滤器 DSL



过滤器不计算得分，需要的处理更少，并且会缓存匹配

查询和过滤器

查询示例：

```json
{
  "query":{
    "match":{
      "title":"CNN"
    }
  }
}
```

过滤器查询：

```json
{
    "query": {
        "filtered": {
            "query": {
                "match": {
                    "title": "CNN"
                }
            },
          "filter":{
            "term":{
              "host":"cx"
            }
          }
        }
    }
}
```





## 5分析数据

分析是在文档被发送并加入倒排索引之前，ES 在起主体上进行的操作。在文档被加入索引之前，ES 让每个被分析字段经过一系列的处理步骤：

- 字符过滤——使用字符过滤器转变字符

- 文本切分为分词——将文本切分为单个或多个分词

- 分词过滤——使用分词过滤器转变每个分词

- 分词索引——将这些分词存储到索引中

  

## 6使用相关性进行搜索



TF-IDF(TF term frequency 词频；IDF inverse document frequency 逆文档频率)，一个词条出现在某个文档中的次数越多，它就越相关，但是，如果该词条出现在不同的文档次数越多，它就越不相关。

> 逆文档频率检查一个单词是否出现在某篇文档中，而不是在该文档中出现多少次

如何配置 ES 来使用不同的相似度计算方式

一 修改某个字段映射中的 similarity 参数：

```json
{
  "mappings":{
    "get-together":{
      "properties":{
        "title":{
          "type":"string",
          "similarity":"BM25"
        }
      }
    }
  }
}
```

二 在字段的映射中指定一个扩展：

```json
{　
  "settings":{
    "index":{
      "analysis":{
        ...　　　
      },
        "similarity":{　
          "my_custom_similarity":{
              "type":"BM25",
              "k1":1.2,
               "b":0.75,　　　　　
               "discount_overlaps":false　　　　
          }　　　
        }　　
      }　
    },
    "mappings":{　
      "mytype":{　
        "properties":{　
          "title":{　　　
            "type":"string",　　
            "similarity":"my_custom_similarity"
          }
        }
      }
    }
  }
```

如果决定总是使用某种特定的打分方法，可以全局性的配置它，在 elasticsearch.yml 配置文件中加入下面的设置：

```yml
index.similarity.default.type:BM25
```

### 使用解释来理解文档是如何被评分的

在请求主体中设置“解释”标志：

```json
{
  "query":{
    "match":{
      "title":"CNN"
    }
  },
  "explain":true
}
```

### 使用查询再打分来减小评分操作的性能影响

使用rescore特性：

```json
{
  "query":{
    "match":{
      "title":"CNN"
    }
  },
  "resocre":{
    "window_size":20,
    "query":{
      "resocre_query":{
        "match":{
          "title":{
            "type":"phrase",
            "query":"elasticsearch hadoop",
            "slop":5
          }
        }
      },
      "query_weight":0.8,
      "rescore_query_weight":1.3
    }
  }
}
```

### 使用 function_score 来定制得分

function_score 允许用户指定任意数量的的任意函数，让它们作用于匹配了初始查询的文档，修改其得分，从而达到精细化控制结果相关性的目的，

如果定义了一组函数，如何合并得分：

- 函数查询之间如何合并（score_mode）
- 函数查询与原查询如何合并 {boost_mode}

function_score 查询的基本结构：

```json
{
  "query":{
    "function_score":{
      "query":{
        "match":{
          "description":"elasticsearch"
        }
      },
      "functions":[],
      "score_mode":"sum"
      "boost_mode":"replace"
    }
  }
}
```

来看具体的函数定义:

- weight 函数：

  ```json
  {
    "weight":1.5,
    "filter":{
      "term":{
        "description":"hadoop"
      }
    }
  }
  ```

- field_value_factor：

  ```json
  {
    "field_value_factor":{
      "field":"reviews",
      "factor":2.5,
      "modifier":"ln"
    }
  }
  ```
  
- 脚本：

  ```json
  {
    "script_sore":{
      "script":"Math.log(doc['attendess'].values.size()*myweight)",
      "param":{
        "myweight":3
      }
    }
  }
  ```

- 随机

  ```json
  {
    "random_score":{
      "seed":1234
    }
  }
  ```

- 衰减函数

  有三种类型的衰减函数，分别为 linear、gauss、exp，每种衰减函数遵循相同的语法：

  ```json
  {
    "type":{
      "origin":"",
      "offset":"",
      "scale":"",
      "decay":""
    }
  }
  ```

  

### 使用脚本来排序

### 字段数据



## 7使用聚集来探索数据

## 8文档间的关系

文档间的关系包括：对象类型、嵌套文档、父子关系和反规范化

- 对象类型：将一个对象作为文档字段的值

  ```json
  {
    "name":"Denver technology group",
    "events":[
      {
        "date":"2014-12-22",
        "title":"Introduction to Elasticsearch"
      },
      {
        "date":"2014-06-20",
        "title":"Introduction to Hadoop"
      }
    ]
  }
  ```

  缺点：在存储的时候，内部对象的边界未被考虑在内

- 嵌套文档

  将所有数据硬塞到同一篇文档中不见得是明智之举，有时需要为了某个属性的修改来重新索引整篇文档

- 父子关系

- 反规范化

## 在生产环境部署 ES

