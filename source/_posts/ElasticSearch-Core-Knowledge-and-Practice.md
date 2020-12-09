---
title: ElasticSearch Core Knowledge and Practice
date: 2020-10-30 18:00:17
tags:
---

## Logstash安装与导入

安装Logstash，并且导入Movielens的测试数据集
- Small: 100,000 ratings and 3,600 tag applications applied to 9,000 movies by 600 users. Last updated 9/2018.
- movielens/ml-latest-small/movies.csv movie数据
- movielens/logstash.conf //logstash 7.x 配置文件，
- movielens/logstash6.conf  //logstash 6.x 配置文件

```
#下载与ES相同版本号的logstash，（7.1.0），并解压到相应目录
#修改movielens目录下的logstash.conf文件
#path修改为,你实际的movies.csv路径
input {
  file {
    path => "YOUR_FULL_PATH_OF_movies.csv"
    start_position => "beginning"
    sincedb_path => "/dev/null"
  }
}

on windows ==> use `NUL` in sincedb_path to supress the use of a sincedb

put logstash.conf in a config file for better management

#启动Elasticsearch实例，然后启动 logstash，并制定配置文件导入数据
bin/logstash -f /YOUR_PATH_of_logstash.conf

```

* run `bin/elasticsearch.bat` first and then
* run `bin/kibana.bat`
* run `bin/logstash -f path_to/logstash.conf` or just `bin/logstash` if you already run with the conf file
* run `filebeat.exe -c filebeat.yml` ==> for filebeat 


## 基本概念：节点，集群，分片以及副本

通过kibana导入sample data电商数据并且使用command去了解数据信息

Index相关API
```
# 查看index相关信息
GET kibana_sample_data_ecommerce

# 查看index文档总数
GET kibana_sample_data_ecommerce/_count

# 查看前10条文档，了解文档格式
POST kibana_sample_data_ecommerce/_search 
{}

# _cat indices API
# 查看indices
GET /_cat/indices/kibana*?v&s=index

#查看状态为绿的索引
GET /_cat/indices?v&health=green

#按照文档个数排序
GET /_cat/indices?v&s=docs.count:desc

#查看具体的字段
GET /_cat/indices/kibana*?pri&v&h=health,index,pri,rep,docs.count,mt

#How much memory is used per index?
GET /_cat/indices?v&h=i,tm&s=tm:desc

```

分布式系统的可用性与扩展性

* 高可用性
    * 服务可用性 - 允许有节点停止服务
    * 数据可用性 - 部分节点丢失，不会丢失数据

* 可扩展性
    * 请求量升级 / 数据的不断增长（将数据分布到所有的节点上）
    
### 分片 （primary shard & replica shard）

* 一个三节点的集群中，blogs索引的分片分布情况
    * 思考：增加一个节点或者改变大主分片数对系统的影响
    

## 倒排索引介绍

书的目录，索引页


