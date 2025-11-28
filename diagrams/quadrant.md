# Quadrant Charts

---
title: "Quadrant Charts"
status: published
owner: "PIMPyourDocs"
created: 2024-01-15
updated: 2024-01-15
tags: [diagrams, mermaid, quadrant, matrix, radar]
---

## Overview

Quadrant charts plot items on two axes, creating four regions for categorization.

**Best for:**

- Technology radar
- Priority matrices (Eisenhower matrix)
- Risk assessment
- Feature prioritization
- Competitive analysis

---

## Syntax Reference

### Basic Structure

```mermaid
quadrantChart
    title Chart Title
    x-axis Low X --> High X
    y-axis Low Y --> High Y
    
    quadrant-1 Top Right Label
    quadrant-2 Top Left Label
    quadrant-3 Bottom Left Label
    quadrant-4 Bottom Right Label
    
    Item A: [0.8, 0.7]
    Item B: [0.3, 0.4]
```

### Coordinates

- X and Y values range from 0 to 1
- [0, 0] is bottom-left
- [1, 1] is top-right

---

## Example: Technology Radar

```mermaid
quadrantChart
    title Technology Radar - Languages & Frameworks
    x-axis Low Adoption --> High Adoption
    y-axis Low Maturity --> High Maturity
    
    quadrant-1 Adopt
    quadrant-2 Trial
    quadrant-3 Assess
    quadrant-4 Hold
    
    Go: [0.8, 0.85]
    Rust: [0.45, 0.75]
    TypeScript: [0.9, 0.8]
    Python: [0.95, 0.9]
    Zig: [0.15, 0.35]
    Deno: [0.25, 0.5]
    Bun: [0.3, 0.4]
    HTMX: [0.35, 0.55]
    React: [0.95, 0.85]
    Svelte: [0.4, 0.7]
    Vue: [0.7, 0.8]
    Angular: [0.6, 0.75]
```

---

## Example: Eisenhower Priority Matrix

```mermaid
quadrantChart
    title Task Priority Matrix
    x-axis Not Urgent --> Urgent
    y-axis Not Important --> Important
    
    quadrant-1 Do First
    quadrant-2 Schedule
    quadrant-3 Delegate
    quadrant-4 Eliminate
    
    Production outage: [0.95, 0.95]
    Security patch: [0.85, 0.9]
    Customer escalation: [0.9, 0.8]
    Quarterly planning: [0.3, 0.85]
    Tech debt refactor: [0.25, 0.7]
    Documentation update: [0.2, 0.5]
    Meeting prep: [0.6, 0.4]
    Email backlog: [0.4, 0.3]
    Social media: [0.1, 0.1]
```

---

## Example: Risk Assessment

```mermaid
quadrantChart
    title Risk Assessment Matrix
    x-axis Low Likelihood --> High Likelihood
    y-axis Low Impact --> High Impact
    
    quadrant-1 Critical
    quadrant-2 Major
    quadrant-3 Minor
    quadrant-4 Moderate
    
    Data breach: [0.3, 0.95]
    DDoS attack: [0.6, 0.7]
    Key person leaves: [0.5, 0.6]
    Cloud provider outage: [0.2, 0.8]
    Dependency vulnerability: [0.8, 0.5]
    Database corruption: [0.15, 0.9]
    API rate limiting: [0.7, 0.3]
    Minor bug: [0.9, 0.15]
```

---

## Example: Feature Prioritization (Value vs Effort)

```mermaid
quadrantChart
    title Feature Prioritization
    x-axis Low Effort --> High Effort
    y-axis Low Value --> High Value
    
    quadrant-1 Major Projects
    quadrant-2 Quick Wins
    quadrant-3 Fill-ins
    quadrant-4 Time Sinks
    
    SSO Integration: [0.8, 0.9]
    Mobile App: [0.95, 0.85]
    Dark Mode: [0.2, 0.4]
    Export to CSV: [0.15, 0.6]
    Real-time Sync: [0.7, 0.75]
    Emoji Reactions: [0.1, 0.2]
    API v2: [0.85, 0.8]
    Keyboard Shortcuts: [0.25, 0.5]
    Custom Themes: [0.6, 0.3]
```

---

## Example: Competitive Analysis

```mermaid
quadrantChart
    title Competitive Landscape
    x-axis Low Market Share --> High Market Share
    y-axis Low Growth Rate --> High Growth Rate
    
    quadrant-1 Stars
    quadrant-2 Question Marks
    quadrant-3 Dogs
    quadrant-4 Cash Cows
    
    Our Product: [0.4, 0.8]
    Competitor A: [0.85, 0.3]
    Competitor B: [0.6, 0.5]
    Competitor C: [0.2, 0.6]
    New Entrant: [0.1, 0.9]
    Legacy Player: [0.7, 0.15]
```

---

## Example: Team Skills Matrix

```mermaid
quadrantChart
    title Team Skills Assessment
    x-axis Low Proficiency --> High Proficiency
    y-axis Low Strategic Value --> High Strategic Value
    
    quadrant-1 Invest
    quadrant-2 Develop
    quadrant-3 Maintain
    quadrant-4 Leverage
    
    Kubernetes: [0.6, 0.9]
    Machine Learning: [0.25, 0.85]
    React: [0.85, 0.7]
    Python: [0.9, 0.75]
    Rust: [0.2, 0.6]
    SQL: [0.95, 0.5]
    Terraform: [0.7, 0.8]
    Legacy Java: [0.8, 0.2]
```

---

## Best Practices

1. **Label axes clearly** — State what low/high means
2. **Name quadrants** — Use action-oriented labels (Adopt, Assess, Hold)
3. **Limit items** — 10-15 items max for readability
4. **Use consistent criteria** — All items evaluated on same scale
5. **Include legend** — Document what scores mean
6. **Update regularly** — Radar charts especially need periodic review
7. **Explain positioning** — Document why items are where they are

---

## Common Quadrant Frameworks

| Framework | X-Axis | Y-Axis | Use |
|-----------|--------|--------|-----|
| Eisenhower | Urgency | Importance | Task prioritization |
| BCG Matrix | Market Share | Growth Rate | Portfolio analysis |
| Risk Matrix | Likelihood | Impact | Risk assessment |
| Value/Effort | Effort | Value | Feature prioritization |
| Tech Radar | Adoption | Maturity | Technology decisions |

---

## References

- [Technology Radar](https://www.thoughtworks.com/radar) — ThoughtWorks example
- [Eisenhower Matrix](https://en.wikipedia.org/wiki/Time_management#The_Eisenhower_Method) — Priority framework
- [Mermaid Quadrant Docs](https://mermaid.js.org/syntax/quadrantChart.html) — Full syntax reference
