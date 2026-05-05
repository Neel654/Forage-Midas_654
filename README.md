# 💳 Midas Core - Transaction Processing & Incentive Management System

A Spring Boot microservice built for the **JPMC Advanced Software Engineering Forage Program** that processes financial transactions in real-time, manages user balances, and integrates with incentive programs.

## 🎯 Project Overview

Midas Core is a backend system designed to handle transaction processing workflows in a financial environment. It leverages event-driven architecture with Apache Kafka to process transactions asynchronously, persist data efficiently, and provide real-time balance queries through REST APIs.

### Key Capabilities
- ✅ **Real-time Transaction Processing** - Handles transactions via Kafka event streaming
- ✅ **Account Balance Management** - Calculates and tracks user account balances
- ✅ **Incentive Program Integration** - Connects to external incentive APIs to reward transactions
- ✅ **Data Persistence** - Stores all transaction and user records in H2 database
- ✅ **REST API** - Query balances and transaction data through clean REST endpoints

## 🏗️ Architecture

```
┌─────────────────┐
│  Kafka Topic    │
│ ("transaction") │
└────────┬────────┘
         │
         ▼
┌─────────────────────────┐
│ TransactionReceiver     │
│ (Kafka Consumer)        │
└────────┬────────────────┘
         │
         ▼
┌──────────────────────────────┐
│ TransactionHandler           │
│ (Business Logic)             │
└────────┬─────────────────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌─────────┐ ┌──────────────────┐
│ Database│ │ IncentiveQuerier │
│ Conduit │ │ (External API)   │
└─────────┘ └──────────────────┘
    │
    ▼
┌──────────────────┐
│ H2 Database      │
│ (Persistence)    │
└──────────────────┘

REST API: BalanceRestController → Query Balances
```

## 🛠️ Technology Stack

| Layer | Technology |
|-------|-----------|
| **Runtime** | Java 17 |
| **Framework** | Spring Boot 3.2.5 |
| **Event Streaming** | Apache Kafka 3.1.4 |
| **Data Access** | Spring Data JPA |
| **Database** | H2 (Embedded) |
| **Build Tool** | Maven |
| **Testing** | JUnit + Spring Test + Testcontainers |

## 📁 Project Structure

```
src/main/java/com/jpmc/midascore/
├── MidasCoreApplication.java          # Spring Boot entry point
├── component/                          # Business logic components
│   ├── BalanceRestController.java      # REST API for balance queries
│   ├── TransactionHandler.java         # Core transaction processing logic
│   ├── TransactionReceiver.java        # Kafka consumer
│   ├── DatabaseConduit.java            # Database operations
│   └── IncentiveQuerier.java          # External incentive API client
├── entity/                             # JPA database entities
│   ├── TransactionRecord.java
│   └── UserRecord.java
├── foundation/                         # Domain models
│   ├── Transaction.java
│   ├── Balance.java
│   └── Incentive.java
└── repository/                         # Spring Data repositories
```

## 🚀 Getting Started

### Prerequisites
- Java 17+
- Maven 3.6+
- Apache Kafka running locally (or Docker)

### Installation & Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Neel654/Forage-Midas_654.git
   cd Forage-Midas_654
   ```

2. **Start Kafka** (using Docker)
   ```bash
   docker-compose up -d kafka
   ```

3. **Build the project**
   ```bash
   ./mvnw clean build
   ```

4. **Run the application**
   ```bash
   ./mvnw spring-boot:run
   ```

   The service will start on: `http://localhost:33400`

### Configuration

Edit `application.yml` to customize settings:
```yaml
spring:
  kafka:
    consumer:
      group-id: midas              # Kafka consumer group
      auto-offset-reset: earliest
    producer:
      value-serializer: JsonSerializer
      
general:
  kafka-topic: "transaction"       # Topic name for transactions
  incentive-api-url: "http://localhost:8080/incentive"  # External incentive API

server:
  port: 33400                       # Application port
```

## 📊 API Endpoints

### Get User Balance
```
GET /balance/{userId}
Response: 
{
  "userId": "user123",
  "balance": 5000.00,
  "lastUpdated": "2025-11-18T10:30:00Z"
}
```

## 🔄 How It Works

### Transaction Flow
1. **Ingest**: Transactions are sent to Kafka topic `transaction`
2. **Consume**: `TransactionReceiver` listens on Kafka and receives transaction events
3. **Process**: `TransactionHandler` validates and processes each transaction
4. **Persist**: `DatabaseConduit` saves transaction and updates user balance in H2 database
5. **Incentivize**: `IncentiveQuerier` calls external API to check for applicable incentives
6. **Query**: Users can retrieve their balance via REST API

### Example Transaction Payload
```json
{
  "transactionId": "txn_12345",
  "userId": "user_001",
  "amount": 100.50,
  "timestamp": "2025-11-18T10:15:00Z",
  "category": "purchase"
}
```

## ✅ Testing

Run the test suite:
```bash
./mvnw test
```

The project includes:
- Unit tests for business logic
- Integration tests with Testcontainers for Kafka
- Spring context tests

## 📝 Development Notes

- **Event-Driven**: Uses Kafka for async, scalable transaction processing
- **Microservice Ready**: Clean separation of concerns, easy to extend
- **Database Agnostic**: Switch from H2 to PostgreSQL/MySQL by updating `pom.xml`
- **Stateless**: Can horizontally scale multiple instances

## 🎓 Learning Outcomes (JPMC Forage)

This project demonstrates:
- Spring Boot microservice development
- Event-driven architecture with Kafka
- RESTful API design
- Database persistence with JPA
- Integration with external APIs
- Testing best practices

## 📄 License

This project is part of the JPMC Advanced Software Engineering Forage Program.

## 👤 Author

**Neel654** - JPMC Forage Program Participant

---

**Questions or Feedback?** Feel free to open an issue or submit a PR! 🚀