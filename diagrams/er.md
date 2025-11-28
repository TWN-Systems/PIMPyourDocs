# Entity Relationship Diagrams

---
title: "Entity Relationship Diagrams"
status: published
owner: "PIMPyourDocs"
created: 2024-01-15
updated: 2024-01-15
tags: [diagrams, mermaid, er, database, schema]
---

## Overview

ER diagrams document database schemas, data models, and relationships between entities.

**Best for:**

- Database schema documentation
- API resource relationships
- Domain modeling
- Data dictionary visualization

---

## Syntax Reference

### Basic Relationships

```mermaid
erDiagram
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ LINE_ITEM : contains
    PRODUCT ||--o{ LINE_ITEM : "included in"
```

### Cardinality Notation

| Symbol | Meaning |
|--------|---------|
| `\|\|` | Exactly one |
| `o\|` | Zero or one |
| `\|{` | One or more |
| `o{` | Zero or more |

Reading: `A ||--o{ B` = "A has zero or more B"

### Entity Attributes

```mermaid
erDiagram
    USER {
        uuid id PK "Primary key"
        string email UK "Unique constraint"
        string password_hash
        enum role "admin|user|guest"
        timestamp created_at
        timestamp updated_at
    }
```

| Marker | Meaning |
|--------|---------|
| `PK` | Primary key |
| `FK` | Foreign key |
| `UK` | Unique key |

---

## Example: Multi-tenant SaaS Schema

```mermaid
erDiagram
    TENANT ||--o{ ORGANIZATION : "has many"
    ORGANIZATION ||--o{ USER : "employs"
    ORGANIZATION ||--o{ TEAM : "contains"
    TEAM ||--o{ TEAM_MEMBERSHIP : "has"
    USER ||--o{ TEAM_MEMBERSHIP : "belongs to"
    USER ||--o{ API_KEY : "owns"
    USER ||--o{ AUDIT_LOG : "generates"
    ORGANIZATION ||--o{ AUDIT_LOG : "scoped to"
    
    TENANT {
        uuid id PK "Partition key"
        string name UK "Tenant name"
        string subdomain UK "tenant.example.com"
        enum plan "free|pro|enterprise"
        jsonb settings "Feature flags"
        timestamp created_at
        timestamp deleted_at "Soft delete"
    }
    
    ORGANIZATION {
        uuid id PK
        uuid tenant_id FK "Partition key"
        string name
        string billing_email
        jsonb metadata
        timestamp created_at
    }
    
    USER {
        uuid id PK
        uuid org_id FK
        string email UK "Per-tenant unique"
        string password_hash "Argon2id"
        enum status "active|suspended|deleted"
        enum role "admin|member|viewer"
        timestamp last_login_at
        timestamp created_at
    }
    
    TEAM {
        uuid id PK
        uuid org_id FK
        string name
        string description
        timestamp created_at
    }
    
    TEAM_MEMBERSHIP {
        uuid id PK
        uuid team_id FK
        uuid user_id FK
        enum role "lead|member"
        timestamp joined_at
    }
    
    API_KEY {
        uuid id PK
        uuid user_id FK
        string key_hash UK "SHA-256"
        string prefix "First 8 chars for ID"
        string name
        jsonb scopes "Permission array"
        timestamp expires_at
        timestamp last_used_at
        timestamp created_at
    }
    
    AUDIT_LOG {
        uuid id PK
        uuid org_id FK "Partition key"
        uuid actor_id FK "User who acted"
        string action "resource.verb"
        string resource_type
        uuid resource_id
        jsonb changes "Before/after diff"
        inet ip_address
        timestamp created_at
    }
```

---

## Example: E-commerce Domain

```mermaid
erDiagram
    CUSTOMER ||--o{ ORDER : places
    CUSTOMER ||--o{ ADDRESS : has
    CUSTOMER ||--o{ PAYMENT_METHOD : saves
    
    ORDER ||--|{ ORDER_LINE : contains
    ORDER ||--o| SHIPPING_ADDRESS : "ships to"
    ORDER ||--o| BILLING_ADDRESS : "bills to"
    ORDER ||--o{ PAYMENT : "paid by"
    ORDER ||--o| DISCOUNT : uses
    
    PRODUCT ||--o{ ORDER_LINE : "ordered in"
    PRODUCT ||--o{ INVENTORY : "stocked in"
    PRODUCT }|--|| CATEGORY : "belongs to"
    PRODUCT ||--o{ PRODUCT_IMAGE : has
    PRODUCT ||--o{ PRODUCT_VARIANT : has
    
    WAREHOUSE ||--o{ INVENTORY : stores
    
    CUSTOMER {
        uuid id PK
        string email UK
        string password_hash
        string first_name
        string last_name
        timestamp created_at
    }
    
    ORDER {
        uuid id PK
        uuid customer_id FK
        uuid shipping_address_id FK
        uuid billing_address_id FK
        enum status "pending|paid|shipped|delivered|cancelled"
        decimal subtotal "Before tax/discount"
        decimal tax_amount
        decimal discount_amount
        decimal total
        timestamp ordered_at
        timestamp shipped_at
        timestamp delivered_at
    }
    
    ORDER_LINE {
        uuid id PK
        uuid order_id FK
        uuid product_id FK
        uuid variant_id FK
        integer quantity
        decimal unit_price "Price at time of order"
        decimal line_total
    }
    
    PRODUCT {
        uuid id PK
        uuid category_id FK
        string sku UK
        string name
        text description
        decimal base_price
        boolean is_active
        timestamp created_at
    }
    
    PRODUCT_VARIANT {
        uuid id PK
        uuid product_id FK
        string sku UK
        string name "e.g. Size: Large, Color: Blue"
        decimal price_modifier
        jsonb attributes
    }
    
    INVENTORY {
        uuid id PK
        uuid product_id FK
        uuid variant_id FK
        uuid warehouse_id FK
        integer quantity_on_hand
        integer quantity_reserved
        integer reorder_point
        timestamp last_counted_at
    }
```

---
3
## Example: Authentication System

```mermaid
erDiagram
    USER ||--o{ SESSION : has
    USER ||--o{ OAUTH_CONNECTION : "linked to"
    USER ||--o{ MFA_DEVICE : registers
    USER ||--o{ PASSWORD_RESET : requests
    USER ||--|| USER_PROFILE : has
    
    OAUTH_PROVIDER ||--o{ OAUTH_CONNECTION : provides
    
    USER {
        uuid id PK
        string email UK
        string password_hash
        boolean email_verified
        boolean mfa_enabled
        integer failed_login_attempts
        timestamp locked_until
        timestamp created_at
        timestamp updated_at
    }
    
    USER_PROFILE {
        uuid user_id PK
        string display_name
        string avatar_url
        string timezone
        json preferences
    }
    
    SESSION {
        uuid id PK
        uuid user_id FK
        string token_hash UK
        string ip_address
        string user_agent
        timestamp created_at
        timestamp expires_at
        timestamp last_active_at
    }
    
    OAUTH_PROVIDER {
        string id PK
        string name
        string authorization_url
        string token_url
        string userinfo_url
        boolean is_enabled
    }
    
    OAUTH_CONNECTION {
        uuid id PK
        uuid user_id FK
        string provider_id FK
        string provider_user_id
        string access_token
        string refresh_token
        timestamp token_expires_at
        json provider_data
        timestamp created_at
    }
    
    MFA_DEVICE {
        uuid id PK
        uuid user_id FK
        string type
        string name
        string secret
        json webauthn_credential
        boolean is_primary
        timestamp last_used_at
        timestamp created_at
    }
    
    PASSWORD_RESET {
        uuid id PK
        uuid user_id FK
        string token_hash UK
        timestamp expires_at
        timestamp used_at
        string requested_from_ip
        timestamp created_at
    }
```

---

## Best Practices

1. **Use clear relationship labels** — `places`, `contains`, `belongs to`
2. **Include key markers** — PK, FK, UK for clarity
3. **Add comments** — Inline comments explain non-obvious fields
4. **Show data types** — uuid, string, timestamp, jsonb, etc.
5. **Document enums** — List valid values: `"pending|paid|shipped"`
6. **Group logically** — Related entities should be visually close
7. **Don't overcrowd** — Split large schemas into domain-focused diagrams

---

## References

- [Chen ER Notation](https://en.wikipedia.org/wiki/Entity%E2%80%93relationship_model) — Original ER notation
- [Crow's Foot Notation](https://www.vertabelo.com/blog/crow-s-foot-notation/) — What Mermaid uses
- [Mermaid ER Diagram Docs](https://mermaid.js.org/syntax/entityRelationshipDiagram.html) — Full syntax reference
