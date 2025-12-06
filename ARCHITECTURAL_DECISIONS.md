# Architectural Decisions

## Project Elements

**Client Layer:**
- Web App, Mobile App, Kiosk Machines (customers)
- POS Machine (staff)
- Admin Web Portal (administrators)

**Service Layer:**
- Cinema Service, Ticket Service, Payment Service, Reporting Service, Auth Service

**API Layer:**
- Public Gateway (customers & staff)
- Internal Gateway (admins & internal services)

**Data Layer:**
- One database per service (independent scaling & loose coupling)

**External Integrations:**
- 3rd Party Payment Services, 3rd Party Auth Services (OAuth)

---

## Architectural Patterns

- **Microservices**: Each service independently deployed and scaled
- **API Gateway**: Route and authenticate requests (Public & Internal gateways)
- **Database per Service**: Eventual consistency via async messaging
- **Event-Driven Architecture**: Kafka/RabbitMQ for async inter-service communication
- **Service-to-Service Communication**: gRPC + JWT tokens for internal calls

---

## Communication Between Services

- **Client → Services**: REST APIs via Public Gateway
- **Service → Service**: gRPC with JWT token authentication
- **Async Events**: Kafka/RabbitMQ for payment confirmations, ticket generation, reporting updates
- **External Services**: Direct connections for payment processing and OAuth
- **Admin Access**: Internal Gateway routes to Reporting & Admin services

---

## Authentication & Authorization

**AuthN (Authentication):**
- Customers: OAuth (Google, Facebook, Apple, etc...)
- Staff & Admins: OAuth or centralized credential system
- Service-to-Service: JWT tokens issued by Auth Service

**AuthZ (Authorization):**
- **Customers**: View/purchase own tickets
- **Staff**: Sell tickets, process transactions
- **Admins**: Full access (cinema management, reports, user management)

**Implementation:**
- JWT tokens include user role and scopes
- Public Gateway validates customer tokens
- Internal Gateway validates admin/staff tokens
- Services verify JWT claims for fine-grained access control

---

## API Protocol

- **REST**: End-user endpoints (web, mobile, kiosk clients)
- **gRPC**: Inter-service communication (low latency, high throughput)
- **Message Queue**: Async events via Kafka/RabbitMQ

---
