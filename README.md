## Apache Flink and Kafka

![image](https://github.com/user-attachments/assets/6faf7862-1fee-42e4-90db-7adeed599471)

Services (Run in Docker):-
- Kafka
- Zookeeper
- Python data generator service (Python) Postgres (DB)
- Kafka Producer (Python service)
- Flink Consumer (Maven Java service)

This project demonstrates a real-time data streaming pipeline using Apache Kafka, Apache Flink, and PostgreSQL. It simulates weather data generation, processes the data using Flink, and stores the results in a PostgreSQL database. The entire pipeline is orchestrated using Docker Compose.

## Table of Contents

- [Architecture](#architecture)
- [Components](#components)
- [Data Flow](#data-flow)
- [Configuration](#configuration)

## Architecture

The data streaming pipeline consists of the following components:

1. **Kafka Producer**: Generates and publishes weather data to a Kafka topic.
2. **Kafka Broker**: Manages Kafka topics and partitions.
3. **Zookeeper**: Handles Kafka broker management and metadata.
4. **Flink Processor**: Consumes, processes, and aggregates weather data.
5. **PostgreSQL Database**: Stores the aggregated weather data.

## Components

### Kafka Producer

- **Language**: Python
- **Description**: Simulates weather data and publishes it to the `weather` Kafka topic.

### Kafka Broker

- **Image**: confluentinc/cp-kafka
- **Description**: Acts as a message broker for managing Kafka topics and partitions.

### Zookeeper

- **Image**: confluentinc/cp-zookeeper:latest
- **Description**: Manages Kafka brokers and maintains metadata.

### Flink Processor

- **Language**: Java
- **Description**: Consumes weather data from Kafka, processes it, and writes the results to PostgreSQL.

### PostgreSQL Database

- **Image**: PostgreSQL
- **Description**: Stores the processed and aggregated weather data.

### Data Flow
1. Data Generation: The Kafka producer generates weather data (city, temperature) and sends it to the weather Kafka topic.
2. Data Ingestion: The Kafka broker receives and manages the messages in the weather topic.
3. Data Processing: The Flink processor consumes messages from the Kafka topic, groups the data by city, and applies a tumbling window of 60 seconds to calculate the average temperature.
4. Data Storage: The processed data (city and average temperature) is written to the PostgreSQL database.

### Configuration
#### Docker Compose
The `docker-compose.yml` file defines the services and their configurations. Key configurations include:

Kafka Broker:
  - Port: `9092`
  - Environment Variables: `KAFKA_BROKER_ID`,`KAFKA_ADVERTISED_LISTENERS`, etc.
Zookeeper:
- Port: `32181`
PostgreSQL:
  - Port: `5438`
  - Environment Variables: `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`
  
#### Flink Processor
The Flink processor's main logic is in the Java code, which consumes data from Kafka, processes it, and writes to PostgreSQL. Key configurations include Kafka topic, windowing strategy, and JDBC connection options.



