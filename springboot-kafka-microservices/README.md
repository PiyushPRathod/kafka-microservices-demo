# Spring Boot Kafka Microservices

This project demonstrates event-driven microservices using Spring Boot and Apache Kafka.

## Modules

- **base-domains**
  Shared DTOs used across services.

- **order-service**
  Produces order events to Kafka.

- **stock-service**
  Consumes order events from Kafka and simulates stock processing.

- **email-service**
  Consumes order events from Kafka and simulates email notification processing.

## Architecture

Client → Order Service → Kafka Topic (`order_topics`) → Stock Service + Email Service

## Tech Stack

- Java 17
- Spring Boot
- Spring Kafka
- Apache Kafka (KRaft mode)
- Maven

## Prerequisites

Make sure the following are installed:

- Java 17+
- Maven
- Git
- Apache Kafka

## Run Kafka in WSL Ubuntu

```bash
mkdir -p ~/kafka-lab
cd ~/kafka-lab
wget https://downloads.apache.org/kafka/4.2.0/kafka_2.13-4.2.0.tgz
tar -xzf kafka_2.13-4.2.0.tgz
cd kafka_2.13-4.2.0
bin/kafka-storage.sh random-uuid
bin/kafka-storage.sh format -t YOUR_UUID_HERE -c config/kraft/server.properties
bin/kafka-server-start.sh config/kraft/server.properties



## Run Spring Boot applications on separate terminals

cd order-service
mvn spring-boot:run

cd stock-service
mvn spring-boot:run

cd email-service
mvn spring-boot:run

## Test the application

curl -X POST http://localhost:8080/api/v1/orders \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Laptop",
    "qty": 2,
    "price": 1200.0
  }'

## Expected Outcomes

- order-service publishes the event
- stock-service consumes the event
- email-service consumes the event
