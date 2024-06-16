``` 
```
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



