# Flowchart Diagrams

---
title: "Flowchart Diagrams"
status: published
owner: "PIMPyourDocs"
created: 2024-01-15
updated: 2024-01-15
tags: [diagrams, mermaid, flowchart]
---

## Overview

Flowcharts show processes, decisions, and data flow. They're the most versatile diagram type.

**Best for:**

- Decision trees and algorithms
- Process flows and workflows
- System architecture overviews
- Data flow visualization
- Error handling paths

---

## Syntax Reference

### Direction

| Syntax | Direction |
|--------|-----------|
| `flowchart TB` | Top to bottom |
| `flowchart BT` | Bottom to top |
| `flowchart LR` | Left to right |
| `flowchart RL` | Right to left |

### Node Shapes

```mermaid
flowchart LR
    A[Rectangle] --> B(Rounded)
    B --> C([Stadium])
    C --> D[[Subroutine]]
    D --> E[(Database)]
    E --> F((Circle))
    F --> G{Diamond}
    G --> H{{Hexagon}}
    H --> I[/Parallelogram/]
    I --> J[\Parallelogram Alt\]
    J --> K[/Trapezoid\]
    K --> L[\Trapezoid Alt/]
    L --> M(((Double Circle)))
```

| Shape | Syntax | Standard Meaning |
|-------|--------|------------------|
| Rectangle | `[text]` | Process / Action |
| Rounded | `(text)` | Alternate process |
| Stadium | `([text])` | Terminal (Start/End) |
| Subroutine | `[[text]]` | Predefined process |
| Cylinder | `[(text)]` | Database / Storage |
| Circle | `((text))` | Connector |
| Diamond | `{text}` | Decision |
| Hexagon | `{{text}}` | Preparation |
| Parallelogram | `[/text/]` | Input/Output |
| Trapezoid | `[/text\]` | Manual operation |
| Double Circle | `(((text)))` | Event / Trigger |

### Arrow Types

```mermaid
flowchart LR
    A --> B
    B --- C
    C -.- D
    D -.-> E
    E ==> F
    F --text--> G
    G -.text.-> H
```

| Syntax | Meaning |
|--------|---------|
| `-->` | Arrow |
| `---` | Line (no arrow) |
| `-.-` | Dotted line |
| `-.->` | Dotted arrow |
| `==>` | Thick arrow |
| `--text-->` | Arrow with label |
| `-.text.->` | Dotted arrow with label |

### Subgraphs

```mermaid
flowchart TB
    subgraph Frontend
        A[React App]
        B[Mobile App]
    end
    
    subgraph Backend
        C[API Gateway]
        D[Auth Service]
        E[Core Service]
    end
    
    subgraph Data
        F[(PostgreSQL)]
        G[(Redis)]
    end
    
    Frontend --> Backend
    Backend --> Data
```

### Styling

```mermaid
flowchart LR
    classDef success fill:#9f9,stroke:#333,stroke-width:2px
    classDef error fill:#f99,stroke:#333,stroke-width:2px
    classDef warning fill:#ff9,stroke:#333,stroke-width:2px
    
    A[Start] --> B{Check}
    B -->|Pass| C[Success]:::success
    B -->|Warn| D[Warning]:::warning
    B -->|Fail| E[Error]:::error
```

---

## Example: BitLocker Key Protector Decision Flow

This documents the complete BitLocker unlock decision tree during Windows boot.

```mermaid
flowchart TD
    subgraph Startup["System Startup"]
        A([Power On]) --> B{UEFI + TPM<br/>Present?}
    end
    
    subgraph TPMPath["TPM Path"]
        B -->|Yes| C{TPM Healthy?}
        C -->|Yes| D{PCR Values<br/>Match Policy?}
        D -->|Yes| E[TPM Unseals VMK]
        E --> F{PIN/Key<br/>Required?}
        F -->|No| G[Auto-unlock]
        F -->|Yes| H[Prompt for PIN/Key]
        H --> I{Valid?}
        I -->|Yes| G
        I -->|No| J[Increment Lockout Counter]
        J --> K{Lockout<br/>Threshold?}
        K -->|No| H
        K -->|Yes| L[Recovery Required]
    end
    
    subgraph Recovery["Recovery Path"]
        B -->|No| L
        C -->|No| L
        D -->|No| L
        L --> M{Recovery Key<br/>Available?}
        M -->|Yes| N[Enter 48-digit Key]
        N --> O{Key Valid?}
        O -->|Yes| P[Unlock + Re-seal to TPM]
        O -->|No| Q[Boot Failure]
        M -->|No| Q
    end
    
    subgraph Success["Boot Continues"]
        G --> R[Decrypt FVEK]
        P --> R
        R --> S[Mount Encrypted Volume]
        S --> T([Continue Boot])
    end
    
    style L fill:#f96,stroke:#333,stroke-width:2px
    style Q fill:#f00,stroke:#333,stroke-width:2px,color:#fff
    style G fill:#9f9,stroke:#333,stroke-width:2px
    style T fill:#9f9,stroke:#333,stroke-width:2px
```

---

## Example: HTTP Request Processing Pipeline

```mermaid
flowchart TB
    subgraph Ingress["Ingress Layer"]
        A([Request]) --> B[Load Balancer]
        B --> C{Health Check}
        C -->|Unhealthy| D[503 Service Unavailable]
        C -->|Healthy| E[WAF]
        E --> F{Threat Detected?}
        F -->|Yes| G[403 Forbidden]
        F -->|No| H[Rate Limiter]
        H --> I{Over Limit?}
        I -->|Yes| J[429 Too Many Requests]
        I -->|No| K[API Gateway]
    end
    
    subgraph Auth["Authentication"]
        K --> L{Auth Header?}
        L -->|No| M[401 Unauthorized]
        L -->|Yes| N[Validate Token]
        N --> O{Token Valid?}
        O -->|No| M
        O -->|Expired| P[Refresh Flow]
        O -->|Yes| Q[Extract Claims]
    end
    
    subgraph Authz["Authorization"]
        Q --> R{Has Permission?}
        R -->|No| S[403 Forbidden]
        R -->|Yes| T[Route to Service]
    end
    
    subgraph Processing["Request Processing"]
        T --> U[Service Handler]
        U --> V{Validation OK?}
        V -->|No| W[400 Bad Request]
        V -->|Yes| X[Business Logic]
        X --> Y{Success?}
        Y -->|No| Z[500 Internal Error]
        Y -->|Yes| AA[200 OK]
    end
    
    subgraph Response["Response"]
        D --> AB([Response])
        G --> AB
        J --> AB
        M --> AB
        S --> AB
        W --> AB
        Z --> AB
        AA --> AB
    end
```

---

## Example: CI/CD Pipeline

```mermaid
flowchart LR
    subgraph Trigger
        A[Push to main] --> B{PR Merged?}
        B -->|No| C[Run Tests Only]
        B -->|Yes| D[Full Pipeline]
    end
    
    subgraph Build
        D --> E[Checkout Code]
        E --> F[Install Dependencies]
        F --> G[Lint]
        G --> H{Lint Pass?}
        H -->|No| I[Fail Build]
        H -->|Yes| J[Unit Tests]
        J --> K{Tests Pass?}
        K -->|No| I
        K -->|Yes| L[Build Artifacts]
    end
    
    subgraph Security
        L --> M[SAST Scan]
        M --> N[Dependency Scan]
        N --> O[Container Scan]
        O --> P{Vulnerabilities?}
        P -->|Critical| I
        P -->|None/Low| Q[Sign Artifacts]
    end
    
    subgraph Deploy
        Q --> R[Deploy to Staging]
        R --> S[Integration Tests]
        S --> T{Tests Pass?}
        T -->|No| I
        T -->|Yes| U{Auto-deploy?}
        U -->|No| V[Manual Approval]
        V --> W[Deploy to Production]
        U -->|Yes| W
        W --> X[Smoke Tests]
        X --> Y{Healthy?}
        Y -->|No| Z[Rollback]
        Y -->|Yes| AA([Success])
    end
    
    I --> AB([Pipeline Failed])
    Z --> AB
```

---

## Example: Error Handling Decision Tree

```mermaid
flowchart TD
    A[Error Caught] --> B{Error Type?}
    
    B -->|Validation| C[400 Bad Request]
    B -->|Authentication| D[401 Unauthorized]
    B -->|Authorization| E[403 Forbidden]
    B -->|Not Found| F[404 Not Found]
    B -->|Conflict| G[409 Conflict]
    B -->|Rate Limit| H[429 Too Many Requests]
    B -->|Server Error| I{Retryable?}
    
    I -->|Yes| J{Retry Count < 3?}
    J -->|Yes| K[Exponential Backoff]
    K --> L[Retry Request]
    L --> M{Success?}
    M -->|Yes| N[Return Result]
    M -->|No| J
    J -->|No| O[503 Service Unavailable]
    
    I -->|No| P[500 Internal Server Error]
    
    C --> Q[Log + Return Error]
    D --> Q
    E --> Q
    F --> Q
    G --> Q
    H --> Q
    O --> Q
    P --> Q
    
    Q --> R{Alert Required?}
    R -->|Yes| S[Send to PagerDuty]
    R -->|No| T[Log Only]
```

---

## Best Practices

1. **Use TB for hierarchies, LR for processes** — Follow natural reading direction
2. **Group with subgraphs** — Logical separation improves readability
3. **Color-code outcomes** — Green for success, red for error, yellow for warning
4. **Label all decision branches** — Every arrow from a diamond should have text
5. **Use consistent shapes** — Diamond for decisions, stadium for terminals
6. **Limit to ~15 nodes** — Split complex flows into multiple diagrams
7. **Show error paths** — Don't just document the happy path

---

## References

- [ISO 5807](https://www.iso.org/standard/11955.html) — Flowchart symbols standard
- [Mermaid Flowchart Docs](https://mermaid.js.org/syntax/flowchart.html) — Full syntax reference
