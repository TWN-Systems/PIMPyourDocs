# Architecture Diagrams

---
title: "Architecture Diagrams (C4 Model)"
status: published
owner: "PIMPyourDocs"
created: 2024-01-15
updated: 2024-01-15
tags: [diagrams, mermaid, architecture, c4, deployment]
---

## Overview

Architecture diagrams show system structure at various levels of abstraction. PIMPyourDocs follows the **C4 Model** approach.

**Best for:**

- System context (who uses what)
- Container architecture (services, databases, queues)
- Deployment topology (infrastructure)
- Component diagrams (internal structure)

---

## C4 Model Hierarchy

| Level         | Shows                             | Audience   |
| ------------- | --------------------------------- | ---------- |
| **Context**   | System + external actors          | Everyone   |
| **Container** | Applications, databases, services | Technical  |
| **Component** | Internal modules                  | Developers |
| **Code**      | Classes, functions                | Developers |

**Rule:** Start at Context, zoom in only when needed.

---

## Level 1: System Context

Shows who uses the system and what external systems it integrates with.

```mermaid
flowchart TB
    subgraph External Actors
        User([fa:fa-user Customer])
        Admin([fa:fa-user-shield Admin])
        Partner([fa:fa-building Partner API])
    end
    
    subgraph External Systems
        Email[fa:fa-envelope Email Provider]
        Payment[fa:fa-credit-card Payment Gateway]
        Analytics[fa:fa-chart-line Analytics]
    end
    
    subgraph System Boundary
        App[fa:fa-cube Our Application]
    end
    
    User -->|Uses| App
    Admin -->|Manages| App
    Partner -->|Integrates| App
    
    App -->|Sends notifications| Email
    App -->|Processes payments| Payment
    App -->|Reports events| Analytics
    
    style App fill:#1168bd,stroke:#0b4884,color:#fff
```

---

## Level 2: Container Diagram

Shows the major containers (applications, services, databases) within the system.

```mermaid
flowchart TB
    subgraph Internet
        User([fa:fa-user User])
        Mobile([fa:fa-mobile Mobile App])
    end
    
    subgraph "DMZ"
        CDN[fa:fa-cloud CloudFront CDN]
        WAF[fa:fa-shield AWS WAF]
    end
    
    subgraph "Public Subnet"
        ALB[fa:fa-balance-scale ALB]
        APIGW[fa:fa-door-open API Gateway]
    end
    
    subgraph "Private Subnet - Compute"
        direction TB
        subgraph "EKS Cluster"
            subgraph "Service Mesh"
                AUTH[Auth Service<br/>Go]
                USERS[Users Service<br/>Go]
                ORDERS[Orders Service<br/>Python]
                NOTIFY[Notification Service<br/>Node.js]
            end
        end
    end
    
    subgraph "Private Subnet - Data"
        direction TB
        RDS[(fa:fa-database PostgreSQL<br/>Primary)]
        RDS_R[(fa:fa-database PostgreSQL<br/>Read Replica)]
        REDIS[(fa:fa-bolt Redis Cluster)]
        KAFKA[fa:fa-stream Kafka]
    end
    
    subgraph "External Services"
        STRIPE[Stripe API]
        SENDGRID[SendGrid]
        TWILIO[Twilio]
    end
    
    User --> CDN
    Mobile --> APIGW
    CDN --> WAF --> ALB
    APIGW --> ALB
    ALB --> AUTH
    
    AUTH --> USERS
    AUTH --> ORDERS
    USERS --> RDS
    USERS --> REDIS
    ORDERS --> RDS
    ORDERS --> KAFKA
    
    KAFKA --> NOTIFY
    NOTIFY --> SENDGRID
    NOTIFY --> TWILIO
    ORDERS --> STRIPE
    
    RDS --> RDS_R
    
    classDef external fill:#999,stroke:#666
    classDef data fill:#438dd5,stroke:#2e6295,color:#fff
    classDef compute fill:#85bbf0,stroke:#5d99c6
    
    class STRIPE,SENDGRID,TWILIO external
    class RDS,RDS_R,REDIS,KAFKA data
    class AUTH,USERS,ORDERS,NOTIFY compute
```

---

## Level 3: Component Diagram

Shows the internal structure of a single container.

```mermaid
flowchart TB
    subgraph "Orders Service"
        direction TB
        
        subgraph "API Layer"
            REST[REST Controller]
            GQL[GraphQL Resolver]
            GRPC[gRPC Handler]
        end
        
        subgraph "Application Layer"
            CMD[Command Handlers]
            QRY[Query Handlers]
            EVT[Event Publishers]
        end
        
        subgraph "Domain Layer"
            AGG[Order Aggregate]
            SVC[Domain Services]
            POL[Domain Policies]
        end
        
        subgraph "Infrastructure Layer"
            REPO[Order Repository]
            MSG[Message Publisher]
            EXT[External Clients]
        end
    end
    
    REST --> CMD
    REST --> QRY
    GQL --> CMD
    GQL --> QRY
    GRPC --> CMD
    
    CMD --> AGG
    CMD --> SVC
    QRY --> REPO
    CMD --> EVT
    
    AGG --> POL
    SVC --> POL
    
    EVT --> MSG
    REPO --> DB[(PostgreSQL)]
    MSG --> KAFKA[Kafka]
    EXT --> STRIPE[Stripe]
```

---

## Deployment Diagram

Shows infrastructure topology.

```mermaid
flowchart TB
    subgraph AWS["AWS (us-east-1)"]
        subgraph VPC["VPC (10.0.0.0/16)"]
            subgraph AZ1["Availability Zone A"]
                subgraph Pub1["Public Subnet (10.0.1.0/24)"]
                    NAT1[NAT Gateway]
                    ALB1[ALB Node]
                end
                subgraph Priv1["Private Subnet (10.0.10.0/24)"]
                    EKS1[EKS Node]
                    RDS1[(RDS Primary)]
                end
            end
            
            subgraph AZ2["Availability Zone B"]
                subgraph Pub2["Public Subnet (10.0.2.0/24)"]
                    NAT2[NAT Gateway]
                    ALB2[ALB Node]
                end
                subgraph Priv2["Private Subnet (10.0.20.0/24)"]
                    EKS2[EKS Node]
                    RDS2[(RDS Standby)]
                end
            end
        end
        
        R53[Route 53]
        CF[CloudFront]
        S3[(S3 Bucket)]
    end
    
    Internet([Internet]) --> CF
    CF --> R53
    R53 --> ALB1 & ALB2
    ALB1 --> EKS1
    ALB2 --> EKS2
    EKS1 --> RDS1
    EKS2 --> RDS1
    RDS1 -.->|Replication| RDS2
    EKS1 & EKS2 --> S3
    EKS1 --> NAT1
    EKS2 --> NAT2
```

---

## Network Architecture

```mermaid
flowchart LR
    subgraph Internet
        Client([Client])
    end
    
    subgraph Edge["Edge (CloudFlare)"]
        DNS[DNS]
        WAF[WAF]
        CDN[CDN]
    end
    
    subgraph Ingress["Ingress Layer"]
        LB[Load Balancer<br/>L7]
        GW[API Gateway]
    end
    
    subgraph ServiceMesh["Service Mesh (Istio)"]
        direction TB
        Proxy1[Envoy Proxy]
        Proxy2[Envoy Proxy]
        Proxy3[Envoy Proxy]
    end
    
    subgraph Services
        SVC1[Service A]
        SVC2[Service B]
        SVC3[Service C]
    end
    
    subgraph Data
        DB[(Database)]
        Cache[(Cache)]
        Queue[Message Queue]
    end
    
    Client --> DNS --> WAF --> CDN
    CDN --> LB --> GW
    GW --> Proxy1 --> SVC1
    GW --> Proxy2 --> SVC2
    GW --> Proxy3 --> SVC3
    
    SVC1 --> DB
    SVC2 --> Cache
    SVC3 --> Queue
    
    Proxy1 <-.->|mTLS| Proxy2
    Proxy2 <-.->|mTLS| Proxy3
```

---

## Kubernetes Architecture

```mermaid
flowchart TB
    subgraph Cluster["Kubernetes Cluster"]
        subgraph ControlPlane["Control Plane"]
            API[API Server]
            ETCD[(etcd)]
            SCHED[Scheduler]
            CM[Controller Manager]
        end
        
        subgraph Workers["Worker Nodes"]
            subgraph Node1["Node 1"]
                K1[Kubelet]
                P1[Pod A]
                P2[Pod B]
            end
            
            subgraph Node2["Node 2"]
                K2[Kubelet]
                P3[Pod C]
                P4[Pod D]
            end
            
            subgraph Node3["Node 3"]
                K3[Kubelet]
                P5[Pod E]
                P6[Pod F]
            end
        end
    end
    
    API --> ETCD
    API --> SCHED
    API --> CM
    
    API --> K1
    API --> K2
    API --> K3
    
    K1 --> P1 & P2
    K2 --> P3 & P4
    K3 --> P5 & P6
    
    User([kubectl]) --> API
    Ingress([Ingress]) --> P1 & P3 & P5
```

---

## Best Practices

1. **Start with Context** — Don't jump to component diagrams
2. **One level per diagram** — Don't mix abstraction levels
3. **Color by type** — Blue for compute, green for data, grey for external
4. **Show data flow direction** — Arrows indicate request direction
5. **Label technologies** — "Go", "PostgreSQL", "Kafka"
6. **Use icons sparingly** — Font Awesome icons help recognition
7. **Include legends** — Document your color scheme

---

## References

- [C4 Model](https://c4model.com/) — Official C4 documentation
- [arc42](https://arc42.org/) — Architecture documentation template
- [Mermaid Flowchart Docs](https://mermaid.js.org/syntax/flowchart.html) — For architecture diagrams
