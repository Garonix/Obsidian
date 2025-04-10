**1. docker-compose 配置文件：**

```yaml
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.17.4
    container_name: elasticsearch
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms1g -Xmx1g
      - xpack.security.enabled=false
    ports:
      - "9200:9200"
      - "9300:9300"
    volumes:
      - esdata:/usr/share/elasticsearch/data
    networks:
      - elk
    cap_add:
      - SYS_RESOURCE

  kibana:
    image: docker.elastic.co/kibana/kibana:8.17.4
    container_name: kibana
    environment:
      - ELASTICSEARCH_HOSTS=["http://0.0.0.0:9200"]
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch
    networks:
      - elk

  logstash:
    image: docker.elastic.co/logstash/logstash:8.17.4
    container_name: logstash
    ports:
      - "5044:5044"
      - "9600:9600"
    volumes:
      - ./logstash/pipeline:/usr/share/logstash/pipeline
      - ./logstash/config:/usr/share/logstash/config/
    environment:
      - LS_JAVA_OPTS=-Xms512m -Xmx512m
      - XPACK_MONITORING_ENABLED=true
      - XPACK_MONITORING_ELASTICSEARCH_HOSTS=["http://0.0.0.0:9200"]
    depends_on:
      - elasticsearch
    networks:
      - elk

  filebeat:
    image: docker.elastic.co/beats/filebeat:8.17.4
    container_name: filebeat
    user: root
    volumes:
      - ./filebeat/filebeat.yml:/usr/share/filebeat/filebeat.yml
      - /var/log:/var/log:ro
    depends_on:
      - logstash
    networks:
      - elk
    restart: on-failure

volumes:
  esdata:
    driver: local

networks:
  elk:
    driver: bridge
```

**需要注意的点：** 确保 `volumes` 部分正确映射了主机上的配置文件和数据目录到容器内的相应路径。

**2. filebeat 配置文件 (`filebeat.yml`)：**

```yaml
filebeat.inputs:
  - type: log
    enabled: true
    paths:
      - /var/log/*.log

output.logstash:
  hosts: ["0.0.0.0:5044"]

processors:
  - add_host_metadata:
      when.not.contains.tags: forwarded
  - add_fields:
      target: beat
      fields:
        hostname: filebeat
```

**需要注意的点：** 仔细配置 `filebeat.inputs.paths` 以指向你实际需要收集的日志文件路径，并设置正确的 `output.logstash.hosts`。

**3. logstash 配置文件 (`.conf` 文件，例如 `logstash.conf`)：**

```
input {
  beats {
    port => 5044
  }
}

filter {
  grok {
    match => { "message" => "%{COMBINEDAPACHELOG}" }
  }
}

output {
  elasticsearch {
    hosts => ["0.0.0.0:9200"]
    index => "filebeat-%{+YYYY.MM.dd}"
  }
}
```

**需要注意的点：** 在 `filter` 部分，特别是 `grok` 过滤器的 `match` 部分，你需要根据你的实际日志格式编写正确的模式。