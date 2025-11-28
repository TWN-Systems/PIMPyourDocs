# User Journey Diagrams

---
title: "User Journey Diagrams"
status: published
owner: "PIMPyourDocs"
created: 2024-01-15
updated: 2024-01-15
tags: [diagrams, mermaid, journey, ux, cx]
---

## Overview

User journey diagrams map the experience of users through a process, showing their emotional state at each step.

**Best for:**

- Customer experience mapping
- Onboarding flow documentation
- Support process visualization
- Feature discovery flows
- Pain point identification

---

## Syntax Reference

### Basic Structure

```mermaid
journey
    title My Journey
    section Section Name
        Task name: score: actor1, actor2
```

### Scores

Scores range from 1 (worst) to 5 (best), indicating user satisfaction:

| Score | Meaning |
|-------|---------|
| 1 | Very frustrated |
| 2 | Frustrated |
| 3 | Neutral |
| 4 | Satisfied |
| 5 | Delighted |

---

## Example: User Onboarding

```mermaid
journey
    title New User Onboarding Journey
    
    section Discovery
        Visit landing page: 5: Visitor
        Read features: 4: Visitor
        View pricing: 3: Visitor
        Compare competitors: 3: Visitor
        
    section Signup
        Click Get Started: 5: User
        Fill signup form: 3: User
        Verify email: 4: User
        
    section Onboarding
        Complete profile: 4: User
        Watch tutorial video: 2: User
        Create first project: 5: User
        Invite team member: 4: User
        
    section Activation
        Complete key workflow: 5: User
        Discover advanced feature: 4: User
        Hit usage limit: 2: User
        Upgrade prompt: 3: User
        
    section Conversion
        Review pricing: 3: User
        Enter payment info: 2: User
        Confirm subscription: 4: User
        Receive confirmation: 5: User
```

---

## Example: Support Ticket Journey

```mermaid
journey
    title Customer Support Experience
    
    section Issue Discovery
        Encounter bug: 1: Customer
        Search documentation: 3: Customer
        Documentation unhelpful: 2: Customer
        
    section Contact Support
        Find support page: 3: Customer
        Submit ticket: 4: Customer
        Receive auto-reply: 3: Customer
        
    section Resolution
        Wait for response: 2: Customer
        First agent response: 4: Customer, Agent
        Provide more details: 3: Customer
        Issue escalated: 2: Customer
        Senior agent assigned: 4: Customer, Senior Agent
        Solution provided: 5: Customer, Senior Agent
        
    section Follow-up
        Verify fix works: 5: Customer
        Rate support: 4: Customer
        Receive survey: 3: Customer
```

---

## Example: E-commerce Purchase

```mermaid
journey
    title E-commerce Purchase Journey
    
    section Awareness
        See social media ad: 4: Shopper
        Click through to site: 4: Shopper
        
    section Consideration
        Browse products: 4: Shopper
        Read reviews: 5: Shopper
        Compare options: 3: Shopper
        Check shipping cost: 2: Shopper
        
    section Decision
        Add to cart: 5: Customer
        View cart: 4: Customer
        Enter shipping info: 3: Customer
        See final price: 2: Customer
        Enter payment: 3: Customer
        Complete order: 5: Customer
        
    section Post-Purchase
        Receive confirmation: 5: Customer
        Track shipment: 4: Customer
        Receive package: 5: Customer
        Unbox product: 5: Customer
        Leave review: 4: Customer
```

---

## Example: Developer API Integration

```mermaid
journey
    title API Integration Journey
    
    section Discovery
        Find API in docs: 4: Developer
        Read getting started: 4: Developer
        Review authentication: 3: Developer
        
    section Setup
        Create account: 4: Developer
        Generate API key: 5: Developer
        Set up environment: 3: Developer
        
    section First Call
        Copy example code: 5: Developer
        Run first request: 2: Developer
        Debug auth error: 1: Developer
        Find solution in docs: 3: Developer
        Successful first call: 5: Developer
        
    section Integration
        Implement use case: 4: Developer
        Handle edge cases: 3: Developer
        Hit rate limit: 2: Developer
        Implement retry logic: 3: Developer
        
    section Production
        Deploy to staging: 4: Developer
        Run integration tests: 4: Developer
        Deploy to production: 5: Developer
        Monitor in dashboard: 5: Developer
```

---

## Example: Multi-Actor Journey

```mermaid
journey
    title Job Application Process
    
    section Application
        Find job posting: 4: Candidate
        Submit application: 3: Candidate
        Receive application: 4: Recruiter
        
    section Screening
        Review resume: 4: Recruiter
        Schedule phone screen: 3: Candidate, Recruiter
        Conduct phone screen: 4: Candidate, Recruiter
        
    section Interview
        Schedule on-site: 3: Candidate, Recruiter
        Prepare for interview: 3: Candidate
        Conduct interviews: 4: Candidate, Hiring Manager
        Debrief with team: 4: Hiring Manager, Recruiter
        
    section Decision
        Extend offer: 5: Recruiter
        Receive offer: 5: Candidate
        Negotiate terms: 3: Candidate, Recruiter
        Accept offer: 5: Candidate, Recruiter, Hiring Manager
        
    section Onboarding
        First day: 4: New Hire, HR
        Meet team: 5: New Hire
        Complete training: 3: New Hire
```

---

## Best Practices

1. **Use realistic scores** — Don't make everything a 5
2. **Identify pain points** — Low scores highlight improvement areas
3. **Show multiple actors** — Different perspectives reveal gaps
4. **Group by phase** — Sections should be logical journey stages
5. **Be specific** — "Fill 10-field form" beats "Sign up"
6. **Validate with users** — Scores should reflect real feedback
7. **Keep it focused** — One persona's journey per diagram

---

## References

- [Customer Journey Mapping](https://www.nngroup.com/articles/customer-journey-mapping/) — Nielsen Norman Group guide
- [Mermaid User Journey Docs](https://mermaid.js.org/syntax/userJourney.html) — Full syntax reference
