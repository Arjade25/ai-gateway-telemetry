# ROADMAP.md

# AI Gateway Telemetry Service Roadmap

This document defines the planned evolution of the project.

The roadmap is intentionally incremental.

Each phase should produce a working, deployable application before moving to the next.

The project should never sacrifice simplicity in pursuit of future functionality.

---

# Guiding Principle

Build only what is necessary for the current phase.

Future phases should never influence the implementation of the current phase unless doing so significantly improves maintainability.

---

# Phase 1 — Telemetry Gateway MVP

## Goal

Build a reliable AI Gateway capable of collecting standardized telemetry for every LLM request.

This phase validates the core hypothesis:

> Can we reliably collect provider-agnostic AI telemetry?

---

## Features

### Gateway

* HTTP API
* Provider abstraction
* Single provider implementation
* Request forwarding
* Response forwarding

---

### Telemetry

Capture:

* provider
* model
* prompt
* response
* input tokens
* output tokens
* total tokens
* latency
* finish reason
* provider request ID
* timestamp
* cost

---

### Persistence

* PostgreSQL
* Prisma
* Single telemetry table

---

### API

* POST /chat
* GET /events
* GET /events/{id}

---

## Out of Scope

* Authentication
* Teams
* Organizations
* Dashboards
* Analytics
* Employee monitoring
* VS Code extension
* Browser extension
* CLI
* Streaming
* Redis
* NATS

---

## Exit Criteria

The phase is complete when:

* AI requests can be submitted.
* Telemetry is persisted.
* Cost is calculated.
* Events can be retrieved.
* The service is stable.

---

# Phase 2 — Gateway Improvements

## Goal

Improve the robustness of the gateway without expanding into analytics.

---

## Candidate Features

* Multiple providers
* Configuration improvements
* Better validation
* Provider health checks
* Retry policies
* Structured logging
* Request tracing
* API versioning
* Better error handling

---

## Exit Criteria

The gateway reliably supports multiple providers with a common interface.

---

# Phase 3 — Developer Experience

## Goal

Make the gateway easier to integrate into applications.

---

## Candidate Features

* Authentication
* API keys
* SDK
* CLI
* OpenAPI documentation
* Docker image
* Health endpoints
* Metrics endpoint

---

## Exit Criteria

Developers can integrate the gateway with minimal effort.

---

# Phase 4 — Telemetry Platform

## Goal

Begin transforming telemetry into useful information.

---

## Candidate Features

* Filtering
* Pagination
* Search
* Aggregation
* Usage statistics
* Cost statistics
* Model statistics
* Provider statistics

---

## Exit Criteria

Consumers can explore telemetry efficiently.

---

# Phase 5 — Dashboard

## Goal

Visualize telemetry.

---

## Candidate Features

* Web dashboard
* Usage graphs
* Cost graphs
* Model usage
* Provider usage
* Daily trends
* Monthly trends

---

## Exit Criteria

Telemetry can be understood visually.

---

# Phase 6 — AI Insights

## Goal

Enrich telemetry using AI.

---

## Candidate Features

* Prompt categorization
* Programming language detection
* Framework detection
* Prompt summarization
* Session detection
* Knowledge gap analysis
* Secret detection
* Prompt sanitization

---

## Exit Criteria

Telemetry becomes actionable intelligence.

---

# Phase 7 — Integrations

## Goal

Collect telemetry from developer workflows.

---

## Candidate Features

* VS Code extension
* JetBrains plugin
* CLI integration
* Browser extension
* GitHub integration
* GitLab integration

---

## Exit Criteria

Developers can use the gateway from their preferred tools.

---

# Phase 8 — Workforce Analytics

## Goal

Expand from telemetry to organizational insights.

---

## Candidate Features

* Organizations
* Teams
* Users
* Multi-tenancy
* Role-based access
* Team analytics
* AI adoption metrics
* Cost allocation
* Department reporting

---

## Exit Criteria

Organizations can understand AI adoption across teams.

---

# Phase 9 — Event-Driven Architecture

## Goal

Scale the platform as independent services emerge.

---

## Candidate Features

* NATS
* Event publishing
* Analytics service
* Notification service
* Billing service
* Background processing

Example Event

```text id="6zkkgt"
telemetry.created
```

Possible Consumers

* Analytics
* Dashboard
* Billing
* Notifications
* AI Categorization
* Security
* Reporting

---

## Exit Criteria

The gateway publishes events without knowing which services consume them.

---

# Phase 10 — Enterprise Platform

## Goal

Provide enterprise-grade operational capabilities.

---

## Candidate Features

* Audit logging
* SSO
* OAuth
* SCIM
* Enterprise RBAC
* Budget management
* Quotas
* Policy engine
* Compliance features
* Data retention policies

---

# Never Build Early

The following technologies should only be introduced when there is a demonstrated need.

* Redis
* NATS
* Kafka
* Kubernetes
* Microservices
* CQRS
* Event sourcing
* Background workers
* Distributed caching

Complexity should always follow demonstrated requirements.

---

# Success Metric

The project succeeds if every phase leaves the system in a deployable, maintainable, and well-documented state.

No phase should leave the repository partially complete.

---

# Guiding Principle

Every new feature should answer two questions:

1. Which roadmap phase does this belong to?

2. Does implementing it now help validate the current phase?

If the answer to the second question is "no," defer the feature until its appropriate phase.
