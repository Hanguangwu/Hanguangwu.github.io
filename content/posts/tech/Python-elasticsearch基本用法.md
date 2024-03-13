---
title: "Python-elasticsearch基本用法"
date: 2021-08-16T19:34:25+08:00
lastmod: 2021-08-24T22:54:22+08:00
draft: false
categories:
- 技术
- 搜索
tags:
- ES
- Getting Started
---

# Python-elasticsearch基本用法

官方文档：https://www.elastic.co/guide/en/elasticsearch/client/python-api/current/connecting.html#_getting_a_document

## 安装及初始化

使用pip安装即可

```shell
sudo pip3 install elasticsearch
sudo pip3 install elasticsearch[async]	#支持异步，可不安装
```

## **实例化es客户端**

![image-20210219115247238](https://gitee.com/daycen/drawing-bed/raw/master/img/20210505105207.png)

实际上这里文档中给了三种创建客户端的方式，我们选择HTTP authentication方式（看起来清晰一些，当然你也可以用别的）实例化es对象

```python
es = Elasticsearch(
    [
        {"host": "127.0.0.1", "port": 9200},
    ],
    http_auth=("username", "secret"),
    timeout=3600
)
```

ES提供了两种搜索的方式：　　

　 请求参数方式

　 请求体方式（带body 的那种查询，把查询的内容放入body中，会造成一定的开销，但是易于理解）

这里我们使用请求体方式进行搜索

配置一个body

```python
body = {
    "settings": {
        "number_of_shards": 3,
        "number_of_replicas": 2
    },
    "mappings":{
        # "_doc":{
        "properties":{
            "id":{
                "type":"integer",
            },
            "text": {
                "type": "text",
                "analyzer": "ik_max_word",  #指定ik分词器，适用中文分词。
                "index":False
            },
            "userId": {
                "type": "long",
            },
            "reprinted": {
                "type": "keyword",
            },
        }
```

注：可以看到，body中实际上就是之前我们使用请求体参数搜索时设置的一些东西

## 单一操作

### 增加

**create**

必须指定待查询的idnex、type、id和查询体body；缺一不可，否则报错

```python
es.indices.create(index = "testpy", body = body)
```

**index**　

相比于create，index的用法就相对灵活很多；id并非是一个必选项，如果指定，则该文档的id就是指定值，若不指定，则系统会自动生成一个全局唯一的id赋给该文档。

```python
es.index(index = "testpy", doc_type = "_doc", id = 1, body = {"id":1, "name":"小明"})
```

### 删除

　　delete：删除指定index、type、id的文档

```python
es.indices.delete(index = 'test')
```

### 查找

　　get：获取指定index、type、id所对应的文档

```python
es.get(index="testpy", id=2)
```

### 更新

　　update：跟新指定index、type、id所对应的文档

```python
es.update(index='testpy', doc_type='_doc', id='2', body={待更新字段})
```

## 批量操作

### 查询

search：查询满足条件的所有文档，没有id属性，且index，type和body均可为None。 body的语法格式必须符合DSL格式

```python
es.search(index = "test", doc_type = "_doc", body = query)
```

复合查询语句

```python
query = {'query': {'match_all': {}}}# 查找所有文档
query = {'query': {'term': {'name': 'jack'}}}# 查找名字叫做jack的所有文档
query = {'query': {'range': {'age': {'gt': 11}}}}# 查找年龄大于11的所有文档
allDoc = es.search(index='indexName', doc_type='typeName', body=query)
```

### 删除

delete_by_query

```python
query = {'query': {'match': {'sex': 'famale'}}}# 删除性别为女性的所有文档
query = {'query': {'range': {'age': {'lt': 11}}}}# 删除年龄小于11的所有文档
es.delete_by_query(index='indexName', body=query, doc_type='typeName')
```

### 更新

update_by_query

```python
query = {
            "script": {
            "lang": "painless",
            # "inline": "if (ctx._source.test_code == null) {ctx._source.test_code= '02'}"
            "inline": "ctx._source.kw_sourceType= 'trueTime'"   #新增字段kw_sourceType值为trueTime
              }
            }
res = es.update_by_query(index="hot_rank", doc_type="baidu_hot_search_rank", body=query)
```

## 完整测试工程代码

```python
from elasticsearch import Elasticsearch
from datetime import datetime
#from elasticsearch import AsyncElasticsearch

#es = Elasticsearch(host="localhost", port=9200)

es = Elasticsearch(
    [
        {"host": "127.0.0.1", "port": 9200},
    ],
    http_auth=("username", "secret"),
    timeout=3600
)
body = {
    "settings": {
        "number_of_shards": 3,
        "number_of_replicas": 2
    },
    "mappings":{
        # "_doc":{
        "properties":{
            "id":{
                "type":"integer",
            },
            "text": {
                "type": "text",
                "analyzer": "ik_max_word",  #指定ik分词器，适用中文分词。
                "index":False
            },
            "userId": {
                "type": "long",
            },
            "reprinted": {
                "type": "keyword",
            },
        }
        # }
    }
}
#创建 index
#es.indices.create(index = "testpy", body = body)
#删除 index
#es.indices.delete(index = 'test')

#插入数据
#es.index(index = "testpy", doc_type = "_doc", id = 1, body = {"id":1, "name":"小明"})
#可以不用指定id，create会自动添加id。
#es.create(index="testpy", doc_type = "_doc",id = 2, body = {"id":2, "name":"小红"})

'''doc = {
    'author': 'author_name',
    'text': 'Interensting content...',
    'timestamp': datetime.now(),
}'''

res = es.get(index="testpy", id=2)
#es.search(index = "test", doc_type = "_doc", body = query)
print(res['_source'])
```






