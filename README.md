# AI Gateway Telemetry Service

> A provider-agnostic gateway that captures standardized telemetry for every Large Language Model (LLM) request.

---

## Overview

AI Gateway Telemetry Service is a lightweight backend that sits between client applications and LLM providers such as Anthropic, OpenAI, and Google.

Every request passing through the gateway generates a standardized telemetry record containing metadata such as:

* Provider
* Model
* Prompt
* Response
* Input Tokens
* Output Tokens
* Total Tokens
* Request Cost
* Latency
* Timestamp

The gateway is intentionally focused on **telemetry collection**.

It is **not** an analytics platform, employee monitoring solution, or AI assistant.

Those capabilities may be built on top of the gateway in future iterations.

---

## Why This Exists

Every AI provider exposes different SDKs, response formats, and usage information.

This makes it difficult to answer questions like:

* Which models are being used?
* How many tokens are consumed?
* How much do AI requests cost?
* Which providers are most heavily used?

Instead of every application integrating directly with providers, they communicate with this gateway.

```text
Client

↓

AI Gateway Telemetry Service

↓

LLM Provider
```

The gateway standardizes telemetry regardless of provider.

---

## Current Scope (MVP)

The MVP focuses on one responsibility:

> Capture reliable telemetry for every AI request.

Implemented features:

* HTTP API
* Provider abstraction
* Request forwarding
* Usage extraction
* Cost calculation
* Telemetry persistence
* Telemetry retrieval

---

## Out of Scope

The MVP intentionally excludes:

* Authentication
* Multi-tenancy
* Dashboards
* Analytics
* Employee monitoring
* Screenshots
* Browser tracking
* Notifications
* NATS
* Redis
* Kubernetes
* Microservices

The project should remain intentionally small.

---

## Planned Architecture

```text
Client
   │
   ▼
AI Gateway
   │
   ▼
LLM Provider
   │
   ▼
Telemetry
   │
   ▼
PostgreSQL
```

Future versions may introduce event-driven processing, analytics, dashboards, and integrations.

---

## Technology Stack

Backend

* NestJS
* TypeScript

Database

* PostgreSQL
* Prisma

Package Manager

* pnpm

Runtime

* Node.js 22

---

## Documentation

Detailed documentation can be found in the `docs/` directory.

| Document        | Purpose                       |
| --------------- | ----------------------------- |
| PROJECT.md      | Product vision and scope      |
| REQUIREMENTS.md | Functional requirements       |
| ARCHITECTURE.md | System architecture           |
| DATABASE.md     | Database design               |
| API.md          | REST API specification        |
| DECISIONS.md    | Architecture Decision Records |
| ROADMAP.md      | Planned project evolution     |
| CLAUDE.md       | Instructions for Claude Code  |

---

## Project Philosophy

The project follows one guiding principle:

> Every successful AI request should produce one reliable telemetry record.

Everything else is secondary.

---

## Development Status

Current Phase

**Phase 1 — Telemetry Gateway MVP**

See `docs/ROADMAP.md` for future phases.

---

## Contributing

Before contributing:

1. Read the documentation.
2. Understand the current roadmap phase.
3. Review architectural decisions.
4. Keep the implementation aligned with the MVP.

See `CONTRIBUTING.md` for detailed guidelines.

---

## License

TBD
