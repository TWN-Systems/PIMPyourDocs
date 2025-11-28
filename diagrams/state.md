# State Diagrams

---
title: "State Diagrams"
status: published
owner: "PIMPyourDocs"
created: 2024-01-15
updated: 2024-01-15
tags: [diagrams, mermaid, state, uml, fsm]
---

## Overview

State diagrams (statecharts) show the lifecycle of an object or system, including all possible states and the transitions between them.

**Best for:**

- Object lifecycle documentation
- Protocol state machines
- Workflow status tracking
- Order/ticket/document lifecycles
- Connection state management

---

## Syntax Reference

### Basic States

```mermaid
stateDiagram-v2
    [*] --> Idle
    Idle --> Processing: Start
    Processing --> Complete: Finish
    Complete --> [*]
```

| Element | Syntax | Meaning |
|---------|--------|---------|
| Initial | `[*] -->` | Start state |
| Final | `--> [*]` | End state |
| State | `StateName` | Named state |
| Transition | `A --> B: event` | State change |

### State Descriptions

```mermaid
stateDiagram-v2
    state "Waiting for Payment" as WFP
    state "Payment Received" as PR
    
    [*] --> WFP
    WFP --> PR: payment_received
    PR --> [*]
```

### Composite States

```mermaid
stateDiagram-v2
    [*] --> Active
    
    state Active {
        [*] --> Idle
        Idle --> Running: start
        Running --> Idle: stop
    }
    
    Active --> Terminated: shutdown
    Terminated --> [*]
```

### Parallel States (Fork/Join)

```mermaid
stateDiagram-v2
    [*] --> Active
    
    state Active {
        state fork <<fork>>
        state join <<join>>
        
        [*] --> fork
        fork --> NetworkCheck
        fork --> DiskCheck
        
        NetworkCheck --> join
        DiskCheck --> join
        join --> Ready
    }
    
    Active --> [*]
```

### Choice (Conditional)

```mermaid
stateDiagram-v2
    [*] --> Validating
    
    state check <<choice>>
    
    Validating --> check
    check --> Approved: [valid]
    check --> Rejected: [invalid]
    
    Approved --> [*]
    Rejected --> [*]
```

### Notes

```mermaid
stateDiagram-v2
    [*] --> Active
    Active --> Inactive: timeout
    
    note right of Active
        This is the normal
        operating state
    end note
    
    note left of Inactive
        Requires manual
        reactivation
    end note
```

---

## Example: TLS 1.3 Handshake States (Client)

Based on RFC 8446 TLS 1.3 specification.

```mermaid
stateDiagram-v2
    [*] --> START: Connection initiated
    
    START --> WAIT_SH: Send ClientHello
    
    state WAIT_SH {
        [*] --> Waiting
        Waiting --> Received: ServerHello received
    }
    
    WAIT_SH --> WAIT_EE: Process ServerHello
    
    state "Wait Encrypted Extensions" as WAIT_EE {
        [*] --> Decrypt
        Decrypt --> Parse: Decrypt with handshake keys
    }
    
    WAIT_EE --> WAIT_CERT_CR: EncryptedExtensions received
    
    state WAIT_CERT_CR {
        [*] --> CheckAuth
        CheckAuth --> CertPath: Server sends Certificate
        CheckAuth --> NoCert: PSK mode (no cert)
    }
    
    WAIT_CERT_CR --> WAIT_CV: Certificate received
    
    WAIT_CV --> WAIT_FINISHED: CertificateVerify valid
    WAIT_CV --> ALERT: CertificateVerify invalid
    
    state WAIT_FINISHED {
        [*] --> VerifyMAC
        VerifyMAC --> Derived: Verify Finished MAC
    }
    
    WAIT_FINISHED --> CONNECTED: Finished verified
    WAIT_FINISHED --> ALERT: Finished invalid
    
    CONNECTED --> [*]: Connection closed
    
    ALERT --> [*]: Fatal alert sent
    
    note right of START
        RFC 8446 TLS 1.3
        Client State Machine
    end note
    
    note right of CONNECTED
        Application data
        can now flow
    end note
```

---

## Example: Order Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Draft: create_order
    
    Draft --> Submitted: submit
    Draft --> Cancelled: cancel
    
    state Submitted {
        [*] --> PendingPayment
        PendingPayment --> PaymentReceived: payment_success
        PendingPayment --> PaymentFailed: payment_failed
        PaymentFailed --> PendingPayment: retry_payment
    }
    
    Submitted --> Processing: payment_confirmed
    Submitted --> Cancelled: cancel
    
    state Processing {
        [*] --> Picking
        Picking --> Packing: items_picked
        Packing --> ReadyToShip: packed
    }
    
    Processing --> Shipped: ship
    Processing --> Cancelled: cancel
    
    state Shipped {
        [*] --> InTransit
        InTransit --> OutForDelivery: out_for_delivery
        OutForDelivery --> Delivered: delivered
        OutForDelivery --> DeliveryFailed: delivery_failed
        DeliveryFailed --> InTransit: reattempt
    }
    
    Shipped --> Delivered: confirm_delivery
    
    Delivered --> Returned: initiate_return
    Delivered --> Completed: finalize
    
    state Returned {
        [*] --> ReturnRequested
        ReturnRequested --> ReturnApproved: approve
        ReturnRequested --> ReturnDenied: deny
        ReturnApproved --> ReturnReceived: receive_return
        ReturnReceived --> Refunded: process_refund
    }
    
    Refunded --> [*]
    Completed --> [*]
    Cancelled --> [*]
    
    note right of Draft
        Customer can edit
        order details
    end note
    
    note right of Cancelled
        No refund needed
        if payment not received
    end note
```

---

## Example: TCP Connection States

Based on RFC 793.

```mermaid
stateDiagram-v2
    [*] --> CLOSED
    
    CLOSED --> LISTEN: passive open
    CLOSED --> SYN_SENT: active open / send SYN
    
    LISTEN --> SYN_RCVD: rcv SYN / send SYN,ACK
    LISTEN --> CLOSED: close
    
    SYN_SENT --> ESTABLISHED: rcv SYN,ACK / send ACK
    SYN_SENT --> SYN_RCVD: rcv SYN / send SYN,ACK
    SYN_SENT --> CLOSED: timeout / RST
    
    SYN_RCVD --> ESTABLISHED: rcv ACK
    SYN_RCVD --> FIN_WAIT_1: close / send FIN
    SYN_RCVD --> LISTEN: rcv RST
    
    ESTABLISHED --> FIN_WAIT_1: close / send FIN
    ESTABLISHED --> CLOSE_WAIT: rcv FIN / send ACK
    
    FIN_WAIT_1 --> FIN_WAIT_2: rcv ACK
    FIN_WAIT_1 --> CLOSING: rcv FIN / send ACK
    FIN_WAIT_1 --> TIME_WAIT: rcv FIN,ACK / send ACK
    
    FIN_WAIT_2 --> TIME_WAIT: rcv FIN / send ACK
    
    CLOSE_WAIT --> LAST_ACK: close / send FIN
    
    CLOSING --> TIME_WAIT: rcv ACK
    
    LAST_ACK --> CLOSED: rcv ACK
    
    TIME_WAIT --> CLOSED: 2MSL timeout
    
    note right of ESTABLISHED
        Data transfer
        can occur
    end note
    
    note right of TIME_WAIT
        Wait 2x Maximum
        Segment Lifetime
    end note
```

---

## Example: Git File States

```mermaid
stateDiagram-v2
    [*] --> Untracked: create file
    
    Untracked --> Staged: git add
    Untracked --> [*]: delete file
    
    state "Tracked" as T {
        Unmodified --> Modified: edit file
        Modified --> Staged: git add
        Modified --> Unmodified: git checkout
        Staged --> Unmodified: git commit
        Staged --> Modified: edit file
    }
    
    Staged --> Unmodified: git commit
    Untracked --> T: git add + commit
    
    T --> Untracked: git rm --cached
    T --> [*]: git rm
    
    note right of Staged
        Changes ready
        to be committed
    end note
```

---

## Example: Kubernetes Pod Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Pending: Pod created
    
    state Pending {
        [*] --> Scheduling
        Scheduling --> Scheduled: Node assigned
        Scheduled --> ImagePulling: Init containers ready
        ImagePulling --> ImagePulled: Images available
    }
    
    Pending --> Running: All containers started
    Pending --> Failed: Unschedulable / Image pull failed
    
    state Running {
        [*] --> Healthy
        Healthy --> Unhealthy: Probe failed
        Unhealthy --> Healthy: Probe passed
        Unhealthy --> CrashLoopBackOff: Repeated failures
        CrashLoopBackOff --> Healthy: Container recovers
    }
    
    Running --> Succeeded: All containers exit 0
    Running --> Failed: Container exits non-zero
    Running --> Terminating: Delete requested
    
    state Terminating {
        [*] --> PreStop
        PreStop --> GracePeriod: Hook complete
        GracePeriod --> Terminated: SIGTERM sent
    }
    
    Terminating --> [*]: Pod removed
    Succeeded --> [*]
    Failed --> [*]
    
    note right of Pending
        Waiting for
        resources
    end note
    
    note right of CrashLoopBackOff
        Exponential
        backoff delay
    end note
```

---

## Best Practices

1. **Always show `[*]`** — Clearly indicate start and end states
2. **Use composite states** — Group related states for clarity
3. **Label all transitions** — Every arrow should have an event/trigger
4. **Add notes** — Explain non-obvious states
5. **Keep it flat when possible** — Avoid deep nesting
6. **Document error states** — Show failure paths, not just happy path
7. **Reference specifications** — Link to RFCs/specs for protocol states

---

## References

- [UML 2.5 State Machine Diagrams](https://www.omg.org/spec/UML/2.5.1/) — Formal semantics
- [Mermaid State Diagram Docs](https://mermaid.js.org/syntax/stateDiagram.html) — Full syntax reference
- [RFC 8446](https://tools.ietf.org/html/rfc8446) — TLS 1.3 specification
- [RFC 793](https://tools.ietf.org/html/rfc793) — TCP specification
