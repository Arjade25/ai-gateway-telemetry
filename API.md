# API.md

# REST API Specification

Version: MVP

---

# Overview

The AI Gateway Telemetry Service exposes a minimal REST API.

The API has two responsibilities:

1. Accept AI requests.
2. Retrieve recorded telemetry.

No other functionality is provided.

---

# Base URL

Example

```
http://localhost:3000
```

---

# Endpoints

| Method | Endpoint     | Purpose                            |
| ------ | ------------ | ---------------------------------- |
| POST   | /chat        | Submit an AI request               |
| GET    | /events      | Retrieve all telemetry             |
| GET    | /events/{id} | Retrieve a single telemetry record |

---

# POST /chat

## Description

Submits a prompt to the configured AI provider.

The gateway forwards the request, captures telemetry, persists it, and returns the provider response.

---

## Request Body

```json
{
  "provider": "anthropic",
  "model": "claude-sonnet-4",
  "prompt": "Explain Prisma relationships."
}
```

---

## Field Definitions

### provider

Required

Type

String

Examples

* anthropic
* openai
* google

---

### model

Required

Type

String

Example

```
claude-sonnet-4
```

---

### prompt

Required

Type

String

The text submitted to the model.

---

## Success Response

HTTP

```
200 OK
```

Example

```json
{
  "provider": "anthropic",
  "model": "claude-sonnet-4",
  "response": "Prisma relationships define..."
}
```

The response intentionally contains only what the client needs.

Telemetry is stored internally.

---

# Possible Errors

## Invalid Request

```
400 Bad Request
```

---

## Unsupported Provider

```
400 Bad Request
```

---

## Unsupported Model

```
400 Bad Request
```

---

## Provider Error

```
502 Bad Gateway
```

---

## Internal Error

```
500 Internal Server Error
```

---

# GET /events

Returns every telemetry record currently stored.

---

## Success Response

```
200 OK
```

Example

```json
[
  {
    "id": "...",
    "provider": "anthropic",
    "model": "claude-sonnet-4",
    "inputTokens": 182,
    "outputTokens": 611,
    "totalTokens": 793,
    "cost": 0.0134,
    "latencyMs": 1680,
    "createdAt": "2026-07-02T11:18:53Z"
  }
]
```

For the MVP, no pagination or filtering is required.

---

# GET /events/{id}

Returns one telemetry record.

---

## Success Response

```json
{
  "id": "...",
  "provider": "anthropic",
  "model": "claude-sonnet-4",
  "prompt": "...",
  "response": "...",
  "inputTokens": 182,
  "outputTokens": 611,
  "totalTokens": 793,
  "latencyMs": 1680,
  "cost": 0.0134,
  "finishReason": "stop",
  "providerRequestId": "...",
  "createdAt": "2026-07-02T11:18:53Z"
}
```

---

# Versioning

The MVP uses a single API version.

Future versions should use URI versioning.

Example

```
/v1/chat
```

---

# Authentication

Authentication is intentionally excluded from the MVP.

Future versions may introduce API keys or OAuth.

---

# Idempotency

Not required for MVP.

Future versions may support idempotency keys for request replay protection.

---

# Rate Limiting

Not required for MVP.

---

# Streaming

Not supported.

Every request waits for the provider response before returning.

---

# Pagination

Not required.

---

# Filtering

Not required.

---

# Sorting

Not required.

---

# Future Endpoints

These endpoints are intentionally excluded from the MVP.

```
POST /embeddings

POST /images

POST /audio

POST /responses

POST /agents

GET /providers

GET /models

GET /pricing

GET /metrics

GET /health
```

These should only be introduced after the telemetry gateway has been validated.

---

# Design Philosophy

The API should remain extremely small.

Every endpoint should exist because it directly contributes to one of two goals:

1. Submit an AI request.

2. Retrieve telemetry.

Anything outside those responsibilities belongs in a future version.
