# DeliveryBoy & EndUser - Real-time Location Tracking using Spring Boot & Kafka

This project demonstrates real-time location updates using **Apache Kafka** with two Spring Boot microservices:

- **DeliveryBoy Service** → Produces location updates.
- **EndUser Service** → Consumes location updates and listens for location changes in real time.

---

## **Project Structure**
│── deliveryboy/ # Producer service
│ ├── src/main/java/com/deliveryboy
│ │ ├── config # Kafka Producer Configuration
│ │ ├── controller # REST Controller for location updates
│ │ ├── service # Kafka Producer Service
│ │ ├── constant # App constants
│ │ └── DeliveryboyApplication.java
│
│── enduser/ # Consumer service
│ ├── src/main/java/com/enduser
│ │ ├── config # Kafka Consumer Configuration
│ │ ├── AppConstant # Application constants
│ │ ├── EnduserApplication.java

## **Tech Stack**
- **Java 17**
- **Spring Boot 3+**
- **Spring Kafka**
- **Apache Kafka** (local broker)
- **Maven**

---

## **Kafka Setup**

### **1. Download & Extract Kafka**
```bash
wget https://downloads.apache.org/kafka/3.7.0/kafka_2.13-3.7.0.tgz
tar -xvzf kafka_2.13-3.7.0.tgz
cd kafka_2.13-3.7.0
2. Start Zookeeper
bash


bin/zookeeper-server-start.sh config/zookeeper.properties
3. Start Kafka Broker
bash


bin/kafka-server-start.sh config/server.properties
4. Create a Kafka Topic
bash


bin/kafka-topics.sh --create --topic delivery-location --bootstrap-server localhost:9092
Configuration
DeliveryBoy Service → application.properties
properties


spring.kafka.producer.bootstrap-servers=localhost:9092
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.topic.name=delivery-location
server.port=8081
EndUser Service → application.properties
properties


spring.kafka.consumer.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=enduser-group
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.topic.name=delivery-location
server.port=8082
Usage
1. Run DeliveryBoy Service
bash


cd deliveryboy
mvn spring-boot:run
2. Run EndUser Service
bash


cd enduser
mvn spring-boot:run
3. Send Location Update (Producer → Consumer)
API Endpoint - DeliveryBoy
http


POST http://localhost:8081/api/location/update
Content-Type: application/json

{
    "orderId": "ORD123",
    "latitude": 28.6139,
    "longitude": 77.2090
}
EndUser Output (Consumer Console Log)
log


[KafkaListener] New Location Received:
Order ID: ORD123
Latitude: 28.6139
Longitude: 77.2090
Testing Kafka Manually
Produce Message
bash


bin/kafka-console-producer.sh --topic delivery-location --bootstrap-server localhost:9092
> {"orderId":"ORD123","latitude":28.61,"longitude":77.20}
Consume Message
bash


bin/kafka-console-consumer.sh --topic delivery-location --from-beginning --bootstrap-server localhost:9092
Future Enhancements
Add authentication & authorization using Spring Security

Save location history in MongoDB/MySQL

Add WebSocket for real-time UI updates

Dockerize services

Author
Md Nazir
