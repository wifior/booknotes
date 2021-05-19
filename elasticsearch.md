### ElasticSearch

```she
sudo docker pull elasticsearch:7.12.0
sudo docker pull kibana:7.12.0
mkdir -p elasticsearch/config
mkdir -p elasticsearch/data
chmod -R 777 elasticsearch/
echo "http.host:0.0.0.0" >> elasticsearch/config/elasticsearch.yml
docker run --name elasticsearch -p 9200:9200 -p 9300:9300 \
-e "discovery.type=single-node" \
-e ES_JAVA_OPTS="-Xms64m-Xmx128m" \
-v elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v elasticsearch/data:/usr/share/elasticsearch/data \
-v elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-d elasticsearch:7.12.0
#运行kibana
docker run --name kibana -e ELASTICSEARCH_HOSTS=https://192.168.5.5P:9200 -p 5601:5601 \
-d kibana:7.12.0
```

检索命令

```tex
_cat
GET /_cat/nodes   ：查看所有节点
GET /_cat/health   ：查看es健康状况
GET /_cat/master   ：查看主节点
GET /_cat/indices   ：查看所有索引
show databases;
```

索引一个文档

```tex
保存一个数据，保存在哪个索引的哪个类型下，指定用哪个唯一标识

在customer索引下的external类型下保存1号数据
PUT customer/external/1
用PUT必须加id,POST不要求
```

查询文档

```tex
GET customer/external/1
更新携带?if_seq_no=0&if_primary_term=1
```

更新文档

```tex
POST customer/external/1/_update

POST 带_update会对比原来数据，与原来一样，什么也不做
PUT、POST会修改版本号
```

删除文档

```tex
DELETE costomer/external/1
```

批量api

```tex
POST customer/external/_bulk
```



