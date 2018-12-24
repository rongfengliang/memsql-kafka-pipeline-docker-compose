# memsql with kafka pipeline demo

## how to run

* mkdir .env file && get license from memsql website && copy license content to .env

```code
LICENSE_KEY=${youlicensecontent}
```

* start deps

```code
docker-compose up -d
```

* create kafka topic

note: must inside kafka container

```code
/opt/kafka/bin/kafka-topics.sh --topic test --zookeeper zk:2181 --create --partitions 8 --replication-factor 1
```

* create insert memsql database && table && pipeline

note: you can use mysql client do below ops

```code
database:
CREATE DATABASE quickstart_kafka;
USE quickstart_kafka;
table:
CREATE TABLE messages (id text);

pipeline:
CREATE PIPELINE `quickstart_kafka` AS LOAD DATA KAFKA 'kafka/test' INTO TABLE `messages`;
```

* start pipeline

```code
START PIPELINE quickstart_kafka;
```

* send message

note: must inside kafka container

```code
/opt/kafka/bin/kafka-console-producer.sh --topic test --broker-list 127.0.0.1:9092
```

* select insert message

```code
SELECT * FROM quickstart_kafka.messages;
```