# Skywalking-Dcoker Quick Start
其中引用了[wutang/elasticsearch-shanghai-zone ](https://hub.docker.com/r/wutang/elasticsearch-shanghai-zone/)镜像和[wutang/skywalking-docker](https://hub.docker.com/r/wutang/skywalking-docker/) 镜像

- ```cd /skywalking-docker/5.x/quick-start/```
- ```docker-compose up docker-compose-v2.yml```

```

version: '2'
services:
  elasticsearch-service:
    image: wutang/elasticsearch-shanghai-zone:5.6.10
    container_name: elasticsearch
    environment:
      - cluster.name=elasticsearch
      - bootstrap.memory_lock=true
      - xpack.security.enabled=false
      - "ES_JAVA_OPTS=-Xms2g -Xmx2g"
      - node.name=elasticsearch_node_1
    ulimits:
      memlock:
        soft: -1
        hard: -1
    ports:
      - 9200:9200
      - 9300:9300
  
  skywalking:
    image: wutang/skywalking-docker:5.x
    container_name: skywalking
    environment:
      - ES_CLUSTER_NAME=elasticsearch
      - ES_ADDRESSES=elasticsearch-service:9300
      - BIND_HOST=skywalking
      - AGENT_JETTY_BIND_HOST=skywalking
      - NAMING_BIND_HOST=skywalking
      - UI_JETTY_BIND_HOST=0.0.0.0
    depends_on:
      - elasticsearch-service
    links:
      - elasticsearch-service
    ports:
      - 8080:8080
      - 10800:10800
      - 11800:11800
      - 12800:12800

```
