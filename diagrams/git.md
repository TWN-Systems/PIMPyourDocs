# Git Graphs

---
title: "Git Graphs"
status: published
owner: "PIMPyourDocs"
created: 2024-01-15
updated: 2024-01-15
tags: [diagrams, mermaid, git, branching]
---

## Overview

Git graphs visualize branching strategies, merge flows, and release processes.

**Best for:**

- Branching strategy documentation
- Release process visualization
- Merge conflict scenarios
- Git workflow training

---

## Syntax Reference

### Basic Operations

```mermaid
gitGraph
    commit id: "Initial"
    commit id: "Add feature"
    branch develop
    checkout develop
    commit id: "Dev work"
    checkout main
    merge develop id: "Merge"
    commit id: "Hotfix"
```

### Commands

| Command | Description |
|---------|-------------|
| `commit` | Add a commit |
| `commit id: "text"` | Commit with message |
| `commit tag: "v1.0"` | Commit with tag |
| `branch name` | Create branch |
| `checkout name` | Switch to branch |
| `merge name` | Merge branch |
| `cherry-pick id` | Cherry-pick commit |

### Commit Types

```mermaid
gitGraph
    commit id: "Normal"
    commit id: "Highlight" type: HIGHLIGHT
    commit id: "Reverse" type: REVERSE
```

---

## Example: GitFlow

```mermaid
gitGraph
    commit id: "Initial" tag: "v1.0.0"
    
    branch develop
    checkout develop
    commit id: "Dev setup"
    
    branch feature/auth
    checkout feature/auth
    commit id: "Add login"
    commit id: "Add logout"
    commit id: "Add session"
    
    checkout develop
    merge feature/auth id: "Merge auth"
    
    branch feature/api
    checkout feature/api
    commit id: "REST endpoints"
    commit id: "Add validation"
    
    checkout develop
    merge feature/api id: "Merge API"
    
    branch release/1.1
    checkout release/1.1
    commit id: "Bump version"
    commit id: "Fix bug"
    
    checkout main
    merge release/1.1 id: "Release" tag: "v1.1.0"
    
    checkout develop
    merge release/1.1 id: "Back-merge"
    
    checkout main
    branch hotfix/security
    commit id: "CVE fix"
    
    checkout main
    merge hotfix/security id: "Hotfix" tag: "v1.1.1"
    
    checkout develop
    merge hotfix/security id: "Backport"
```

---

## Example: GitHub Flow

```mermaid
gitGraph
    commit id: "Initial" tag: "v1.0"
    
    branch feature/user-profile
    checkout feature/user-profile
    commit id: "Add profile page"
    commit id: "Add avatar upload"
    commit id: "Add settings"
    
    checkout main
    merge feature/user-profile id: "PR #42" tag: "v1.1"
    
    branch feature/notifications
    checkout feature/notifications
    commit id: "Add notification model"
    commit id: "Add push support"
    
    checkout main
    merge feature/notifications id: "PR #43" tag: "v1.2"
    
    branch bugfix/login-issue
    checkout bugfix/login-issue
    commit id: "Fix OAuth redirect"
    
    checkout main
    merge bugfix/login-issue id: "PR #44" tag: "v1.2.1"
```

---

## Example: Trunk-Based Development

```mermaid
gitGraph
    commit id: "Initial"
    commit id: "Feature A (flag)" tag: "v1.0"
    commit id: "Feature B (flag)"
    commit id: "Feature C (flag)" tag: "v1.1"
    commit id: "Enable Feature A"
    commit id: "Fix regression" tag: "v1.1.1"
    commit id: "Enable Feature B" tag: "v1.2"
    commit id: "Enable Feature C"
    commit id: "Remove flag A" tag: "v1.3"
```

---

## Example: Release Branching

```mermaid
gitGraph
    commit id: "v2.0 base" tag: "v2.0.0"
    
    branch release/2.0
    checkout release/2.0
    commit id: "2.0.1 fix"
    commit id: "2.0.2 fix" tag: "v2.0.2"
    
    checkout main
    commit id: "New feature"
    commit id: "Breaking change" tag: "v2.1.0"
    
    branch release/2.1
    checkout release/2.1
    commit id: "2.1.1 fix" tag: "v2.1.1"
    
    checkout main
    commit id: "More features"
    commit id: "Major rewrite" tag: "v3.0.0"
    
    checkout release/2.0
    commit id: "Security patch" tag: "v2.0.3"
    
    checkout release/2.1
    cherry-pick id: "Security patch"
    commit id: "Cherry-picked" tag: "v2.1.2"
```

---

## Example: Merge vs Rebase

### Merge Strategy

```mermaid
gitGraph
    commit id: "A"
    commit id: "B"
    
    branch feature
    checkout feature
    commit id: "C"
    commit id: "D"
    
    checkout main
    commit id: "E"
    commit id: "F"
    
    merge feature id: "Merge commit"
```

### Rebase Strategy

```mermaid
gitGraph
    commit id: "A"
    commit id: "B"
    commit id: "E"
    commit id: "F"
    commit id: "C' (rebased)"
    commit id: "D' (rebased)"
```

---

## Best Practices

1. **Show the happy path** — Don't include every possible branch
2. **Use meaningful IDs** — `"Add OAuth"` not `"abc123"`
3. **Tag releases** — Mark version numbers
4. **Keep it simple** — Complex graphs defeat the purpose
5. **Document your strategy** — The graph should match your CONTRIBUTING.md

---

## References

- [GitFlow](https://nvie.com/posts/a-successful-git-branching-model/) — Original GitFlow article
- [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow) — Simplified workflow
- [Trunk-Based Development](https://trunkbaseddevelopment.com/) — Alternative approach
- [Mermaid Git Graph Docs](https://mermaid.js.org/syntax/gitgraph.html) — Full syntax reference
