## Apache Flink and Kafka

![image](https://github.com/user-attachments/assets/6faf7862-1fee-42e4-90db-7adeed599471)

Services (Run in Docker):-
- Kafka
- Zookeeper
- Python data generator service (Python) Postgres (DB)
- Kafka Producer (Python service)
- Flink Consumer (Maven Java service)

This project demonstrates a real-time data streaming pipeline using Apache Kafka, Apache Flink, and PostgreSQL. It simulates weather data generation, processes the data using Flink, and stores the results in a PostgreSQL database. The entire pipeline is orchestrated using Docker Compose.

## Components

### Kafka Producer:

- **Purpose:** To simulate `weather` data and publish it to a Kafka topic.
- **Implementation:** Written in Python, it uses the `wait-for-it.sh` script to ensure Kafka is ready before sending data. The producer sends `weather` data (city and temperature) to the `weather` topic at regular intervals.
### Kafka Broker:

- **Purpose:** Acts as a message broker that manages the Kafka topics and partitions, ensuring reliable message delivery.
- **Configuration:** Configured to use Zookeeper for cluster management. It listens on port `9092`.
### Zookeeper:

- **Purpose:** Manages the Kafka brokers, handles leader election, and maintains metadata about Kafka topics and partitions.
- **Configuration:** Configured to use port `32181`.
### Flink Processor:

- **Purpose:** To consume `weather` data from the Kafka topic, process it, and aggregate the temperature data.
- **Implementation:** Written in Java, it reads from the ``weather`` topic, performs windowed aggregation (averages temperature by city over 60-second windows), and writes the aggregated data to a PostgreSQL database.
- **Aggregation Logic:** Uses `TumblingProcessingTimeWindows` to create 60-second windows for each city and calculates the average temperature within each window.
### PostgreSQL Database:

- **Purpose:** To store the processed and aggregated ``weather`` data.
- **Configuration:** Exposed on port `5438`. The database schema includes a table named `weather` to store city names and their corresponding average temperatures.
### Data Flow
1. **Data Generation:** The Kafka producer generates `weather` data (city, temperature) and sends it to the `weather` Kafka topic.
2. **Data Ingestion:** The Kafka broker receives and manages the messages in the `weather` topic.
3. **Data Processing:** The Flink processor consumes messages from the Kafka topic. It groups the data by city and applies a tumbling window of 60 seconds to calculate the average temperature.
4. **Data Storage:** The processed data (city and average temperature) is written to the PostgreSQL database.
### Docker Setup
The project uses Docker Compose to orchestrate the services, ensuring they are correctly configured and started in the right order. The `docker-compose.yml` file defines the services, their dependencies, and network configurations to allow seamless communication between components.

### Conclusion
This pipeline demonstrates a typical data streaming architecture, using Kafka for real-time message ingestion, Flink for stream processing and aggregation, and PostgreSQL for storing the results. It showcases how to build a robust and scalable data pipeline that can handle real-time data with Docker containers to simplify deployment and management.





