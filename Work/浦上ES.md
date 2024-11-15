docker run -d \
  --name elasticsearch \
  -p 9200:9200 \
  -p 9300:9300 \
  -v /home/recharge/es_data/elasticsearch/data:/usr/share/elasticsearch/data \
  -v /home/recharge/es_data/elasticsearch/config:/usr/share/elasticsearch/config \
  -e "discovery.type=single-node" \
  docker.elastic.co/elasticsearch/elasticsearch:7.17.3



docker run -d \
  --name elasticsearch \
  -p 9200:9200 \
  -p 9300:9300 \
  -v /home/recharge/es_data/elasticsearch/data:/usr/share/elasticsearch/data \
  -e "discovery.type=single-node" \
  docker.elastic.co/elasticsearch/elasticsearch:7.17.3



-- 启动命令
docker run -d \
  --name elasticsearch \
  -p 9200:9200 \
  -p 9300:9300 \
  -v /home/recharge/es_data/elasticsearch/data:/usr/share/elasticsearch/data \
  -v /home/recharge/es_data/elasticsearch/config:/usr/share/elasticsearch/config \
  -e "discovery.type=single-node" \
  --memory=2g \
  docker.elastic.co/elasticsearch/elasticsearch:7.17.3



docker run -d \
  --name elasticsearch \
  -p 9200:9200 \
  -p 9300:9300 \
  -v /home/recharge/es_data/elasticsearch/data:/usr/share/elasticsearch/data \
  -v /home/recharge/es_data/elasticsearch/config:/usr/share/elasticsearch/config \
  -e "discovery.type=single-node" \
  -e "xpack.security.enabled=true" \
  -e "ELASTIC_PASSWORD=*?puShang2024" \
  --memory=2g \
  docker.elastic.co/elasticsearch/elasticsearch:7.17.3



curl -u elastic:**PUshang2024 http://localhost:9200

docker exec -it elasticsearch /bin/bash

./bin/elasticsearch-plugin install file:///usr/share/elasticsearch/elasticsearch-analysis-ik-7.17.3.zip


wget https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.17.3/elasticsearch-analysis-ik-7.17.3.zip


   docker cp elasticsearch-analysis-ik-7.17.3.zip elasticsearch:/usr/share/elasticsearch/




docker exec -it kibana /bin/bash

docker cp kibana:/usr/share/kibana/config/kibana.yml /home/recharge/es_data/kibana/config/




docker run -d \
  --name kibana \
  --link elasticsearch:elasticsearch \
  -p 5601:5601 \
  -v \/home/recharge/es_data/kibana/config/:/usr/share/kibana/config \
  docker.elastic.co/kibana/kibana:7.17.3



   curl -u elastic:**PUshang2024 -X GET "localhost:9200/_cat/plugins?v"


   docker logs -f elasticsearch


   curl -X http://localhost:9200