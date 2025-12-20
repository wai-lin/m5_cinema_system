# Cinema Management System

> This is a system redesign of a existing monolithic system to microservice architecture.

## 1 Project Overview

A comprehensive management system for cinemas that enables ticket sales, inventory management, and operational oversight across multiple platforms. Supports clients purchasing tickets through web, mobile, and kiosk interfaces; sellers processing transactions via POS machines; and administrators managing cinema operations and generating business reports.

---

### 1.1 Problem Summary

**Context:** One of the biggest cinema chain in Myanmar with 20+ branches across for each major cities.

- Single Monolithic System
  - The project become hard to maintain as the complexity grows.
- Vertical Scaling is no longer enough
  - It's getting expensive.
  - Cannot scale individual services on demand.
- Single point of failure
  - A single crash in system will cause the whole service to stagnant.
- Database Contention
  - Reporting queries slow down real-time ticket sales.
- Operational Issues
  - Full deployment required for any changes

**Solution:** Migrate to microservices architecture with independent services for ticketing, payment, and reporting.

#### Type of Users

- Customers
- Staffs
- Admins/Managers

#### Customers

Customers can buy movie tickets via:
- Web App
- Mobile App
- Kiosk Machines

#### Staffs

Staffs can sell tickets via:
- POS Machines

#### Admins/Managers

Admins can manage cinemas, view reports via:
- Web Dashboard

---

Next: [1.2 User Flows](./1.2_USER_FLOWS.md)

