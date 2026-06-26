# Phase 1: Microservices Payment Platform

## Architecture Diagram

```mermaid
graph TD
    Client[Client App] --> API_Gateway[API Gateway]
    
    API_Gateway --> Auth[Auth Service]
    API_Gateway --> User[User Service]
    API_Gateway --> Wallet[Wallet Service]
    API_Gateway --> Payment[Payment Service]
    
    Auth -.-> DB_Auth[(Auth DB)]
    User -.-> DB_User[(User DB)]
    Wallet -.-> DB_Wallet[(Wallet DB)]
    Payment -.-> DB_Payment[(Payment DB)]
    
    Payment -- Initiate Saga --> Kafka[Kafka Message Broker]
    
    Kafka --> Wallet
    Kafka --> Transaction[Transaction Service]
    Kafka --> Notification[Notification Service]
    
    Transaction -.-> DB_Tx[(Transaction DB)]
    
    Eureka[Service Registry] -.-> API_Gateway
    Eureka -.-> Auth
    Eureka -.-> User
    Eureka -.-> Wallet
    Eureka -.-> Payment
    Eureka -.-> Transaction
    Eureka -.-> Notification
```

## Running the Platform
1. Start infrastructure:
   ```bash
   docker-compose up -d
   ```
2. Build all modules:
   ```bash
   mvn clean install -DskipTests
   ```
3. Run the services sequentially:
   - `service-registry`
   - `config-server`
   - `api-gateway`
   - Remaining microservices...
