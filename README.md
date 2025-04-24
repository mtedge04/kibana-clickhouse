This is a distribution that includes quesma, kibana, clickhouse and kafka. I implemented this to test netflow from ElastiFlow flowcoll to Kafka and into Clickhouse but really like Kibana so used quesma to transform queries. Files that might need to be changed updated docker-compose.yml, quesma/examples/kibana-sample-data/connect. You might also need to initialize the kafka connect for clickhouse with something like this - path to clickhouse connect will need to be updated. 

curl -X PUT -H "Content-Type: application/json" \
  -d '{
    "connector.class": "com.clickhouse.kafka.connect.ClickHouseSinkConnector",
    "tasks.max": "1",
    "topics": "elastiflow-flow-codex-",
    "plugin.path": "/home/user/clickhouse-kafka-connect-v1.2.9",
    "ssl": "false",
    "hostname": "kibana-sample-data-clickhouse-1",
    "database": "default",
    "password": "",
    "port": "8123",
    "value.converter.schemas.enable": "false",
    "name": "clickhouse-sink-connector",
    "value.converter": "org.apache.kafka.connect.json.JsonConverter",
    "schemas.enable": "false",
    "key.converter": "org.apache.kafka.connect.storage.StringConverter",
    "connection.keep_alive": "true",
    "exactlyOnce": "false",
    "transforms": "routeToOneTable",
    "transforms.routeToOneTable.type": "org.apache.kafka.connect.transforms.RegexRouter",
    "transforms.routeToOneTable.regex": "elastiflow-flow-codex-.*",
    "transforms.routeToOneTable.replacement": "elastiflow_flow_codex"
  }' \
  http://localhost:8083/connectors/clickhouse-sink-connector/config