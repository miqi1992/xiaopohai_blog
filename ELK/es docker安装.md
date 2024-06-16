#es安装 #docker
```java
docker run -d \
  --restart=always \
  --name elasticsearch \
  --network es-net \
  -p 9200:9200 \
  -p 9300:9300 \
  --privileged \
  -e "discovery.type=single-node" \
  -v /home/recharge/es_data/elasticsearch/data:/usr/share/elasticsearch/data \
  -v /home/recharge/es_data/elasticsearch/logs:/usr/share/elasticsearch/logs \
  -v /home/recharge/es_data/elasticsearch/plugins:/usr/local/elasticsearch7.12.1/plugins \
  -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" \
  elasticsearch:7.17.3
```



``` java
docker run -d \
--name kibana \
-e ELASTICSEARCH_HOSTS=http://elasticsearch:9200 \
--network=es-net \
-p 5601:5601 \
kibana:7.17.3
```



### 常见问题
1. 权限问题
sudo chown -R 1000:1000 /home/recharge/es_data/elasticsearch/config
sudo chown -R 1000:1000 /home/recharge/es_data/elasticsearch/data
sudo chown -R 1000:1000 /home/recharge/es_data/elasticsearch/logs

