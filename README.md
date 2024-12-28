# elastic-kibana

Let's run a Elasticsearch as a single node cluster using Docker Compose with XPack disabled. 
To run the Elasticsearch Docker image as a single node, you have to set discovery.type to single-node. At startup, the bootstrap checks are bypassed. The single node will elect itself as the master node and will not join a cluster with any other node.

A complete docker-compose.yml example to run a single node Elasticsearch Cluster including Kibana:

```
version: '3.7'

services:
  elasticsearch:
    image: elasticsearch:8.17.0
    environment:
      - xpack.security.enabled=false  # Disable to use Elasticsearch’s authentication, authorization and audit features
      - discovery.type=single-node  # Switch default mode from multi-node to single-node
    ulimits:
      memlock:
        soft: -1
        hard: -1
      nofile:
        soft: 65536
        hard: 65536
    volumes:
      - elasticsearch-data:/usr/share/elasticsearch/data
    ports:
      - 9200:9200

  kibana:
    image: kibana:8.17.0
    ports:
      - 5601:5601
    depends_on:
      - elasticsearch

volumes:
  elasticsearch-data:
    driver: local
```
