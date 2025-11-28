# ADR-{NUMBER}: {TITLE}

---
title: "{TITLE}"
status: proposed | accepted | deprecated | superseded
owner: "{TEAM_OR_PERSON}"
created: {YYYY-MM-DD}
updated: {YYYY-MM-DD}
tags: [architecture, {domain}]
supersedes: ADR-{N}  # if applicable
superseded_by: ADR-{N}  # if applicable
---

## Status

{PROPOSED | ACCEPTED | DEPRECATED | SUPERSEDED}

## Context

What is the issue that we're seeing that is motivating this decision or change?

Describe the forces at play:
- Technical constraints
- Business requirements  
- Team capabilities
- Timeline pressures
- Dependencies

## Decision

What is the change that we're proposing and/or doing?

State the decision clearly using RFC 2119 language:
- We MUST...
- We SHOULD...
- We MAY...

## Consequences

What becomes easier or more difficult to do because of this change?

### Positive

- Benefit 1
- Benefit 2

### Negative

- Tradeoff 1
- Tradeoff 2

### Neutral

- Change that is neither good nor bad

## Alternatives Considered

### Alternative 1: {NAME}

**Description:** Brief description

**Pros:**
- Pro 1

**Cons:**
- Con 1

**Why rejected:** Reason

### Alternative 2: {NAME}

...

## Implementation

High-level implementation notes or links to relevant tickets/PRs.

## References

- [Link to relevant documentation]
- [Link to relevant discussion]

---

## ADR Template Notes

**ADRs are immutable once accepted.** If a decision changes, create a new ADR that supersedes this one.

**File naming:** `{NNNN}-{kebab-case-title}.md` (e.g., `0001-use-postgresql.md`)

**Status transitions:**
```
proposed → accepted → [deprecated | superseded]
```
