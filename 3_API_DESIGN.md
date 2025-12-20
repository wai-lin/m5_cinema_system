# 3. API Design Package

## 3.1 API Paradigm Decision

| API Type | Use Case | Justification |
|----------|----------|---------------|
| **REST** | Client → Public Gateway | Simple, cacheable, widely supported by web/mobile/kiosk clients |
| **gRPC** | Service → Service | Low latency, strong typing, efficient for internal high-throughput calls |
| **Kafka** | Async events | Decoupled communication for non-blocking operations |

### Why REST for External APIs?

- Universal client support (browsers, mobile SDKs, kiosk systems)
- Easy to cache (GET requests for movies, showtimes)
- Stateless and simple to debug
- Well-understood by frontend teams

### Why not GraphQL?

- Overkill for this domain—our data access patterns are predictable
- REST caching is simpler for read-heavy operations (movie listings)
- Adds complexity without significant benefit for our use case

### Why gRPC for Internal APIs?

- Binary protocol = faster than JSON over REST
- Strong contract via Protobuf schemas
- Built-in support for streaming (useful for real-time seat updates)
- Ideal for service-to-service within Kubernetes cluster

---

## 3.2 REST Endpoints

### Cinema Service

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/cinemas` | List all cinema branches |
| GET | `/cinemas/{id}/movies` | List movies showing at a cinema |
| GET | `/movies/{id}/showtimes` | List showtimes for a movie |
| GET | `/showtimes/{id}/seats` | Get seat availability for a showtime |

### Ticket Service

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/seats/lock` | Lock selected seats (temporary hold) |
| POST | `/tickets` | Purchase ticket (after payment confirmed) |
| GET | `/tickets/{id}` | Get ticket details |
| POST | `/tickets/{id}/cancel` | Cancel a ticket |
| POST | `/tickets/{id}/refund` | Request refund for a ticket |

### Admin Endpoints (Internal Gateway)

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/admin/showtimes` | Create a new showtime |
| GET | `/admin/reports/sales` | Get sales report |

---

## 3.3 Authentication & Authorization

### Authentication Strategy

| User Type | Method | Details |
|-----------|--------|---------|
| Customers | OAuth 2.0 | Google, Facebook, Apple sign-in, etc |
| Staff | Credentials + JWT | Username/password, issued JWT |
| Admins | Credentials + JWT | Username/password with MFA |
| Service-to-Service | JWT | Issued by Auth Service, validated by receiving service |

### Why JWT?

- Stateless: no session storage required
- Contains claims (user ID, role, permissions)
- Gateways can validate without calling Auth Service on every request
- Short-lived tokens + refresh tokens for security

### Authorization (Roles & Permissions)

| Role | Permissions |
|------|-------------|
| Customer | Browse movies, purchase own tickets, view/cancel own bookings |
| Staff | All customer permissions + sell tickets via POS, process refunds |
| Admin | All permissions + manage cinemas, showtimes, users, view reports |

### Implementation

- JWT tokens include `role` and `scopes` claims
- Public Gateway validates customer/staff tokens
- Internal Gateway validates admin tokens
- Each service checks JWT claims for fine-grained access control
