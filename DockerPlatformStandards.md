# Docker Platform Standards

**Status:** Draft v0.1  
**Owner:** Robert Hagglund  
**Last Updated:** 2026-11-06

---

# Purpose

This document defines the baseline standards used for all Docker-based platform projects in the lab environment.

The goals are:

- Consistent deployment patterns
- Predictable network allocation
- Self-contained projects
- Repeatable recovery procedures
- Clear documentation standards
- Minimal cross-project dependencies

---

# Platform Naming

Each major solution is implemented as an independent platform.

Current platforms:

| Platform | Host IP |
|-----------|-----------|
| CivicOS | 10.0.7.200 |
| Sandbox | 10.0.8.200 |
| Models | 10.0.9.200 |
| Shared | 10.0.10.200 |

Platform names should be short, descriptive, and business-focused.

Avoid technology-specific names.

Preferred:

- Models
- Shared
- Monitoring
- Media

Avoid:

- Docker-AI
- OllamaStack
- PostgreSQLServer

---

# Network Standard

## Address Scheme

Each platform receives a dedicated subnet:

```text
10.0.n.0/24
```

The platform host IP shall be:

```text
10.0.n.200
```

Examples:

```text
10.0.7.200    CivicOS
10.0.8.200    Sandbox
10.0.9.200    Models
10.0.10.200   Shared
```

---

# Platform Principles

All platforms shall be:

- Self-contained
- Independently deployable
- Git-managed
- Documented
- Backed up
- Recoverable

A platform should continue operating if another platform is offline whenever reasonably practical.

---

# Directory Structure

Recommended layout:

```text
platform/
├── compose/
├── config/
├── data/
├── backups/
├── docs/
├── logs/
└── .env
```

---

# Documentation Requirements

Each platform shall contain:

```text
docs/
├── architecture.md
├── changelog.md
├── decisions.md
└── inventory.md
```

## architecture.md

Contains:

- System overview
- Service relationships
- Ports
- External dependencies

## changelog.md

Contains:

- Significant modifications
- Version milestones
- Upgrade history

## decisions.md

Contains:

- Architectural decisions
- Rationale
- Rejected alternatives

## inventory.md

Contains:

- Services
- Containers
- Volumes
- Networks
- Databases

---

# Source Control

All platforms should be managed through Git.

Repository contents should include:

```text
compose/
config/
docs/
```

Repository contents should not include:

```text
data/
backups/
logs/
```

unless intentionally archived.

---

# Database Standard

Databases should be treated as platform resources.

Preferred database:

```text
PostgreSQL
```

Use SQLite only when:

- single-user
- disposable
- temporary
- proof-of-concept

---

# Container Standards

All containers should:

- Use explicit image versions whenever practical
- Use persistent storage
- Use restart policies
- Use named volumes or mapped folders

Preferred restart policy:

```yaml
restart: unless-stopped
```

---

# Service Expansion

Services added later should fit naturally within the platform without requiring structural redesign.

Example:

```text
Models
├── Ollama
├── Open WebUI
├── PostgreSQL
├── LiteLLM
├── Qdrant
└── ComfyUI
```

The platform remains "Models" regardless of internal technologies.

---

# Shared Services

Cross-platform services belong in the Shared platform whenever appropriate.

Examples:

- SMTP relay
- Notification services
- Certificate management
- Monitoring infrastructure
- Shared utility services

Application-specific services should remain within their owning platform.

---

# Backup Philosophy

Platforms should be recoverable from:

- Git repository
- Configuration export
- Platform backup

Recovery should not depend on undocumented manual steps.

---

# Decision Log

## 2026-11-06

Established initial platform standards:

- CivicOS assigned 10.0.7.200
- Sandbox assigned 10.0.8.200
- Models assigned 10.0.9.200
- Shared assigned 10.0.10.200

Established subnet convention:

```text
10.0.n.0/24
```

Established host convention:

```text
10.0.n.200
```

Defined self-contained platform architecture as the preferred deployment model.
