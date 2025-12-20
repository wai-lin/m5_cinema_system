# 5. Implementation Team Proposals

## Team Structure

| Role | Responsibility |
|------|----------------|
| Staff | Collect cinema data (hall info, seating plans) |
| Admins/Managers | Define report requirements, provide business logic |
| Developers | Build and maintain the system |
| DevOps | Infrastructure, CI/CD, monitoring, deployments |

---

## Development Teams

**2 teams, 3-4 developers each (6-8 total)**

**1-2 DevOps engineers (shared across teams)**

### DevOps Responsibilities

- Set up and maintain Kubernetes clusters
- CI/CD pipelines for all services
- Monitoring and alerting (Prometheus, Grafana)
- Centralized logging (ELK stack)
- Infrastructure as Code (Terraform)
- Database backups and disaster recovery

---

## Phase I: Internal Users

**Goal:** Ship to direct system users (Staff + Admins)

**Timeline:** 6-8 months

**Channels:** POS Machines, Admin Web Portal

| Team | Responsibilities |
|------|------------------|
| Team 1 | Auth Service, Cinema Service, Admin Portal, Reporting Service |
| Team 2 | Ticket Service, Payment Service, POS Machines |

**Key Deliverables:**
- Seat locking implementation
- Payment/reversal handling
- Reporting tuned for operations
- Monitoring/alerts for gateways and services

---

## Phase II: Customer-Facing Channels

**Goal:** Open customer-facing online channels

**Timeline:** 4-6 months

**Channels:** Mobile Apps, Web Application, Kiosk Machines

| Team | Responsibilities |
|------|------------------|
| Team 1 | Mobile Apps (iOS, Android) |
| Team 2 | Kiosk Machines, Web Application |

**Key Deliverables:**
- Scale Public Gateway for higher traffic
- Promo/discount service
- Ticket delivery (QR/email/SMS)
- Improved concurrency controls

**Rollout Order:** Kiosk → Web → Mobile (after payment and seat-lock are stable)

---

## Future Phases

Depending on business priority:
- Snack Store Management Service
- Discount and Promotion Service
- 3rd-party integrations (chat/wallet apps)
- Loyalty/CRM and advanced analytics
