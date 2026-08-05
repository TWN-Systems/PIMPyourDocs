# PIMPyourDocs Specification

**Version:** 1.0.0  
**Status:** Living Document

---

## 1. Document Structure

### 1.1 File Naming

```
kebab-case-names.md
```

- Lowercase only
- Hyphens for spaces
- `.md` extension always
- No spaces, no underscores, no special characters

**Why:** Maximum filesystem compatibility. Works on case-sensitive and case-insensitive systems. URL-safe.

### 1.2 Directory Structure

```
docs/
├── index.md              # Entry point
├── architecture/         # System design & decisions
│   ├── index.md
│   └── decisions/        # ADRs live here
│       └── 0001-use-postgres.md
├── services/             # Per-service documentation
│   └── {service-name}/
│       ├── index.md
│       ├── api.md
│       └── runbook.md
├── operations/           # Operational documentation
│   ├── runbooks/
│   └── incidents/
└── guides/               # How-to guides
    └── ...
```

### 1.3 Required Front Matter

Every document MUST begin with a YAML front matter block:

```yaml
---
title: Document Title
status: draft | review | published | deprecated
owner: team-name or @username
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2]
---
```

**Why:** Machine-parseable metadata enables automation, search, and staleness detection.

---

## 2. Spec as Code

### 2.1 The Principle

Documentation describes the *intended state*—the specification—not the current state.

```
❌ "The service currently uses PostgreSQL 14"
✅ "The service MUST use PostgreSQL 14.x"
```

### 2.2 RFC 2119 Keywords

Use RFC 2119 keywords for requirements:

| Keyword | Meaning |
|---------|---------|
| MUST | Absolute requirement |
| MUST NOT | Absolute prohibition |
| SHOULD | Recommended, but exceptions exist |
| SHOULD NOT | Not recommended, but exceptions exist |
| MAY | Optional |

**Example:**

```markdown
## Authentication Requirements

- Clients MUST authenticate using OAuth 2.0
- Tokens SHOULD be refreshed before expiry
- Clients MAY cache tokens for up to 1 hour
```

### 2.3 Testable Documentation

Where possible, documentation should be testable:

```markdown
## API Contract

The `/health` endpoint MUST return:
- HTTP 200 when healthy
- Response time under 100ms
- JSON body: `{"status": "healthy"}`

<!-- test:api GET /health -->
```

These markers can be parsed by test harnesses to validate reality against spec.

### 2.4 Version Binding

Specifications should be version-bound:

```markdown
## Dependencies

| Component | Version | Constraint |
|-----------|---------|------------|
| PostgreSQL | 14.x | MUST match major version |
| Redis | 7.x | SHOULD use latest patch |
| Node.js | 20.x | MUST use LTS |
```

---

## 3. Diagrams as Code

### 3.1 Mermaid Only

All diagrams MUST be Mermaid. No external image files for architectural diagrams.

**Why:**

- Version controlled with the doc
- Diff-able
- No binary blobs in git
- Renders anywhere Mermaid is supported (GitHub, GitLab, VS Code, Obsidian, etc.)

### 3.2 Supported Diagram Types

```mermaid
mindmap
  root((PIMPyourDocs Diagrams))
    Flowcharts
      Process flows
      Decision trees
    Sequence Diagrams
      API interactions
      System flows
    Entity Relationship
      Data models
      Schema design
    State Diagrams
      Lifecycle management
      Status flows
    Architecture
      C4 Context
      C4 Container
      Deployment
```

### 3.3 Diagram Standards

Every diagram MUST have:

1. **A title** (using `---` config or comment)
2. **A legend** if symbols aren't self-explanatory
3. **Consistent styling** per type

**Example:**

```mermaid
---
title: Payment Service Context
---
flowchart TB
    subgraph External
        U[User]
        PS[Payment Provider]
    end
    
    subgraph Internal
        API[API Gateway]
        PAY[Payment Service]
        DB[(PostgreSQL)]
    end
    
    U --> API
    API --> PAY
    PAY --> PS
    PAY --> DB
```

**Source for above diagram:**

```text
flowchart TB
    subgraph External
        U[User]
        PS[Payment Provider]
    end
    
    subgraph Internal
        API[API Gateway]
        PAY[Payment Service]
        DB[(PostgreSQL)]
    end
    
    U --> API
    API --> PAY
    PAY --> PS
    PAY --> DB
```

### 3.4 C4 Model Support

For architecture diagrams, use C4 model conventions:

```mermaid
flowchart TB
    subgraph legend[Legend]
        direction LR
        P[Person]
        S[System]
        C[Container]
        CO[Component]
    end
```

---

## 4. Linking & References

### 4.1 Internal Links

Use relative links only:

```markdown
✅ See [API documentation](./api.md)
✅ See [Architecture Decision](../architecture/decisions/0001-use-postgres.md)
❌ See [API documentation](/docs/services/payment/api.md)
❌ See [API documentation](https://wiki.company.com/docs/api)
```

**Why:** Relative links work regardless of where the docs are hosted.

### 4.2 Anchor Links

Use explicit anchors for section linking:

```markdown
## Authentication {#auth}

...

See [Authentication](#auth) for details.
```

### 4.3 External References

External references should use reference-style links:

```markdown
See the [PostgreSQL documentation][pg-docs] for details.

[pg-docs]: https://www.postgresql.org/docs/14/
```

**Why:** Centralizes external URLs for easier maintenance.

---

## 5. Code Examples

### 5.1 Language Tags

Always specify the language:

```python
def example():
    pass
```

Use the triple backtick syntax with language identifier: \`\`\`python

### 5.2 Runnable Examples

Prefer complete, runnable examples over snippets:

**❌ Bad — incomplete snippet:**

```python
response = client.get("/api/users")
```

**✅ Good — complete, runnable:**

```python
import httpx

client = httpx.Client(base_url="https://api.example.com")
response = client.get("/api/users")
print(response.json())
```

### 5.3 Configuration Examples

Configuration examples should be copy-pasteable with comments:

```yaml
# config.yaml
database:
  host: localhost      # MUST be resolvable hostname
  port: 5432           # Default PostgreSQL port
  name: myapp          # Database name
  pool_size: 10        # SHOULD be 2x CPU cores
```

---

## 6. Templates

### 6.1 Architecture Decision Record (ADR)

See [templates/ADR.md](./templates/ADR.md)

### 6.2 Service Documentation

See [templates/SERVICE.md](./templates/SERVICE.md)

### 6.3 Runbook

See [templates/RUNBOOK.md](./templates/RUNBOOK.md)

### 6.4 API Documentation

See [templates/API.md](./templates/API.md)

### 6.5 Incident Post-Mortem

See [templates/INCIDENT.md](./templates/INCIDENT.md)

---

## 7. Automation Hooks

### 7.1 Staleness Detection

Documents with `updated` front matter older than threshold trigger warnings:

```yaml
# .pimpyourdocs.yaml
staleness:
  warning_days: 90
  error_days: 180
  exclude:
    - "architecture/decisions/*"  # ADRs are immutable
```

### 7.2 Link Checking

All internal links MUST be validated in CI:

```yaml
# Example GitHub Action
- name: Check links
  uses: lycheeverse/lychee-action@v1
  with:
    args: --offline docs/
```

### 7.3 Diagram Validation

Mermaid diagrams should be validated:

```bash
npx @mermaid-js/mermaid-cli -i docs/ --validate
```

---

## 8. Rendering

### 8.1 Supported Renderers

PIMPyourDocs is designed to render correctly in:

- GitHub/GitLab native rendering
- VS Code with Markdown Preview
- Obsidian
- MkDocs / Material for MkDocs
- Docusaurus
- Any CommonMark-compliant renderer with Mermaid support

### 8.2 Local Rendering

For local preview:

```bash
# Simple option
npx serve docs/

# With Mermaid support
npx @mermaid-js/mermaid-cli serve docs/

# Full static site
mkdocs serve
```

### 8.3 No Renderer Lock-in

Documents MUST be readable and useful in raw form. Don't rely on renderer-specific features.

---

## 9. Migration Paths

### 9.1 From Confluence

```bash
# Export space as HTML
# Convert with pandoc
pandoc -f html -t markdown confluence-export.html -o output.md
```

### 9.2 From Notion

Use notion-to-md or official export → markdown.

### 9.3 From Google Docs

Export as .docx, convert with pandoc:

```bash
pandoc -f docx -t markdown document.docx -o output.md
```

---

## 10. Compliance Checklist

Before publishing, every document should pass:

- [ ] Has valid front matter
- [ ] Uses RFC 2119 keywords for requirements
- [ ] All diagrams are Mermaid (no image files)
- [ ] All links are relative
- [ ] Code blocks have language tags
- [ ] Readable in raw form
- [ ] Renders correctly in at least 2 different viewers

---

## 11. Language & Writing Standards (ASD-STE100)

Every document in this repo is written in **ASD-STE100 Simplified Technical English**
(Issue 9, 2025-01-15) — the controlled language aerospace uses so a tired reader cannot
misread an instruction. STE is a near-perfect negative of AI writing tell: long sentences,
synonym rotation, hedges, and filler. This section is the standard, paraphrased for software
docs; the official standard is a free download at asd-ste100.org.

### 11.1 Classify Each Document

Every document is either **procedural** (tells the reader what to do) or **descriptive**
(explains what a thing is). The classification drives the sentence limit.

| Template | Classification |
|---|---|
| RUNBOOK, INCIDENT | procedural |
| ADR, SERVICE, API | descriptive |

Do not mix the two in one passage. A note inside a procedure is descriptive.

### 11.2 The Rules

**Sentences (5.1, 6.3):** maximum **20 words** per sentence in procedural text, **25** in
descriptive. Code spans, identifiers, numbers with units, and quoted text count as ONE word
(rule 8.6). No sentence over the limit, ever.

**Verbs (3.1–3.7):** simple tenses only — no present perfect ("has been updated" → "we
updated"), no "-ing" verb clauses after a comma ("..., making it easy to..."). Active voice.
One instruction per sentence (5.2). A required condition comes BEFORE its command (5.4):
"If the build fails, read the log" — never "...read the log if the build fails".

**Modals (3.7):** approved: **can, will, must**. Banned: **should, would, may, might, could**.
- "should" as a requirement → "must"; as a suggestion → delete it or state it as fact.
- "may/might/could" (possibility) → "can".
- Exception: uppercase RFC 2119 keywords in requirement lists are legal terms of art.
  The prose around them obeys STE.

**Punctuation (8.1, 4.2, GR-6):** no semicolons — write two sentences. No contractions
("don't" → "do not"). No Latin abbreviations: "e.g." → "for example", "i.e." → "that is",
"etc." → name the items.

**Vocabulary (1.1–1.14):** one word, one meaning, one part of speech. Collapse rotations:

| Pick one | Instead of |
|---|---|
| check | verify / confirm / validate / ensure |
| configuration | settings / options |
| delete | remove / drop / destroy |
| error | issue / problem / failure |

**Slop words (banned):** leverage, utilize, seamlessly, effortlessly, robust, comprehensive,
performant, facilitate, streamline, delve, plethora, myriad, crucial, pivotal, and "it is
worth noting that". Delete them or give the measurable fact.

**Untouchables (1.5, 8.6):** code, identifiers, CLI commands, flags, file paths, quoted
error messages, and product names are exact — even when they break vocabulary rules.

### 11.3 The Self-Check (MUST pass before publishing)

Documentation is testable (section 2.3). Run the checker before every publish:

```
stefold lint --type <procedural|descriptive> <file>
```

The four manual checks, when the checker is unavailable:

1. Count words in the three longest sentences. Over 20/25 → split.
2. Search for: `'ll` `'re` `'s` contractions, `has been` / `have been`, `should`,
   `-ing` verbs after a comma, semicolons.
3. Every `if` and `when` stands at the START of its sentence, before the command.
4. Search for the verbs you did not pick in your consistency pass. Replace every hit.

`stefold score` adds the prose metric: countlm bits/word. STE text is low-surprisal;
a rising score is a style-drift warning, not a verdict.

`stefold` is a streaming single-pass linter on the fold substrate (hashtrinity rule
tables, countlm scoring, autofold trace). It ships with the skill's self-test fixtures.

---

*This specification is itself a PIMPyourDocs document.*
