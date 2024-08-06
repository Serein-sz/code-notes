```
docker pull elasticsearch:7.4.2
mkdir -p /home/elasticsearch/config
mkdir -p /home/elasticsearch/data
mkdir -p /home/elasticsearch/plugins
echo 'http.host: 0.0.0.0' >> /home/elasticsearch/config/elasticsearch.yml
```

```
docker run --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx128m" -v /home/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v /home/elasticsearch/data:/usr/share/elasticsearch/data/ -v /home/elasticsearch/plugins:/usr/share/elasticsearch/plugins/ -d elasticsearch:7.4.2
```