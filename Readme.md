# Elasticsearch + Logstash + Kibana Stack
Run the the ELK (Elasticsearch, Logstash, Kibana) stack with Docker and Docker Compose.
All docker images is provided at [here](https://www.docker.elastic.co/).


## Setup
```sh
> cp .env.example .env
> cp /kibana/kibana-example.yml /kibana/kibana.yml

# set value in .env and kibana.yml

docker-compose up
```

**Note important**: If you using Ubuntu/Linux, you may need to change the permissions of folder:

```sh
sudo chmod -R 777 /path_to_project/data
```

## Issue Note
- When memory host contain volumn low. Docker kibana automatically down.

## Create SSL
- Change `xpack.security.enabled` to `false`
- Run `docker-compose exec elasticsearch bash`
```
[root@c9f915e86309 elasticsearch]# bin/elasticsearch-certutil ca

[root@c9f915e86309 elasticsearch]# bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12

```
- Press `Ctrl + D` to exit container
- Run `docker cp "$(docker-compose ps -q elasticsearch)":/usr/share/elasticsearch/elastic-certificates.p12 .` to copy `elastic-stack-ca.p12` to current folder
- Add `./elastic-certificates.p12:/usr/share/elasticsearch/config/elastic-certificates.p12` to `docker-compose.yml`
- `elasticsearch.yml`
```sh
cluster.name: my-elasticsearch-cluster
network.host: 0.0.0.0
xpack.security.enabled: true
xpack.security.transport.ssl.enabled: true
# xpack.security.transport.ssl.keystore.type: PKCS12
# xpack.security.transport.ssl.verification_mode: certificate
# xpack.security.transport.ssl.keystore.path: elastic-certificates.p12
# xpack.security.transport.ssl.truststore.path: elastic-certificates.p12
# xpack.security.transport.ssl.truststore.type: PKCS12
```

## Exploring Your Cluster
Using cURL in the [Console](http://localhost:5601/app/kibana#/dev_tools/console?_g=()) to:

## References:
- [Elasticsearch start docker](https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-docker.html)
- [Elasticsearch Document](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [Kibana Document](https://www.elastic.co/guide/en/kibana/current/index.html)
- [Logstash Document](https://www.elastic.co/guide/en/logstash/current/index.html)
- [Install Elasticsearch with Docker](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)
- [The Search API](https://www.elastic.co/guide/en/elasticsearch/reference/current/_the_search_api.html)
- [Sending Docker Logs to ElasticSearch and Kibana with FileBeat](https://www.sarulabs.com/post/5/2019-08-12/sending-docker-logs-to-elasticsearch-and-kibana-with-filebeat.html)
- [Authenticate kibana with nginx](https://documentation.wazuh.com/3.7/installation-guide/optional-configurations/kibana_ssl.html)
- [Authenticate kibana tutorial](http://codingfundas.com/setting-up-elasticsearch-6-8-with-kibana-and-x-pack-security-enabled/index.html)
