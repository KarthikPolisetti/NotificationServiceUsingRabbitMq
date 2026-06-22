**Notification Service**
A Spring Boot microservice that accepts order creation requests, persists orders to PostgreSQL, and publishes order events to RabbitMQ. A RabbitMQ listener consumes those events and logs notifications.

Features
REST API for creating orders
Spring Data JPA persistence to PostgreSQL
RabbitMQ messaging for order notifications
WebSocket support included via dependencies
Auto-created database schema using spring.jpa.hibernate.ddl-auto=update
Requirements
Java 17
Maven
RabbitMQ running on localhost:5672
PostgreSQL running on localhost:5432
PostgreSQL database postgres with credentials:
username: postgres
password: pgAdmin4
Configuration
src/main/resources/application.properties contains:

spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
spring.datasource.url=jdbc:postgresql://localhost:5432/postgres
spring.datasource.username=postgres
spring.datasource.password=pgAdmin4
spring.jpa.hibernate.ddl-auto=update
Build and Run
From the project root:

./mvnw clean package
./mvnw spring-boot:run
or run the packaged JAR:

java -jar target/notification-service-0.0.1-SNAPSHOT.jar
API
POST /api/orders

Request body example:

{
"productName": "Example Product"
}

Response:

✅ Order placed successfully and event published!

Behavior:

saves a new Order entity with status = "PENDING"
publishes a notification message to RabbitMQ queue notification.queue
Core Components
NotificationServiceApplication — Spring Boot application entry point
OrderController — REST endpoint for creating orders
Order — JPA entity mapped to orders table
OrderRepository — Spring Data JPA repository
RabbitMQConfig — defines RabbitMQ queue notification.queue
NotificationConsumer — listens to RabbitMQ queue and logs messages
Notes
The code currently uses the package com.example.notification_service
RabbitMQ consumer prints received messages to the console
PostgreSQL driver is configured in pom.xml as runtime dependency
Testing
Unit tests are available under src/test/java
Run with: ./mvnw test
Dependencies
spring-boot-starter-web
spring-boot-starter-amqp
spring-boot-starter-data-jpa
spring-boot-starter-websocket
postgresql runtime driver
spring-boot-starter-test
spring-rabbit-test
