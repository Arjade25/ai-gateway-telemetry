# DECISIONS.md

# Architecture Decision Record (ADR)

This document records significant architectural decisions made during the development of the AI Gateway Telemetry Service.

Its purpose is to explain **why** a decision was made, not just **what** was decided.

Future changes should append new decisions rather than rewriting history.

---

# ADR-001

## Title

The MVP will be a single monolithic backend service.

## Status

Accepted

## Context

The project is in its earliest stage.

The primary objective is validating telemetry collection rather than building distributed infrastructure.

Microservices would introduce unnecessary operational complexity.

## Decision

The MVP will be implemented as a single deployable backend application.

## Consequences

Advantages

* Easier development
* Easier debugging
* Simpler deployment
* Faster iteration

Trade-offs

* Less independently scalable
* Larger codebase over time

This is acceptable for the MVP.

---

# ADR-002

## Title

PostgreSQL will be the primary database.

## Status

Accepted

## Context

The gateway stores structured telemetry records.

Relational storage provides consistency and simplicity.

## Decision

Use PostgreSQL.

Use Prisma as the ORM.

## Consequences

Advantages

* Mature ecosystem
* Strong SQL support
* Excellent Prisma integration

Trade-offs

* Requires schema migrations

Accepted.

---

# ADR-003

## Title

Telemetry is the primary product.

## Status

Accepted

## Context

Many AI gateways already exist.

The differentiator for this project is telemetry rather than request routing.

## Decision

The gateway exists to reliably collect standardized telemetry.

Routing is a means to achieve telemetry collection.

## Consequences

Future work should prioritize telemetry quality over gateway features.

---

# ADR-004

## Title

Store prompt and response.

## Status

Accepted

## Context

Telemetry is significantly more useful when prompt and response are available.

Future analytics, prompt categorization, and debugging depend on this information.

## Decision

The MVP stores:

* prompt
* response

Future versions may introduce encryption or redaction.

## Consequences

Requires careful handling of sensitive information in future versions.

---

# ADR-005

## Title

Cost is calculated by the gateway.

## Status

Accepted

## Context

Providers generally return token usage but not normalized request cost.

Cost should remain consistent across providers.

## Decision

The gateway computes cost using pricing configuration.

Clients cannot submit cost values.

## Consequences

Pricing configuration becomes part of the application.

---

# ADR-006

## Title

Pricing is configuration.

## Status

Accepted

## Context

Provider pricing changes over time.

Embedding pricing inside business logic makes maintenance difficult.

## Decision

Maintain pricing separately from application logic.

Business logic consumes pricing.

It does not define pricing.

---

# ADR-007

## Title

Support multiple providers through adapters.

## Status

Accepted

## Context

Different providers expose different SDKs and response formats.

## Decision

Each provider will implement its own adapter.

The remainder of the application works with normalized objects.

## Consequences

Adding providers should require minimal changes.

---

# ADR-008

## Title

No authentication in the MVP.

## Status

Accepted

## Context

Authentication is unrelated to validating telemetry collection.

## Decision

The MVP has no authentication.

Future versions may introduce:

* API Keys
* OAuth
* JWT

## Consequences

Development becomes significantly simpler.

---

# ADR-009

## Title

No dashboards in the MVP.

## Status

Accepted

## Context

The current objective is proving telemetry collection.

Dashboards consume telemetry.

They do not create it.

## Decision

The MVP exposes REST endpoints only.

Visualization will be developed later.

---

# ADR-010

## Title

No employee monitoring functionality.

## Status

Accepted

## Context

The long-term vision includes workforce analytics.

However, employee monitoring introduces unrelated complexity.

## Decision

The MVP focuses solely on AI request telemetry.

No screenshots.

No browser monitoring.

No activity tracking.

No keystrokes.

---

# ADR-011

## Title

REST for the MVP.

## Status

Accepted

## Context

Only one consumer currently exists.

Introducing asynchronous messaging would add complexity without solving a current problem.

## Decision

Use synchronous REST throughout the MVP.

NATS remains part of the future architecture.

## Consequences

The system is simpler to build and debug.

Future versions may publish telemetry events after persistence.

---

# ADR-012

## Title

NATS will be introduced only when multiple consumers exist.

## Status

Accepted

## Context

An event bus provides value only when multiple services react independently to the same event.

Currently there is only one responsibility:

Persist telemetry.

## Decision

NATS is intentionally excluded from the MVP.

Future architecture may publish:

telemetry.created

events after successful persistence.

Possible consumers:

* Analytics
* Dashboards
* Notifications
* Billing
* AI Categorization
* Security
* Reporting

## Consequences

The MVP avoids unnecessary distributed infrastructure while preserving a clear migration path.

---

# ADR-013

## Title

Favor simplicity over architectural purity.

## Status

Accepted

## Context

Many greenfield projects become overengineered before solving the core problem.

## Decision

Prefer explicit, readable implementations over advanced architectural patterns.

Avoid introducing abstractions until they solve a demonstrated problem.

## Consequences

The codebase should remain approachable for new contributors.

---

# Future ADRs

Future architectural changes should be recorded here.

Examples include:

* Authentication strategy
* Multi-tenancy
* Streaming responses
* Provider caching
* Rate limiting
* Event-driven architecture
* VS Code integration
* Analytics pipeline
* Dashboard architecture

No significant architectural change should be implemented without first recording the reasoning in this document.

---

# Guiding Principle

Every architectural decision should answer one question:

> Does this improve our ability to build a reliable AI Gateway Telemetry Service while keeping the MVP simple?

If the answer is no, reconsider the decision before implementing it.
