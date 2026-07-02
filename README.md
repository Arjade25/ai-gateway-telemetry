# PROJECT.md

# AI Gateway Telemetry

> Collect accurate LLM telemetry. Nothing more.

---

# Vision

Modern engineering teams are rapidly adopting Large Language Models (LLMs) such as Claude, GPT, Gemini, and other AI assistants.

Organizations increasingly want visibility into:

* Which models are being used
* How many tokens are consumed
* How much AI usage costs
* How AI adoption changes over time

Today, this information is often scattered across multiple providers and dashboards.

This project aims to build a **provider-agnostic AI Gateway** that sits between applications and LLM providers, capturing standardized telemetry for every request.

The gateway itself is the product.

Everything else—dashboards, analytics, employee monitoring, engineering insights—will be built on top of this foundation in future iterations.

---

# Problem Statement

Applications communicate directly with AI providers.

```
Application
     │
     ▼
Claude / OpenAI / Gemini
```

As a result:

* token usage is fragmented
* cost is difficult to calculate
* provider usage is inconsistent
* telemetry formats differ
* analytics become difficult

Instead, requests should flow through a centralized gateway.

```
Application
      │
      ▼
AI Gateway
      │
      ▼
LLM Provider
```

This allows every request to generate a standardized telemetry record.

---

# Primary Goal

The gateway has exactly one responsibility:

> Capture reliable telemetry for every AI request.

A successful request should produce:

* prompt
* response
* provider
* model
* input tokens
* output tokens
* total tokens
* latency
* timestamp
* calculated cost

The gateway should return the original provider response to the client after recording telemetry.

---

# Product Philosophy

The project values simplicity over completeness.

If a feature does not directly contribute to collecting AI telemetry, it does not belong in the MVP.

The objective is to build a dependable foundation rather than a feature-rich platform.

---

# MVP Definition

The Minimum Viable Product consists of:

* HTTP API
* Provider integration
* Telemetry extraction
* Cost calculation
* Persistent storage
* Telemetry retrieval endpoints

Nothing else.

---

# Out of Scope

The following features are intentionally excluded from the MVP.

## Authentication

* User accounts
* Login
* JWT
* OAuth
* RBAC

## Organizations

* Multi-tenancy
* Teams
* Departments
* Employee hierarchy

## Analytics

* Dashboards
* Charts
* Reports
* Productivity scoring

## Employee Monitoring

* Screenshots
* Application monitoring
* Website monitoring
* Browser history
* Keystroke logging
* Mouse tracking
* Attendance

## AI Enhancements

* Prompt categorization
* Prompt quality scoring
* Prompt sanitization
* Secret detection
* AI recommendations
* Knowledge gap analysis

## Infrastructure

* Microservices
* NATS
* Redis
* Kafka
* BullMQ
* Kubernetes
* Event buses
* WebSockets

The MVP should remain a single backend service.

---

# Success Criteria

The MVP is considered successful if:

1. A client sends a prompt.
2. The gateway forwards the request.
3. The provider returns a response.
4. The gateway extracts provider metadata.
5. The gateway computes request cost.
6. The telemetry is persisted.
7. The original response is returned to the client.

Every successful request should produce exactly one telemetry record.

---

# Future Vision

Once reliable telemetry collection has been validated, future iterations may introduce:

* AI dashboards
* Usage analytics
* Cost analytics
* Team-level reporting
* Multi-tenancy
* VS Code integration
* CLI integration
* Browser extension
* AI workforce analytics
* Engineering productivity insights
* Employee monitoring integrations

These are future products built on top of the gateway.

They are **not** responsibilities of the gateway itself.

---

# Design Principles

Every design decision should satisfy the following principles.

## Simplicity

Prefer the simplest implementation that satisfies the requirements.

Avoid unnecessary abstractions.

---

## Extensibility

Adding a new provider should require minimal changes.

Provider-specific logic should remain isolated.

---

## Reliability

Every successful request must produce a telemetry record.

Telemetry collection is the primary responsibility of the system.

---

## Provider Agnostic

The gateway should avoid assumptions about any specific LLM provider.

Support for additional providers should be straightforward.

---

## Maintainability

Readable code is preferred over clever code.

Small modules and explicit logic are favored over heavy abstraction.

---

# Guiding Question

Before implementing any feature, ask:

> Does this improve the gateway's ability to reliably collect LLM telemetry?

If the answer is **no**, it should not be implemented in the MVP.
