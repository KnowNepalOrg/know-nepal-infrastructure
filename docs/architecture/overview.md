# Know Nepal Architecture Overview

This document provides a high-level overview of the architecture of the Know Nepal platform.

It describes how the major components of the platform are organized and how they communicate with each other.

> **Note:** The core Know Nepal application repositories are currently private. This document describes the architecture and system design without exposing private source code, credentials, secrets, or sensitive production configuration.

---

## Architecture Overview

Know Nepal is designed as a modular platform where different areas of Nepal-related information are separated into domain-specific services.

At a high level, the platform consists of:

* Frontend application
* API Gateway
* Domain services
* Databases
* Infrastructure and deployment services

The architecture is designed to allow individual domains to be developed, maintained, and evolved independently while remaining part of the same platform.

---

## High-Level Architecture

```text
                           Internet
                              │
                              ▼
                         Cloudflare
                              │
                              ▼
                         Frontend
                              │
                              ▼
                        API Gateway
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
          ▼                   ▼                   ▼
      Geography            History             Culture
       Service             Service             Service
          │                   │                   │
          ├───────────────────┼───────────────────┤
          │                   │                   │
          ▼                   ▼                   ▼
      Education           Healthcare          Wildlife
       Service             Service             Service
          │                   │                   │
          └───────────────────┬───────────────────┘
                              │
                              ▼
                       Data / Databases
```

The diagram represents the logical structure of the platform. The actual deployment topology may evolve as the project grows.

---

## Main Components

### Frontend

The frontend is the user-facing application through which users interact with Know Nepal.

It is responsible for:

* User interface
* Navigation
* Presenting information
* Communicating with backend APIs
* Client-side interactions

The frontend communicates with the backend through the API Gateway rather than directly managing individual backend services.

---

### API Gateway

The API Gateway acts as the central entry point for backend API requests.

Its responsibilities include:

* Request routing
* Service discovery or service addressing
* Authentication and authorization integration
* Centralized API access
* Cross-service request handling where required

The gateway provides a consistent API boundary between the frontend and backend services.

---

### Domain Services

Know Nepal separates major information domains into independent services.

Examples include:

* Geography
* History
* Culture
* Education
* Healthcare
* Wildlife
* Destinations

Each service is responsible for its own domain rather than handling unrelated areas of the platform.

This separation allows development and maintenance to happen around clear domain boundaries.

---

### Data Layer

The services use databases to persist application data.

The exact database structure and implementation may differ between services depending on future requirements.

Database credentials and production connection details are never stored in this public repository.

---

## Request Flow

A typical request follows a flow similar to:

```text
User
  │
  ▼
Frontend
  │
  ▼
API Gateway
  │
  ▼
Relevant Domain Service
  │
  ▼
Database
  │
  ▼
Domain Service
  │
  ▼
API Gateway
  │
  ▼
Frontend
  │
  ▼
User
```

For example, a request for information about a geographical location would be routed through the gateway to the relevant geography service.

---

## Why This Architecture?

The platform uses domain separation to keep different areas of Know Nepal independently maintainable.

This provides several advantages:

* Clear ownership of responsibilities
* Smaller and more focused services
* Easier maintenance
* Independent development of domains
* Reduced coupling between unrelated features
* Flexibility to evolve individual services over time

The architecture is not considered final. It will evolve as Know Nepal grows and real operational requirements become clearer.

---

## Infrastructure

Know Nepal's infrastructure may include services and tools for:

* Containerization
* Application hosting
* Database hosting
* DNS
* Reverse proxying
* Continuous integration and deployment
* Monitoring and operational management

Specific infrastructure configuration is maintained separately from this high-level architecture document.

---

## Repository Structure

The Know Nepal platform is organized across multiple repositories.

Application repositories contain the implementation of individual platform components, while this infrastructure repository contains shared infrastructure configuration and documentation.

A simplified structure is:

```text
Know Nepal Organization
│
├── Frontend
├── API Gateway
├── Domain Services
│   ├── Culture
│   ├── History
│   ├── Geography
│   ├── Education
│   ├── Healthcare
│   ├── Wildlife
│   └── Destinations
│
└── Infrastructure
```

The core application repositories are currently private.

---

## Security Boundary

Public architecture documentation does not contain:

* Passwords
* API keys
* Access tokens
* Private keys
* Production credentials
* Database passwords
* Private environment variables
* Sensitive infrastructure access information

Secrets are managed separately through appropriate environment and deployment mechanisms.

---

## Architecture Evolution

Know Nepal is an actively developing project.

Architecture decisions may change as:

* New domains are introduced
* Services evolve
* Infrastructure requirements change
* Performance requirements become clearer
* Contributors join the project
* Production experience provides new information

Significant architectural changes should be documented so that the reasoning behind the system remains understandable over time.
