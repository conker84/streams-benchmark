# How to do a benchmark

Starting from [this document](https://docs.google.com/document/u/2/d/1HpqHSRzelAN1QT0JSJK7ywDOU_ZRgHlr5h_AIxfOgt0/edit#) we're trying to create a reproducible env in order to compare and improve the performances of the Neo4j Streams Plugin.

## Docker

### Prerequisites

Go into [Neo4j](http://localhost:8080) and stop the Sink via the follwing query:

```cypher
CALL streams.sink.stop()
```

Define the constrains:

```cypher
CREATE CONSTRAINT ON (tx:TxStr) ASSERT tx.txId IS UNIQUE;
CREATE CONSTRAINT ON (c:CustomerStr) ASSERT c.custId IS UNIQUE;
```

Check if the topic `benchmark` exists:

```bash
docker run --network streams-benchmark_default \
    --volume $PWD/kafka-data:/data \
    confluentinc/cp-kafkacat \
    kafkacat -b broker:9093 -L | grep benchmark -A 2
```

Create a Kafka topic `benchmark` (if necessary):

```bash
docker exec -it broker kafka-topics --create \
    --zookeeper zookeeper:2181 \
    --replication-factor 1 --partitions 1 \
    --topic benchmark
```

Publish `1M messages` into the `benchmark` topic:

```bash
docker run --network streams-benchmark_default \
    --volume $PWD/kafka-data:/data \
    confluentinc/cp-kafkacat \
    kafkacat -b broker:9093 \
    -t benchmark \
    -P -l /data/data.1M.json
```

Wait until it ends... (it takes a while)

Count the messages into the topic just to be sure:

```bash
docker run --network streams-benchmark_default \
    --volume $PWD/kafka-data:/data \
    confluentinc/cp-kafkacat \
    kafkacat -b broker:9093 \
    -t benchmark \
    -C -e -q | wc -l
```

Now choose what you want to test.

### Test with the Neo4j Streams Plugin Sink

You need to (de)comment the following line inside the compose file in order to manage the auto commit:

`NEO4J_kafka_enable_auto_commit: "false"`

In the beta release we added a new feature that, in case of `kafka.enable.auto.commit=false`, allows committing the offsets in async way, you can simply manage it by (de)commenting the following line:

`NEO4J_kafka_streams_async_commit: "true"`

Start the Sink

```cypher
CALL streams.sink.start()
```

### Test with the Neo4j Kafka Connect Sink

You can choose between two options:

#### Install the Sink with parallelization (default behaviour)

```bash
curl -X POST localhost:8083/connectors \
    -H 'Content-Type:application/json' \
    -H 'Accept:application/json' \
    -d @connect.neo4j.json
```

#### Install the Non Parallelized Sink

```bash
curl -X POST localhost:8083/connectors \
    -H 'Content-Type:application/json' \
    -H 'Accept:application/json' \
    -d @connect.neo4.non-parallelized.json
```

### Test with a simple kafka consumer

Run:
`java -jar simple-neo4j-consumer-1.0.jar localhost:9092 benchmark bolt://localhost:7687 neo4j benchmark`

N.b:
`java -jar simple-neo4j-consumer-1.0.jar <kafka:port> <topic> <neo4j:port> <neo4j_user> <neo4j_pass> [<auto_commit>]`

`auto_commit` values:

* `true` (default)
* `false` disable auto commit and uses the `Consumer#commitSync` as the Neo4j Streams plugin

### How to compute the result

**N.b.** At the end of the test (indipendentely the one you choose) you **must** have:

- 1245480 nodes
- 1000000 relationships

Wait the ingestion ends and then run the following cypher query:

```cypher
MATCH (n:TxStr) WITH count(n) AS countTx
MATCH (n:TxStr) WITH countTx, max(n.finalTimestamp) AS maxTime
MATCH (n:TxStr) WITH countTx, maxTime, min(n.finalTimestamp) AS minTime
RETURN (countTx * 1.000) / ((maxTime - minTime) / 1000)
```
