# Class Diagrams

---
title: "Class Diagrams"
status: published
owner: "PIMPyourDocs"
created: 2024-01-15
updated: 2024-01-15
tags: [diagrams, mermaid, class, uml, oop]
---

## Overview

Class diagrams document object-oriented designs, showing classes, their attributes, methods, and relationships.

**Best for:**

- API client SDK documentation
- Domain model design
- Interface contracts
- Design pattern illustration
- Type hierarchy documentation

---

## Syntax Reference

### Class Definition

```mermaid
classDiagram
    class User {
        +String id
        +String email
        -String passwordHash
        #DateTime createdAt
        +validate() bool
        +save() void
        -hashPassword(plain) String
    }
```

### Visibility Modifiers

| Symbol | Visibility |
|--------|------------|
| `+` | Public |
| `-` | Private |
| `#` | Protected |
| `~` | Package/Internal |

### Generics

```mermaid
classDiagram
    class Repository~T~ {
        +getById(id) T
        +getAll() List~T~
        +save(entity: T) void
        +delete(id) void
    }
    
    class UserRepository {
        +findByEmail(email) User
    }
    
    Repository~T~ <|-- UserRepository
```

### Relationships

```mermaid
classDiagram
    classA <|-- classB : Inheritance
    classC *-- classD : Composition
    classE o-- classF : Aggregation
    classG --> classH : Association
    classI -- classJ : Link
    classK ..> classL : Dependency
    classM ..|> classN : Realization
```

| Symbol | Meaning | Read as |
|--------|---------|---------|
| `<\|--` | Inheritance | "extends" |
| `*--` | Composition | "owns" (lifecycle bound) |
| `o--` | Aggregation | "has" (independent lifecycle) |
| `-->` | Association | "uses" |
| `..>` | Dependency | "depends on" |
| `..\|>` | Realization | "implements" |

### Cardinality

```mermaid
classDiagram
    Customer "1" --> "*" Order : places
    Order "1" *-- "1..*" OrderLine : contains
```

### Stereotypes and Annotations

```mermaid
classDiagram
    class IRepository~T~ {
        <<interface>>
        +getById(id) T
        +save(entity: T) void
    }
    
    class AbstractEntity {
        <<abstract>>
        +UUID id
        +DateTime createdAt
        #onCreate()* void
    }
    
    class UserService {
        <<service>>
    }
```

---

## Example: Repository Pattern

```mermaid
classDiagram
    class IRepository~T~ {
        <<interface>>
        +GetById(id: Guid) T
        +GetAll() IEnumerable~T~
        +Add(entity: T) void
        +Update(entity: T) void
        +Delete(id: Guid) void
    }
    
    class IUnitOfWork {
        <<interface>>
        +Users IRepository~User~
        +Orders IRepository~Order~
        +BeginTransaction() IDbTransaction
        +Commit() void
        +Rollback() void
    }
    
    class Entity {
        <<abstract>>
        +Guid Id
        +DateTime CreatedAt
        +DateTime? UpdatedAt
        #OnCreate() void
        #OnUpdate() void
    }
    
    class User {
        +String Email
        +String PasswordHash
        +UserRole Role
        +Bool IsActive
        +ValidatePassword(password: String) Bool
        +ChangePassword(newHash: String) void
    }
    
    class Order {
        +Guid UserId
        +OrderStatus Status
        +Decimal Total
        +List~OrderLine~ Lines
        +AddLine(product: Product, qty: Int) void
        +Submit() void
        +Cancel() void
    }
    
    class OrderLine {
        +Guid ProductId
        +Int Quantity
        +Decimal UnitPrice
        +Decimal LineTotal
    }
    
    class PostgresRepository~T~ {
        -NpgsqlConnection _connection
        -String _tableName
        +GetById(id: Guid) T
        +GetAll() IEnumerable~T~
        +Add(entity: T) void
        +Update(entity: T) void
        +Delete(id: Guid) void
    }
    
    class PostgresUnitOfWork {
        -NpgsqlConnection _connection
        -NpgsqlTransaction _transaction
        +Users IRepository~User~
        +Orders IRepository~Order~
        +BeginTransaction() IDbTransaction
        +Commit() void
        +Rollback() void
    }
    
    Entity <|-- User : extends
    Entity <|-- Order : extends
    Order *-- OrderLine : contains
    IRepository~T~ <|.. PostgresRepository~T~ : implements
    IUnitOfWork <|.. PostgresUnitOfWork : implements
    PostgresUnitOfWork --> PostgresRepository~User~ : uses
    PostgresUnitOfWork --> PostgresRepository~Order~ : uses
```

---

## Example: Strategy Pattern

```mermaid
classDiagram
    class PaymentProcessor {
        -IPaymentStrategy strategy
        +setStrategy(strategy: IPaymentStrategy) void
        +processPayment(amount: Decimal) PaymentResult
    }
    
    class IPaymentStrategy {
        <<interface>>
        +pay(amount: Decimal) PaymentResult
        +refund(transactionId: String) RefundResult
        +supports(currency: String) Bool
    }
    
    class StripeStrategy {
        -String apiKey
        -StripeClient client
        +pay(amount: Decimal) PaymentResult
        +refund(transactionId: String) RefundResult
        +supports(currency: String) Bool
    }
    
    class PayPalStrategy {
        -String clientId
        -String clientSecret
        -PayPalClient client
        +pay(amount: Decimal) PaymentResult
        +refund(transactionId: String) RefundResult
        +supports(currency: String) Bool
    }
    
    class CryptoStrategy {
        -String walletAddress
        -BlockchainClient client
        +pay(amount: Decimal) PaymentResult
        +refund(transactionId: String) RefundResult
        +supports(currency: String) Bool
    }
    
    PaymentProcessor --> IPaymentStrategy : uses
    IPaymentStrategy <|.. StripeStrategy : implements
    IPaymentStrategy <|.. PayPalStrategy : implements
    IPaymentStrategy <|.. CryptoStrategy : implements
```

---

## Example: Event-Driven Architecture

```mermaid
classDiagram
    class IEvent {
        <<interface>>
        +String eventId
        +DateTime timestamp
        +String aggregateId
    }
    
    class IEventHandler~T~ {
        <<interface>>
        +handle(event: T) void
    }
    
    class EventBus {
        -Map~Type, List~IEventHandler~~ handlers
        +subscribe(eventType: Type, handler: IEventHandler) void
        +publish(event: IEvent) void
        -dispatch(event: IEvent) void
    }
    
    class OrderCreatedEvent {
        +String orderId
        +String customerId
        +Decimal total
        +List~OrderItem~ items
    }
    
    class OrderShippedEvent {
        +String orderId
        +String trackingNumber
        +String carrier
    }
    
    class InventoryHandler {
        -InventoryService inventoryService
        +handle(event: OrderCreatedEvent) void
    }
    
    class NotificationHandler {
        -EmailService emailService
        -SmsService smsService
        +handle(event: OrderCreatedEvent) void
        +handle(event: OrderShippedEvent) void
    }
    
    class AnalyticsHandler {
        -AnalyticsClient client
        +handle(event: IEvent) void
    }
    
    IEvent <|.. OrderCreatedEvent
    IEvent <|.. OrderShippedEvent
    IEventHandler~T~ <|.. InventoryHandler
    IEventHandler~T~ <|.. NotificationHandler
    IEventHandler~T~ <|.. AnalyticsHandler
    EventBus --> IEventHandler~T~ : dispatches to
    EventBus --> IEvent : publishes
```

---

## Best Practices

1. **Use stereotypes** — `<<interface>>`, `<<abstract>>`, `<<service>>`
2. **Show visibility** — Always include `+`, `-`, `#` modifiers
3. **Use generics** — `Repository~T~` instead of repeating for each type
4. **Label relationships** — `*-- "1..*"` with cardinality
5. **Group by layer** — Keep domain, application, infrastructure separate
6. **Focus on contracts** — Interfaces and public methods matter most
7. **Limit scope** — One design pattern or subsystem per diagram

---

## References

- [UML 2.5 Class Diagrams](https://www.omg.org/spec/UML/2.5.1/) — Formal specification
- [Mermaid Class Diagram Docs](https://mermaid.js.org/syntax/classDiagram.html) — Full syntax reference
- [Design Patterns](https://refactoring.guru/design-patterns) — Pattern catalog
