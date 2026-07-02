# CLAUDE.md

# Claude Code Instructions

This repository contains the AI Gateway Telemetry Service.

Read this document before making any architectural or implementation decisions.

---

# Read Order

Before making any changes, read the project documentation in the following order:

1. PROJECT.md
2. REQUIREMENTS.md
3. ARCHITECTURE.md
4. DATABASE.md
5. API.md
6. DECISIONS.md
7. ROADMAP.md

Do not begin implementation until the project goals and constraints are understood.

---

# Project Goal

The purpose of this repository is to build a reliable AI Gateway Telemetry Service.

The gateway should:

* accept AI requests
* forward them to an LLM provider
* capture standardized telemetry
* calculate cost
* persist telemetry
* return the provider response

Nothing more.

---

# Current Phase

Always determine the current roadmap phase before implementing features.

The current development phase is:

**Phase 1 — Telemetry Gateway MVP**

Do not implement features from later phases unless explicitly instructed.

---

# Engineering Philosophy

Prefer:

* simple code
* explicit code
* readable code
* maintainable code
* predictable behavior

Avoid unnecessary abstraction.

If a simple implementation exists, choose it.

---

# Before Writing Code

Before implementing anything:

1. Inspect the repository.
2. Read the documentation.
3. Explain the implementation plan.
4. Identify assumptions.
5. Ask clarifying questions if requirements are ambiguous.

Do not immediately generate code.

---

# Architecture

Follow the documented architecture.

Do not redesign the application unless explicitly requested.

Avoid introducing new architectural patterns without discussion.

---

# Scope Control

Do not expand project scope.

Do not add features because they "might be useful."

If a feature is outside the documented scope:

* explain why
* recommend it as future work
* do not implement it

---

# Database

Treat DATABASE.md as the source of truth.

Do not:

* add columns
* rename columns
* delete columns
* create additional tables

without explicit approval.

If schema changes are required:

Explain why first.

---

# API

Treat API.md as the public contract.

Do not introduce new endpoints without approval.

Do not modify request or response formats without discussion.

---

# Roadmap Discipline

Every feature belongs to a roadmap phase.

Before implementing a feature, ask:

> Which roadmap phase does this belong to?

If the feature belongs to a future phase:

Do not implement it.

---

# Dependencies

Do not add dependencies unless they provide clear value.

Prefer the standard library or existing project dependencies whenever possible.

Explain why a dependency is required before introducing it.

---

# Coding Style

Prefer:

* composition
* dependency injection
* descriptive names
* small classes
* small functions
* explicit types

Avoid:

* unnecessary inheritance
* deep abstraction
* generic factories
* service locators
* overuse of interfaces

Only introduce abstractions when multiple implementations exist.

---

# Error Handling

Handle errors explicitly.

Avoid silent failures.

Return meaningful errors.

Never swallow exceptions.

---

# Logging

Use structured logging.

Do not log:

* API keys
* secrets
* passwords

Logging should aid debugging without exposing sensitive information.

---

# Configuration

Configuration belongs in configuration files or environment variables.

Never hardcode:

* provider pricing
* API keys
* database credentials

---

# Documentation

Documentation is part of the implementation.

Whenever behavior changes:

Update the relevant documentation.

Possible documents include:

* PROJECT.md
* REQUIREMENTS.md
* ARCHITECTURE.md
* DATABASE.md
* API.md
* ROADMAP.md
* DECISIONS.md

Documentation should remain synchronized with the codebase.

---

# Git

Favor small, focused commits.

One logical change per commit.

Commit messages should clearly describe intent.

Examples:

* Add Anthropic provider adapter
* Implement telemetry persistence
* Calculate request cost
* Add telemetry retrieval endpoints

Avoid vague commit messages.

---

# Performance

Optimize only after correctness.

Do not introduce complexity for hypothetical performance gains.

Measure first.

Optimize second.

---

# Security

Never expose:

* provider API keys
* database credentials
* environment variables

Validate all external input.

Treat provider responses as untrusted data.

---

# Future Features

The following are intentionally excluded from the MVP:

* dashboards
* authentication
* organizations
* users
* teams
* VS Code extension
* browser extension
* NATS
* Redis
* Kubernetes
* analytics
* AI categorization
* prompt scoring

Do not implement these unless explicitly requested.

---

# When Unsure

If implementation details are unclear:

Stop.

Explain the uncertainty.

Ask for clarification.

Never guess architecture.

---

# Guiding Question

Before implementing any code, ask:

> Does this help us build a reliable AI Gateway Telemetry Service?

If the answer is "no," it does not belong in the MVP.
