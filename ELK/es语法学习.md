```
#分词器
POST _analyze
{
    "analyzer":"icu_analyzer",
    "text":"中华人民共和国"
}


POST _analyze
{
  "analyzer": "ik_smart",
  "text": "伊利纯牛奶"
}


POST _analyze
{
  "analyzer": "ik_max_word",
  "text": "伊利纯牛奶"
}


#创建索引
PUT /es_db

#创建索引时可以设置分片数和副本数
PUT /es_db
{
  "settings": {
  "number_of_shards" : 3,
      "number_of_replicas" : 2
    
  }
}

#修改索引配置
PUT /es_db/_settings
{
  "index" : {
  "number_of_replicas" : 1
  }
}

# 查询索引
GET /es_db

# 删除索引
DELETE /es_db


#  创建索引并设置分词器 
PUT /es_db
{
    "settings" : {
        "index" : {
            "analysis.analyzer.default.type": "ik_max_word"
        }
    }
}


PUT /es_db/_doc/1
{
"name": "张三",
"sex": 1,
"age": 25,
"address": "广州天河公园",
"remark": "java developer"
}


PUT /es_db/_doc/2
{
"name": "李四",
"sex": 1,
"age": 28,
"address": "广州荔湾大厦",
"remark": "java assistant"
}

PUT /es_db/_doc/3
{
"name": "王五",
"sex": 0,
"age": 26,
"address": "广州白云山公园",
"remark": "php developer"
}


PUT /es_db/_doc/4
{
"name": "赵六",
"sex": 0,
"age": 22,
"address": "长沙橘子洲",
"remark": "python assistant"
}

PUT /es_db/_doc/5
{
"name": "张龙",
"sex": 0,
"age": 19,
"address": "长沙麓谷企业广场",
"remark": "java architect assistant"
}	
	
PUT /es_db/_doc/6
{
"name": "赵虎",
"sex": 1,
"age": 32,
"address": "长沙麓谷兴工国际产业园",
"remark": "java architect"
}


PUT /es_db/_create/6
{
"name": "赵虎2",
"sex": 1,
"age": 32,
"address": "长沙麓谷兴工国际产业园",
"remark": "java architect"
}


POST /es_db/_update/1
{
  "doc": {
    "age": 28
  }
}


POST /es_db/_update_by_query
{
  "query": { 
    "match": {
      "_id": 1
    }
  },
  "script": {
    "source": "ctx._source.age = 30"
  }
}


GET /es_db/_doc/1


### 文档检索
GET /es_db/_doc/_search?q=age:28


GET /es_db/_doc/_search?q=age[25 TO 28]

GET /es_db/_doc/_search?q=age:>=30




PUT /user/_doc/1
{
  "name":"fox",
  "age":32,
  "address":"长沙麓谷"
}


GET /user/_mapping


DELETE /user


PUT /user
{
  "mappings": {
    "dynamic": "true",
    "properties": {
      "name": {
        "type": "text"
      },
      "address": {
        "type": "object",
        "dynamic": "true"
      }
    }
  }
}


PUT /user/_doc/1
{
  "name":"fox",
  "age":32,
  "address":{
    "province":"湖南",
    "city":"长沙"
  }
}


GET /user/_mapping



PUT /user/_doc/2
{
  "name":"fox",
  "nickName": "fox1",
  "age":32,
  "address":{
    "province":"湖南",
    "city":"长沙"
  }
}



### dsl查询
GET /es_db/_search
{
  "query": {
    "match_all": {}
  },
  "size": 20000
}


GET /es_db/_search
{
  "query": {
    "match": {
      "address": "广州白云山公园"
    }
  }
}


GET /es_db/_search
{
  "query": {
    "match": {
      "address": {
        "query": "广州白云山公园",
        "operator": "or",
        "minimum_should_match": 2
      }
    }
  }
}



GET /es_db/_search
{
  "query": {
    "match_phrase": {
      "address": "广州白云山"
    }
  }
}


GET /es_db/_search
{
  "query": {
    "match_phrase": {
      "address": {
        "query": "广州白云",
        "slop": 2
      }
    }
  }
}

GET /es_db/_mapping


GET /es_db/_search 
{
  "query": {
    "multi_match": {
      "query": "长沙张龙",
      "fields": [
        "address",
        "name"
      ],
      "type": "cross_fields",
      "operator": "and"
    }
  }
}


GET /es_db/_mapping

GET /es_db/_search
{
  "query":{
    "term": {
      "address": {
        "value": "白云山"
      }
    }
  }
}


PUT /product/_bulk
{"index":{"_id":1}}
{"productId":"xxx123","productName":"iPhone"}
{"index":{"_id":2}}
{"productId":"xxx111","productName":"iPad"}

GET /product/_search
{
  "query":{
    "term": {
      "productName": {
        "value": "iPhone"
      }
    }
  }
}


GET /product/_analyze
{
  "analyzer":"standard",
  "text":"iPhone"

```