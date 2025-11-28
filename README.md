# PIMPyourDocs

**Practical, Independent Markdown Practices**

> Documentation that belongs to you. Forever.

---

## The Problem

Your documentation is trapped. Locked in proprietary formats. Held hostage by SaaS platforms. Rendered useless the moment you stop paying rent on your own knowledge.

Confluence exports to garbage. Notion locks your data behind their renderer. Google Docs exports to formats nobody asked for. And when these companies pivot, get acquired, or shut down? Your institutional knowledge goes with them.

**This is not acceptable.**

---

## The Philosophy

PIMPyourDocs is built on three fundamental truths:

### 1. Plain Text Wins

Every proprietary format eventually becomes a liability. Every "rich" editor becomes a prison. But plain text? Plain text is forever.

Markdown has won. It's readable without rendering. It's writable without tooling. It's portable across every platform that matters. It's the closest thing we have to a universal documentation standard.

### 2. Spec as Code, Not State as Documentation

Traditional documentation captures *what is*. It's a snapshot. A photograph of a moment that becomes a lie the instant something changes.

**PIMPyourDocs documents *what should be*.** 

Your documentation is the specification. The source of truth. The contract. If reality doesn't match your docs, reality is wrong—and your CI pipeline should catch it.

### 3. Own Your Knowledge

If you can't:
- Version control it
- Render it locally
- Move it between platforms
- Read it in a text editor

...then you don't own it. You're renting it. And rent always goes up.

---

## Why Now? The AI Imperative

Here's the thing about AI: **everything becomes plain text eventually.**

When you feed your docs to an LLM, what happens?
- Your beautiful Confluence formatting? Stripped.
- Your interactive Notion databases? Flattened.
- Your carefully styled Google Docs? Converted to plain text.

AI doesn't care about your fancy rendering. AI cares about *semantic content*. And Markdown is the closest thing to semantic plain text we have.

If your documentation is going to end up as plain text anyway—for RAG pipelines, for AI assistants, for automated analysis—why the fuck would you start anywhere else?

Write for the destination format. Write in Markdown.

---

## Core Principles

### Portability First

```
✅ Git-versionable
✅ Platform-agnostic
✅ Renders in any Markdown viewer
✅ Readable raw
```

### Diagrams as Code

All diagrams are Mermaid. No PNGs locked in someone's cloud. No Visio files from 2015. Code that renders to diagrams, versioned alongside your docs.

### Privacy by Default

Client-side rendering. No telemetry. No "cloud sync" of your sensitive documentation. Your docs, your machine, your business.

### Minimal Viable Structure

Not a 47-page style guide. Not a template empire. The minimum structure needed to be useful, discoverable, and maintainable.

---

## Quick Start

```bash
# Clone the framework
git clone https://github.com/yourorg/pimpyourdocs.git docs/

# Or just copy the structure
cp -r pimpyourdocs/templates/ your-project/docs/
```

See [SPEC.md](./SPEC.md) for the full specification.

See [templates/](./templates/) for ready-to-use templates.

---

## What's Included

```
pimpyourdocs/
├── README.md           # You are here
├── SPEC.md             # The full specification
├── PRINCIPLES.md       # Deep dive on philosophy
├── templates/
│   ├── ADR.md          # Architecture Decision Records
│   ├── RUNBOOK.md      # Operational runbooks
│   ├── SERVICE.md      # Service documentation
│   ├── API.md          # API documentation
│   └── INCIDENT.md     # Incident post-mortems
├── examples/
│   └── ...             # Real-world examples
└── diagrams/
    └── MERMAID.md      # Mermaid diagram standards
```

---

## Escape Your Current Platform

- [Escaping Confluence](./migrations/confluence.md)
- [Escaping Notion](./migrations/notion.md)
- [Escaping Google Docs](./migrations/google-docs.md)

---

## License

MIT. Because documentation practices shouldn't be proprietary either.

---

*PIMPyourDocs: Because your documentation should outlive your vendors.*
