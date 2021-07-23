# Kafka

Can be treated a bit like a database with KSQL

## Topics
 - key/value pairs
 - append-only
 - can be partitioned

## Kafka streams
 - Streamed applications are combining producers and consumers
 - Kafka connect combines these, and makes use of mappings defined on topics to map from e.g. JSON sent by Kafka into Cassandra tables

## Kafka and Cassandra Microservice Architecture

![image](https://user-images.githubusercontent.com/916366/126725975-f11b518c-c4bc-4a1f-b5b1-8e53541f318a.png)

Connect can be used to dispatch data into Cassandra
![image](https://user-images.githubusercontent.com/916366/126725963-97145905-8515-479c-9c71-9cf5df48757d.png)

An emerging pattern allows data to be pulled from Cassandra into Kafka
![image](https://user-images.githubusercontent.com/916366/126726339-dee0244b-5360-4924-b681-ec3a2856cfef.png)
