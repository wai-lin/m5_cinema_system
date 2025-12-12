# User Flows

The following are the core business user flows:

1. Customer purchases tickets via web/mobile app
2. Customer purchases tickets via kiosk machine
3. Staff sells tickets via POS machine
4. Admin manages cinema seating
5. Admin reviews analytics reports
6. Seat locking for concurrent holds
7. Ticket management (cancellation, refund, reschedule)

## Sequence Diagrams

### 1. Customer purchases tickets via web/mobile app

```mermaid
sequenceDiagram
    actor Customer
    Customer->>Web/Mobile: browse movies
    Customer->>Web/Mobile: choose showtime (movie + time + hall)
    Customer->>Web/Mobile: select seats
    Customer->>+Web/Mobile: buy ticket
    Web/Mobile->>Customer: request payment credentials
    Customer->>Web/Mobile: payment credentials
    Web/Mobile->>+Server: process payment
    Server->>-Web/Mobile: payment response
    Web/Mobile->>-Customer: show bought tickets
```

### 2. Customer purchases tickets via kiosk machine

```mermaid
sequenceDiagram
    actor Customer
    Customer->>Kiosk: browse movies
    Customer->>Kiosk: choose showtime (movie + time + hall)
    Customer->>Kiosk: select seats
    Customer->>+Kiosk: buy ticket
    Kiosk->>Customer: request payment (cash / online)
    Customer->>Kiosk: provide payment
    Kiosk->>+Server: process payment
    Server->>-Kiosk: payment response
    Kiosk->>-Customer: issue/print tickets
```

### 3. Staff sells tickets via POS machine

```mermaid
sequenceDiagram
    actor Customer
    actor Staff
    Customer->>Staff: choose showtime (movie + time + hall)
    Staff->>Customer: ask to select seats
    Customer->>Staff: select seats
    Customer->>+Staff: confirm to buy ticket
    Staff->>Customer: request payment (cash / online)
    Customer->>Staff: provide payment
    Staff->>+POS: record payment
    POS->>+Server: process payment (if online)
    Server->>-POS: payment response
    POS->>-Staff: record result
    Staff->>-Customer: give out tickets
```

### 6. Seat locking for concurrent holds

```mermaid
sequenceDiagram
    actor User
    User->>App: select seats
    App->>+Server: request to lock seats
    Server->>Server: check if seats are already taken
    Server->>Server: lock seats if not taken
    Server->>OtherAppsAndMachines: broadcast seat availability update
    Server->>-App: return lock result
    User->>App: continue ticket buying process
```

### 7. Ticket management (cancellation, refund, reschedule)

```mermaid
sequenceDiagram
    actor User
    User->>App: request ticket update (cancellation, refund, reschedule)
    App->>+Server: request ticket update
    Server->>Server: validate ticket and seats
    Server->>Server: process refund/payment reversal if needed
    Server->>Server: unlock seats status
    Server->>OtherAppsAndMachines: broadcast seat availability update
    Server->>-App: return ticket status
    App->>User: show updated ticket status
```
