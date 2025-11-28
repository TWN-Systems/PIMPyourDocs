# ADR-0001: Adopt PIMPyourDocs for Documentation

---
title: "Adopt PIMPyourDocs for Documentation"
status: accepted
owner: "Platform Team"
created: 2024-01-15
updated: 2024-01-15
tags: [architecture, documentation, tooling]
---

## Status

ACCEPTED

## Context

Our documentation is scattered across multiple platforms:

- Confluence for "official" documentation (rarely updated)
- Google Docs for proposals (lost after approval)
- Notion for team wikis (siloed by team)
- README files (inconsistent format)

This creates several problems:

1. **Discoverability** — Nobody knows where to find anything
2. **Staleness** — Docs are updated when created, then forgotten
3. **Portability** — We're locked into each platform's format
4. **Version control** — No history, no blame, no PRs
5. **AI readiness** — Our docs are inaccessible to RAG pipelines

The engineering team has grown from 10 to 50, and tribal knowledge is no longer sufficient.

## Decision

We MUST adopt PIMPyourDocs as our documentation framework.

All technical documentation MUST:
- Be written in Markdown
- Live in the repository it documents (or a central `docs/` repo)
- Use Mermaid for diagrams
- Follow the PIMPyourDocs specification

Existing documentation SHOULD be migrated within Q2.

Teams MAY continue using other tools for ephemeral content (meeting notes, brainstorms) but canonical documentation MUST be in Markdown.

## Consequences

### Positive

- **Single source of truth** — Docs live with code
- **Version controlled** — Full history via git
- **Review process** — PRs for documentation changes
- **Portable** — Can move platforms trivially
- **AI-ready** — Plain text works with any LLM tooling
- **No vendor lock-in** — Free, forever

### Negative

- **Learning curve** — Team needs to learn Markdown/Mermaid
- **Tooling setup** — Need to configure rendering pipeline
- **Migration effort** — Existing docs need to be converted
- **Less "rich"** — No fancy embedded widgets

### Neutral

- Confluence license can be cancelled after migration
- Need to establish ownership of the migration project

## Alternatives Considered

### Alternative 1: Standardize on Notion

**Description:** Move everything to Notion with strict structure.

**Pros:**
- Nice UI
- Good collaboration features
- Teams already use it

**Cons:**
- Vendor lock-in
- Export is lossy
- Not git-friendly
- Expensive at scale
- Data leaves our control

**Why rejected:** Doesn't solve the core problems of portability and version control.

### Alternative 2: Standardize on Confluence

**Description:** Mandate Confluence usage with templates.

**Pros:**
- Already have licenses
- Enterprise features

**Cons:**
- Terrible export
- No code review for docs
- Expensive
- Everyone hates using it

**Why rejected:** Doubles down on our existing problems.

### Alternative 3: Custom Wiki

**Description:** Build/deploy our own wiki (e.g., Wiki.js, BookStack).

**Pros:**
- Self-hosted
- Full control

**Cons:**
- Maintenance burden
- Still not code-native
- Another tool to learn

**Why rejected:** Adds complexity without solving the version control problem.

## Implementation

1. **Week 1-2:** Set up docs repository with PIMPyourDocs structure
2. **Week 3-4:** Document the framework, create team training
3. **Week 5-8:** Migrate critical documentation (runbooks, architecture)
4. **Week 9-12:** Migrate remaining documentation
5. **Week 13:** Cancel Confluence license

## References

- [PIMPyourDocs Specification](../SPEC.md)
- [Mermaid Diagram Standards](../diagrams/MERMAID.md)
- [Migration Playbook](./migrations/playbook.md)
