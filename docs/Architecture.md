# ARCHITECTURE.md

# AI Gateway Telemetry Architecture

Version: MVP

---

# Overview

The AI Gateway Telemetry service is a single backend application responsible for forwarding AI requests to LLM providers while capturing standardized telemetry.

It is intentionally designed as a simple monolithic service.

The architecture favors readability and maintainability over scalability.

The gateway is the only component responsible for interacting with external AI providers.

---

# High-Level Architecture

```text
                Client
                   │
                   │ HTTP
                   ▼
        ┌──────────────────────┐
        │    AI Gateway API    │
        └──────────┬───────────┘
                   │
         Validate Request
                   │
                   ▼
        ┌──────────────────────┐
        │  Gateway Service     │
        └──────────┬───────────┘
                   │
      Select Provider Adapter
                   │
                   ▼
        ┌──────────────────────┐
        │ Provider Adapter      │
        └──────────┬───────────┘
                   │
                   ▼
          Claude / GPT / Gemini
                   │
             AI Response
                   │
                   ▼
        ┌──────────────────────┐
        │ Telemetry Builder    │
        └──────────┬───────────┘
                   │
          Calculate Cost
                   │
                   ▼
        ┌──────────────────────┐
        │ Persistence Layer    │
        └──────────┬───────────┘
                   │
                   ▼
             PostgreSQL
                   │
                   ▼
         Return AI Response
```

---

# Request Lifecycle

Every request follows exactly the same pipeline.

```
Client

↓

POST /chat

↓

Validate Request

↓

Gateway Service

↓

Provider Adapter

↓

External LLM Provider

↓

Receive Response

↓

Measure Latency

↓

Extract Usage Metadata

↓

Calculate Cost

↓

Persist Telemetry

↓

Return Original Response
```

The telemetry collection process is synchronous.

No background workers are involved.

---

# Core Components

## API Layer

Responsibilities

* Accept HTTP requests
* Validate request payloads
* Return HTTP responses

Should not contain business logic.

---

## Gateway Service

Responsibilities

* Orchestrate the request lifecycle
* Call provider adapters
* Measure latency
* Invoke telemetry builder
* Persist telemetry
* Return AI response

This is the heart of the application.

---

## Provider Adapter

Responsibilities

* Communicate with one LLM provider
* Convert gateway request into provider request
* Convert provider response into a normalized format

Each provider has its own adapter.

Examples

```
AnthropicAdapter

OpenAIAdapter

GeminiAdapter
```

Only one provider is required for MVP.

---

## Telemetry Builder

Responsibilities

Convert provider responses into a common telemetry model.

Input

Provider response

Output

```
TelemetryEvent
```

Every provider should produce the same output structure.

---

## Cost Calculator

Responsibilities

* Load pricing configuration
* Calculate request cost
* Return computed value

Business logic should not know provider pricing.

Pricing is configuration.

---

## Persistence Layer

Responsibilities

Persist telemetry records.

No provider-specific logic should exist here.

---

# Normalized Flow

Regardless of provider, the application should work with a common internal model.

```
Client Request

↓

Gateway Request

↓

Provider Adapter

↓

Provider Response

↓

Telemetry Event

↓

Database
```

No other layer should understand provider-specific response formats.

---

# Provider Isolation

Provider SDKs should only exist inside provider adapters.

Incorrect

```
Gateway Service

↓

Anthropic SDK
```

Correct

```
Gateway Service

↓

Provider Interface

↓

Anthropic Adapter

↓

Anthropic SDK
```

This keeps the rest of the application provider agnostic.

---

# Cost Flow

```
Provider Response

↓

Input Tokens

↓

Output Tokens

↓

Pricing Configuration

↓

Cost Calculator

↓

Computed Cost
```

Cost is derived data.

It is never provided directly by providers.

---

# Data Flow

```
Prompt

↓

Provider

↓

Response

↓

Telemetry

↓

Database

↓

Response to Client
```

The response returned to the client should remain independent of telemetry persistence.

---

# Error Flow

Possible failures

```
Validation Error

↓

400
```

---

```
Unsupported Provider

↓

400
```

---

```
Provider Failure

↓

502
```

---

```
Database Failure

↓

500
```

Every failure should return a meaningful error.

Silent failures are not acceptable.

---

# Folder Responsibilities

The project should separate responsibilities rather than technologies.

Example

```
controllers/

Receives HTTP requests.

------------------------

services/

Coordinates request flow.

------------------------

providers/

Contains provider adapters.

------------------------

telemetry/

Builds normalized telemetry.

------------------------

pricing/

Calculates request cost.

------------------------

database/

Persistence logic.

------------------------

config/

Application configuration.
```

---

# Architectural Principles

## Single Responsibility

Every module should have one purpose.

---

## Explicit Dependencies

Avoid hidden behavior.

Dependencies should be obvious.

---

## Provider Agnostic

Changing providers should not require changes throughout the application.

---

## Configuration Driven

Provider pricing should live in configuration.

Never inside business logic.

---

## Simple over Clever

Favor readable code over advanced abstractions.

---

# Future Extension Points

The architecture intentionally leaves room for future capabilities.

Possible additions

```
Authentication

↓

Rate Limiting

↓

Caching

↓

Streaming

↓

Analytics

↓

Dashboards

↓

NATS

↓

Queues

↓

VS Code Extension
```

These should be added by extending the architecture rather than redesigning it.

---

# Deliberate Non-Goals

The architecture intentionally excludes

* Microservices
* Event buses
* CQRS
* Event sourcing
* Domain-driven design
* Repository factories
* Background workers
* Distributed transactions
* Redis
* Kafka
* Kubernetes

The MVP is a single deployable application.

---

# Guiding Principle

The architecture exists for one reason:

> Accept an AI request, forward it, capture reliable telemetry, persist it, and return the response.

Every component in the system should contribute directly to that goal.

If a proposed component does not improve telemetry collection or maintainability, it should not be added to the MVP.
