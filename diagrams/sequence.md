# Sequence Diagrams

---
title: "Sequence Diagrams"
status: published
owner: "PIMPyourDocs"
created: 2024-01-15
updated: 2024-01-15
tags: [diagrams, mermaid, sequence, uml]
---

## Overview

Sequence diagrams show interactions between components over time. They're based on UML 2.5 sequence diagram notation.

**Best for:**

- Protocol flows and handshakes
- API request/response documentation
- Authentication sequences
- Debugging complex multi-service interactions
- Incident timeline reconstruction

---

## Syntax Reference

### Participants

```mermaid
sequenceDiagram
    participant A as Alice
    participant B as Bob
    actor U as User
    
    U->>A: Request
    A->>B: Forward
    B-->>A: Response
    A-->>U: Result
```

### Arrow Types

| Arrow | Syntax | Meaning |
|-------|--------|---------|
| Solid line, solid head | `->>` | Synchronous call |
| Dashed line, solid head | `-->>` | Synchronous response |
| Solid line, open head | `->` | Asynchronous message |
| Dashed line, open head | `-->` | Asynchronous response |
| Solid line, cross | `-x` | Lost message |
| Dashed line, cross | `--x` | Lost response |

### Activation

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    
    C->>+S: Request
    S->>S: Process
    S-->>-C: Response
```

Use `+` to activate, `-` to deactivate. Shows when a component is "working".

### Notes

```mermaid
sequenceDiagram
    participant A
    participant B
    
    Note left of A: Client note
    Note right of B: Server note
    Note over A,B: Spans both participants
    
    A->>B: Message
```

### Loops

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    
    C->>S: Connect
    loop Every 30 seconds
        C->>S: Heartbeat
        S-->>C: ACK
    end
```

### Conditionals

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    participant DB as Database
    
    C->>S: GET /user/123
    S->>DB: SELECT * FROM users WHERE id=123
    
    alt User found
        DB-->>S: User record
        S-->>C: 200 OK + JSON
    else User not found
        DB-->>S: Empty result
        S-->>C: 404 Not Found
    end
```

### Optional Blocks

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    participant Cache
    
    C->>S: GET /data
    
    opt Cache enabled
        S->>Cache: Check cache
        Cache-->>S: Cache miss
    end
    
    S->>S: Compute result
    S-->>C: Response
```

### Parallel Execution

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    participant DB
    participant Cache
    
    C->>S: Request
    
    par Database query
        S->>DB: Query
        DB-->>S: Result
    and Cache update
        S->>Cache: Invalidate
        Cache-->>S: OK
    end
    
    S-->>C: Response
```

### Critical Sections

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    participant Lock
    
    C->>S: Update request
    
    critical Acquire lock
        S->>Lock: Lock resource
        Lock-->>S: Locked
        S->>S: Perform update
        S->>Lock: Release
    option Lock timeout
        Lock-->>S: Timeout
        S-->>C: 503 Service Unavailable
    end
    
    S-->>C: 200 OK
```

### Autonumbering

```mermaid
sequenceDiagram
    autonumber
    
    participant C as Client
    participant S as Server
    
    C->>S: Step one
    S->>S: Step two
    S-->>C: Step three
```

---

## Example: Windows UEFI Secure Boot Sequence

This diagram documents the complete Windows boot process from power-on through user login, including BitLocker TPM unsealing. Based on Microsoft's Boot Process documentation and UEFI Specification 2.9.

```mermaid
sequenceDiagram
    autonumber
    
    participant HW as Hardware/CPU
    participant UEFI as UEFI Firmware
    participant SB as Secure Boot
    participant TPM as TPM 2.0
    participant BL as BitLocker
    participant WBM as Windows Boot Manager
    participant WBL as Windows Boot Loader
    participant KERN as Windows Kernel
    participant SM as Session Manager
    participant WIN as Winlogon
    
    Note over HW,WIN: Phase 1: Platform Initialization (SEC/PEI)
    
    HW->>HW: Power-on Reset Vector (0xFFFFFFF0)
    HW->>UEFI: CPU executes firmware entry point
    UEFI->>UEFI: SEC Phase: CAR init, temp RAM
    UEFI->>UEFI: PEI Phase: Memory init, HOBs created
    UEFI->>TPM: TPM_Startup(CLEAR)
    TPM-->>UEFI: TPM initialized
    
    Note over HW,WIN: Phase 2: Driver Execution Environment (DXE)
    
    UEFI->>UEFI: DXE Phase: Load DXE drivers
    UEFI->>TPM: Extend PCR[0] with firmware hash
    TPM-->>UEFI: PCR[0] extended
    UEFI->>SB: Load Secure Boot databases (db/dbx/KEK/PK)
    SB-->>UEFI: Security policy active
    
    Note over HW,WIN: Phase 3: Boot Device Selection (BDS)
    
    UEFI->>UEFI: BDS Phase: Enumerate boot options
    UEFI->>UEFI: Read BootOrder variable
    UEFI->>SB: Verify bootmgfw.efi signature
    SB->>SB: Check against db/dbx
    
    alt Signature Invalid
        SB-->>UEFI: SECURITY_VIOLATION
        UEFI->>HW: Halt boot / Recovery
    else Signature Valid
        SB-->>UEFI: EFI_SUCCESS
    end
    
    UEFI->>TPM: Extend PCR[4] with boot app hash
    TPM-->>UEFI: PCR[4] extended
    UEFI->>WBM: Execute bootmgfw.efi
    
    Note over HW,WIN: Phase 4: Windows Boot Manager
    
    WBM->>WBM: Parse BCD (Boot Configuration Data)
    WBM->>TPM: Extend PCR[11] with boot policy
    TPM-->>WBM: PCR[11] extended
    
    WBM->>BL: Request Volume Master Key
    BL->>TPM: TPM2_Unseal with PCR policy
    
    alt PCR Mismatch (tampering detected)
        TPM-->>BL: TPM_RC_POLICY_FAIL
        BL-->>WBM: Recovery key required
        WBM->>WIN: Display recovery prompt
    else PCR Policy Satisfied
        TPM-->>BL: Unseal VMK (sealed blob)
        BL->>BL: Decrypt FVEK from VMK
        BL-->>WBM: Volume unlocked
    end
    
    WBM->>SB: Verify winload.efi signature
    SB-->>WBM: Signature valid
    WBM->>WBL: Execute winload.efi
    
    Note over HW,WIN: Phase 5: Windows Boot Loader
    
    WBL->>WBL: Load system registry hive (SYSTEM)
    WBL->>WBL: Load boot drivers (BOOT_START)
    WBL->>SB: Verify each driver signature
    SB-->>WBL: All drivers valid
    WBL->>TPM: Extend PCR[12] with kernel hash
    TPM-->>WBL: PCR[12] extended
    WBL->>KERN: Transfer to ntoskrnl.exe
    
    Note over HW,WIN: Phase 6: Kernel Initialization
    
    KERN->>KERN: Phase 0: HAL init, memory manager
    KERN->>KERN: Phase 1: Executive subsystems
    KERN->>KERN: Start SYSTEM_START drivers
    KERN->>KERN: Object Manager, Security RM
    KERN->>SM: Start smss.exe (Session 0)
    
    Note over HW,WIN: Phase 7: Session Manager & Login
    
    SM->>SM: Create Win32k subsystem
    SM->>SM: Start csrss.exe
    SM->>SM: Start wininit.exe (Session 0)
    SM->>WIN: Start winlogon.exe (Session 1)
    WIN->>WIN: Start LogonUI
    WIN-->>HW: Display login screen
    
    Note over HW,WIN: Boot Complete - Ctrl+Alt+Del to login
```


---

## Example: OAuth 2.0 Authorization Code Flow

```mermaid
sequenceDiagram
    autonumber
    
    participant U as User Browser
    participant App as Client App
    participant AS as Auth Server
    participant RS as Resource Server
    
    Note over U,RS: Authorization Request
    
    U->>App: Click "Login with Provider"
    App->>App: Generate state + PKCE verifier
    App->>U: Redirect to Auth Server
    U->>AS: GET /authorize?response_type=code&client_id=...&state=...&code_challenge=...
    
    Note over U,RS: User Authentication
    
    AS->>U: Display login form
    U->>AS: Submit credentials
    AS->>AS: Validate credentials
    AS->>U: Display consent screen
    U->>AS: Grant consent
    
    Note over U,RS: Authorization Response
    
    AS->>U: Redirect with code
    U->>App: GET /callback?code=...&state=...
    App->>App: Verify state matches
    
    Note over U,RS: Token Exchange
    
    App->>AS: POST /token (code + code_verifier)
    AS->>AS: Verify code_verifier against code_challenge
    AS-->>App: Access token + Refresh token
    
    Note over U,RS: Resource Access
    
    App->>RS: GET /api/resource (Bearer token)
    RS->>AS: Validate token (or introspect)
    AS-->>RS: Token valid
    RS-->>App: Protected resource
    App-->>U: Display data
```

---

## Example: Kafka Consumer Group Rebalance

```mermaid
sequenceDiagram
    autonumber
    
    participant C1 as Consumer 1
    participant C2 as Consumer 2
    participant C3 as Consumer 3 (new)
    participant GC as Group Coordinator
    participant K as Kafka Broker
    
    Note over C1,K: Steady State - 2 Consumers
    
    C1->>GC: Heartbeat (generation=1)
    GC-->>C1: OK
    C2->>GC: Heartbeat (generation=1)
    GC-->>C2: OK
    
    Note over C1,K: New Consumer Joins
    
    C3->>GC: JoinGroup request
    GC->>GC: Trigger rebalance
    
    par Notify existing consumers
        GC-->>C1: Heartbeat response: REBALANCE_IN_PROGRESS
        GC-->>C2: Heartbeat response: REBALANCE_IN_PROGRESS
    end
    
    Note over C1,K: JoinGroup Phase
    
    C1->>GC: JoinGroup (subscriptions)
    C2->>GC: JoinGroup (subscriptions)
    C3->>GC: JoinGroup (subscriptions)
    
    GC->>GC: Elect C1 as leader
    GC-->>C1: JoinGroup response (leader, all members)
    GC-->>C2: JoinGroup response (follower)
    GC-->>C3: JoinGroup response (follower)
    
    Note over C1,K: SyncGroup Phase
    
    C1->>C1: Compute partition assignments
    C1->>GC: SyncGroup (assignments for all)
    C2->>GC: SyncGroup (empty)
    C3->>GC: SyncGroup (empty)
    
    GC-->>C1: Assignment: [P0, P1]
    GC-->>C2: Assignment: [P2, P3]
    GC-->>C3: Assignment: [P4, P5]
    
    Note over C1,K: Resume Consumption (generation=2)
    
    par Fetch from assigned partitions
        C1->>K: Fetch P0, P1
        C2->>K: Fetch P2, P3
        C3->>K: Fetch P4, P5
    end
```

---

## Best Practices

1. **Use `autonumber`** for complex flows — makes referencing steps in documentation easier
2. **Group phases with `Note over`** — provides visual separation and context
3. **Use `alt/else` for error paths** — don't just show the happy path
4. **Limit participants to ~8** — split into multiple diagrams if needed
5. **Use aliases** — `participant DB as PostgreSQL` is clearer than a long name
6. **Show activation** — helps visualize which component is "working"
7. **Include protocol details** — method names, status codes, PCR numbers add precision

---

## References

- [UML 2.5 Specification](https://www.omg.org/spec/UML/2.5.1/) — Formal sequence diagram semantics
- [Mermaid Sequence Diagram Docs](https://mermaid.js.org/syntax/sequenceDiagram.html) — Full syntax reference
- [UEFI Specification 2.9](https://uefi.org/specifications) — Boot process reference
- [RFC 6749](https://tools.ietf.org/html/rfc6749) — OAuth 2.0 specification
