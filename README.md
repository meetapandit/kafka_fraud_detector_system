# Event Streaming using Kafka - Fraud detector System

The goal of this project was to build a simple fraud detector app that produces fake transactions using kafka-python client and then consumes the received events to classify the transactions as FRAUD vs LEGIT

## High-level Architecture

<p align="center">
  <img src="https://github.com/meetapandit/kafka_fraud_detector_system/assets/15186489/1c2b7bcc-88f7-4d4a-9a41-6c5b41156c14"> 
</p>

## Detailed Walkthrough
- There are 2 steps to the fraud detection system architecture
- Generator
  - The Generator or Kafka Producer application that generates fake transactions with source_id, target_id, amount, and currency
  - This is a Python application that is implemented using the kafka-python client
  - The generated events are sent to Kafka topic for further transformation
- Detector
    - The detector is a Python client that consumes the generated events from the Kafka topic
    - It then categorizes the transaction as legit or fake based on a condition specific to this use case (CONDITION - all transactions > $900 are fraud and < $900 as legit)
    - The consumer transforms the events and creates 2 more topics
      - Fraud_transactions topic to filter transactions that are fraud based on the condition
      - Legit_transactions topic to filter transactions that are legitimate based on the condition

## Workflow
- Kafka cluster with 1 broker and replication factor of 1 hosted on docker on an isolated network separate from all other apps
  - The config for the Kafka cluster is defined in docker-compose.kafka.yml with zookeeper and broker config settings along with the network name for running Kafka cluster in an isolated container
- Producer and Consumer are Python apps implemented with the kafka-python client
- Both generator and detector apps have requirements.txt files to handle dependencies and docker-compose.yml with environment variables declared for running generator and detector programs

## Screenshots for test cases to check producer, consumer, and new topics created by consumer for classifying transactions

- Validate if the producer sends events to the transactions topic in Kafka
<p align="center"> 
  <img src="https://github.com/meetapandit/kafka_fraud_detector_system/assets/15186489/85040c27-9f2c-489f-9022-5a2d26af648e"> 
</p>

- Testcase for validating legit transactions
<p align="center"> 
  <img src="https://github.com/meetapandit/kafka_fraud_detector_system/assets/15186489/7120b9e8-08d3-445c-9a6b-9c4bec7a522b">
</p>

- Console output of the actual number of transactions read by the consumer, processed and sent to a new topic streaming.transactions.legit
<p align="center"> 
  <img src="https://github.com/meetapandit/kafka_fraud_detector_system/assets/15186489/aa259bf4-7aad-497e-aee9-fb0603e07b7b">
</p>

- Testcase for validating fraud transactions
<p align="center"> 
  <img src="https://github.com/meetapandit/kafka_fraud_detector_system/assets/15186489/2dcfcfa8-ae3f-49b4-9710-b8bdb78ea01e">
</p>

- Console output of the actual number of transactions read by the consumer, processed and sent to a new topic streaming.transactions.fraud
<p align="center"> 
  <img src="https://github.com/meetapandit/kafka_fraud_detector_system/assets/15186489/ca0fea4f-1fb2-481c-bf4e-768fdc33cedd">
</p>

