# Gantt Charts

---
title: "Gantt Charts"
status: published
owner: "PIMPyourDocs"
created: 2024-01-15
updated: 2024-01-15
tags: [diagrams, mermaid, gantt, timeline, project]
---

## Overview

Gantt charts visualize schedules, showing tasks over time with dependencies.

**Best for:**

- Incident timelines (post-mortems)
- Project schedules
- Release planning
- Sprint planning
- Migration timelines

---

## Syntax Reference

### Basic Structure

```mermaid
gantt
    title Project Schedule
    dateFormat YYYY-MM-DD
    
    section Phase 1
    Task 1    :a1, 2024-01-01, 7d
    Task 2    :a2, after a1, 5d
    
    section Phase 2
    Task 3    :b1, after a2, 10d
```

### Date Formats

| Format | Example |
|--------|---------|
| `YYYY-MM-DD` | 2024-01-15 |
| `HH:mm` | 14:30 |
| `YYYY-MM-DD HH:mm` | 2024-01-15 14:30 |

### Task Types

```mermaid
gantt
    dateFormat YYYY-MM-DD
    
    section Task Types
    Normal task      :a1, 2024-01-01, 5d
    Critical task    :crit, a2, after a1, 3d
    Active task      :active, a3, after a2, 4d
    Done task        :done, a4, after a3, 2d
    Milestone        :milestone, m1, after a4, 0d
```

| Modifier | Effect |
|----------|--------|
| `crit` | Critical path (red) |
| `active` | Currently active |
| `done` | Completed |
| `milestone` | Zero-duration marker |

### Dependencies

```mermaid
gantt
    dateFormat YYYY-MM-DD
    
    Task A    :a, 2024-01-01, 5d
    Task B    :b, after a, 3d
    Task C    :c, after a, 4d
    Task D    :d, after b c, 2d
```

---

## Example: Incident Timeline

```mermaid
gantt
    title INC-2024-0142: Database Connection Pool Exhaustion
    dateFormat HH:mm
    axisFormat %H:%M
    
    section Detection
    PagerDuty alert fired           :milestone, alert, 14:23, 0m
    On-call acknowledged            :ack, 14:23, 2m
    
    section Triage
    Check Grafana dashboards        :triage1, 14:25, 5m
    Identify connection leak        :crit, triage2, 14:30, 10m
    Root cause confirmed            :milestone, rc, 14:40, 0m
    
    section Mitigation
    Kill idle connections           :mit1, 14:40, 5m
    Restart affected pods           :mit2, 14:45, 8m
    Verify recovery                 :mit3, 14:53, 5m
    Service restored                :milestone, restored, 14:58, 0m
    
    section Follow-up
    Stakeholder notification        :follow1, 14:58, 10m
    Incident channel opened         :follow2, 15:08, 5m
    All-clear announcement          :milestone, clear, 15:13, 0m
    
    section Post-Incident
    Draft post-mortem               :post1, after clear, 120m
    Review meeting scheduled        :milestone, review, 17:13, 0m
```

---

## Example: Migration Project

```mermaid
gantt
    title Database Migration: PostgreSQL 14 → 16
    dateFormat YYYY-MM-DD
    excludes weekends
    
    section Planning
    Requirements gathering      :plan1, 2024-01-08, 5d
    Risk assessment             :plan2, after plan1, 3d
    Stakeholder approval        :milestone, m1, after plan2, 0d
    
    section Preparation
    Set up staging environment  :prep1, after m1, 5d
    Upgrade staging DB          :prep2, after prep1, 2d
    Application compatibility   :crit, prep3, after prep2, 10d
    Load testing                :prep4, after prep3, 5d
    Staging sign-off            :milestone, m2, after prep4, 0d
    
    section Production
    Maintenance window scheduled:milestone, m3, 2024-02-17, 0d
    Create production backup    :prod1, 2024-02-17, 1d
    Upgrade production DB       :crit, prod2, after prod1, 1d
    Application deployment      :prod3, after prod2, 1d
    Smoke tests                 :prod4, after prod3, 1d
    Production validation       :milestone, m4, after prod4, 0d
    
    section Rollback Window
    Monitor for issues          :active, roll1, after m4, 7d
    Rollback deadline           :milestone, m5, after roll1, 0d
```

---

## Example: Sprint Planning

```mermaid
gantt
    title Sprint 24.03 - User Authentication
    dateFormat YYYY-MM-DD
    excludes weekends
    
    section Auth Backend
    Design OAuth2 flow          :done, auth1, 2024-01-15, 2d
    Implement token service     :done, auth2, after auth1, 3d
    Add refresh token logic     :active, auth3, after auth2, 2d
    JWT validation middleware   :auth4, after auth3, 2d
    
    section Auth Frontend
    Login page UI               :done, fe1, 2024-01-15, 2d
    OAuth redirect handling     :active, fe2, after fe1, 2d
    Token storage (httpOnly)    :fe3, after fe2, 1d
    Auto-refresh logic          :fe4, after fe3 auth3, 2d
    
    section Testing
    Unit tests                  :test1, after auth4, 2d
    Integration tests           :crit, test2, after fe4 test1, 3d
    Security audit              :test3, after test2, 2d
    
    section Release
    Code freeze                 :milestone, m1, after test3, 0d
    Staging deployment          :rel1, after m1, 1d
    QA sign-off                 :rel2, after rel1, 2d
    Production release          :milestone, m2, after rel2, 0d
```

---

## Example: Release Train

```mermaid
gantt
    title Q1 2024 Release Schedule
    dateFormat YYYY-MM-DD
    
    section v2.1
    Development                 :done, v21dev, 2024-01-01, 21d
    Feature freeze              :milestone, v21ff, 2024-01-22, 0d
    Stabilization               :done, v21stab, 2024-01-22, 7d
    Release                     :milestone, v21rel, 2024-01-29, 0d
    
    section v2.2
    Development                 :active, v22dev, 2024-01-29, 21d
    Feature freeze              :milestone, v22ff, 2024-02-19, 0d
    Stabilization               :v22stab, 2024-02-19, 7d
    Release                     :milestone, v22rel, 2024-02-26, 0d
    
    section v2.3
    Development                 :v23dev, 2024-02-26, 21d
    Feature freeze              :milestone, v23ff, 2024-03-18, 0d
    Stabilization               :v23stab, 2024-03-18, 7d
    Release                     :milestone, v23rel, 2024-03-25, 0d
```

---

## Best Practices

1. **Use `excludes weekends`** for work schedules
2. **Mark milestones** — Zero-duration tasks for key dates
3. **Use `crit`** — Highlight critical path items
4. **Group with sections** — Logical phases improve readability
5. **Show dependencies** — `after task1 task2` for blockers
6. **Use appropriate format** — `HH:mm` for incidents, `YYYY-MM-DD` for projects
7. **Keep it focused** — One project/incident per chart

---

## References

- [Mermaid Gantt Docs](https://mermaid.js.org/syntax/gantt.html) — Full syntax reference
