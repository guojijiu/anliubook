### 碎片知识

1. 实现原理

```
1. 用户调用es提供的接口，将数据整理后请求es保存数据地址；
2. es通过分词控制器将对应的语句分词；
3. 将其权重和分词结果一并存入es数据库；
```

2. 报错缺少分组碎片

```
对应的index执行：

curl -X PUT "http://127.0.0.1:9200/cp_tool_5/_settings" -H "Content-Type: application/json" -d '{
  "settings": {
    "number_of_replicas": 0
  }
}'
```

3. 常用命令：

```
# 分页查询
GET /cp_tool/_search
{
  "from": 0, 
  "size": 1000,  
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "created_at": {
        "order": "desc",
                "unmapped_type": "date",
        "fielddata": true
      }
    }
  ]
}

# 查询单字段，包含任意关键词查询
GET /cp_tool/_search
{
  "size": 1000,
  "query": {
    "match": {
      "name": "tu"
    }
  }
}

# 查询多字段，包含任意关键词查询,并且关键词高亮
GET /cp_tool/_search
{
  "_source": ["id", "name"],
  "from":0,
  "size": 1000,
  "query": {
    "multi_match": {
      "query": "测试 分析",
      "fields": [
        "name",
        "content"
      ],
      "analyzer": "ik_max_word"
    }
  },
  "highlight": {
    "fields": {
      "name": {}
    }
  }
}

# 多关键词并集查询
GET /cp_tool/_search
{
  "size": 1000,
  "query": {
    "query_string": {
      "query": "图 AND 分析",
      "fields": [
        "name"
      ]
    }
  }
}

# 精确查询
GET /cp_tool/_search
{
  "query": {
    "match": {
      "id": "1"
    }
  }
}


# 查询单字段，包含任意关键词查询,并且将查询出来的字段次数进行排序
POST /cp_tool/_search
{
  "query": {
    "function_score": {
      "query": {
        "match": {
          "name": "图"
        }
      },
      "functions": [
        {
          "script_score": {
            "script": {
              "source": "def keywords = doc['fieldName.keyword'].value; return (keywords != null) ? keywords.split(' ').length : 0;"
            }
          }
        }
      ],
      "score_mode": "sum"
    }
  },
  "sort": [
    {
      "_score": {
        "order": "desc"
      }
    }
  ]
}


POST _analyze?pretty
{
  "analyzer": "pinyin",
  "text":     "根据GO或者KEGG富集结果，绘制富集条形图，以可视化的方式展示富集的基因数目以及Qvalue等信息"
}

# 容错匹配
GET /cp_tool/_search
{
  "query": {
    "fuzzy": {
      "name": {
        "value": "cesgi",
        "fuzziness": "AUTO"
      }
    }
  }
}



PUT /cp_tool
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas" : 0,
    "analysis": {
      "analyzer": {
        "custom_analyzer": {
          "tokenizer": "ik_max_word",
          "filter": "custom_pinyin"
        }
      },

      "filter": {
        "custom_pinyin": {
          "type": "pinyin",
          "keep_full_pinyin": false,
          "keep_joined_full_pinyin": true,
          "keep_original": true,
          "limit_first_letter_length": 16,
          "remove_duplicated_term": true,
          "none_chinese_pinyin_tokenize": false
        }
      }
    }
  },

  "mappings": {
    "properties": {
      "name": {
        "type":"text",
        "analyzer": "custom_analyzer",
        "search_analyzer": "ik_smart"
      },
      "description": {
        "type":"text",
        "analyzer": "custom_analyzer",
        "search_analyzer": "ik_smart"
      }
    }
  }
}


GET cp_tool/_analyze?pretty
{
    "analyzer": "ik_max_word",
  "text": "你的测试文本"
}

GET _cat/plugins?v

```
