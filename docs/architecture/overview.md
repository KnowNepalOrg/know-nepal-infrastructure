# Know Nepal Architecture Overview

This document provides a high-level overview of the current Know Nepal platform architecture.

It describes the major application components, domain boundaries, persistence architecture, and the relationship between the application and infrastructure.

> **Current architecture:** Know Nepal is currently implemented as a **modular monolith**. Earlier microservices-oriented designs are historical and should not be treated as the current implementation.

---

## Architecture at a Glance

The current Know Nepal platform consists primarily of:

* A Next.js frontend
* A Spring Boot modular monolith backend
* Seven domain modules
* PostgreSQL persistence
* Flyway database migrations
* Supporting infrastructure and deployment configuration

At a high level:

```text
                         Internet
                            │
                            ▼
                     Know Nepal Frontend
                       Next.js / React
                            │
                         HTTP / REST
                            │
                            ▼
                  Know Nepal Monolith
                    Spring Boot / Java
                            │
       ┌────────────────────┼────────────────────┐
       │                    │                    │
       ▼                    ▼                    ▼
   Geography            History              Culture
       │                    │                    │
       ├──────────────┬─────┴──────────────┬─────┤
       ▼              ▼                    ▼     ▼
 Destinations       Wildlife            Education Healthcare
       │              │                    │       │
       └──────────────┴──────────┬─────────┴───────┘
                                 │
                                 ▼
                             PostgreSQL
```

The seven domains run within the same backend application rather than as independently deployed services.

---

# Modular Monolith

The backend uses a modular monolith architecture.

The application is deployed as a single Spring Boot application while its internal code is separated into domain-oriented modules.

The current domain modules are:

```text
know-nepal-monolith
│
├── Geography
├── History
├── Culture
├── Destinations
├── Wildlife
├── Education
└── Healthcare
```

Each domain contains its own domain-specific application logic and persistence components.

This provides domain separation without requiring each domain to become a separately deployed service.

---

# Frontend

The frontend is implemented using Next.js, React, and TypeScript.

Its responsibilities include:

* Providing the user interface
* Navigation and page rendering
* Displaying knowledge-domain information
* Communicating with the backend API
* Providing administrative interfaces
* Managing client-side interactions

The frontend communicates with the backend through HTTP APIs.

The frontend and backend are maintained together inside the `know-nepal-platform` repository.

---

# Backend

The backend is implemented as a Spring Boot modular monolith.

The main backend application is located under:

```text
know-nepal-platform/
└── know-nepal-monolith/
```

The backend contains seven major domain modules:

```text
com.knownepal
│
├── geography
├── history
├── culture
├── destinations
├── wildlife
├── education
└── healthcare
```

The backend also contains shared application-level and persistence configuration.

---

# Domain Boundaries

Each domain is responsible for its own area of Nepal-related information.

```text
Geography
    └── Geographic information

History
    └── Historical information

Culture
    └── Cultural information

Destinations
    └── Places and destination information

Wildlife
    └── Wildlife and biodiversity information

Education
    └── Education-related information

Healthcare
    └── Healthcare-related information
```

The domains exist as logical boundaries within the same application.

A domain should avoid taking ownership of unrelated functionality that belongs to another domain.

---

# Persistence Architecture

The backend uses PostgreSQL for persistent data storage.

The application has domain-specific persistence configuration.

Each domain has its own persistence configuration containing components such as:

* DataSource
* EntityManagerFactory
* TransactionManager
* Repository layer
* Flyway migration location

The current structure therefore provides stronger persistence boundaries than a conventional monolithic application with one shared persistence configuration.

A simplified view is:

```text
                    Spring Boot Monolith
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
   Geography          Destinations        Education
   Persistence         Persistence        Persistence
        │                  │                  │
        └──────────────────┼──────────────────┘
                           │
                           ▼
                       PostgreSQL
```

The production configuration supports using PostgreSQL schemas to maintain domain separation and can also be configured for separate database connections.

The exact production topology is an infrastructure concern and should be documented alongside the deployment configuration.

---

# Database Migrations

Database schema changes are managed using Flyway.

Migration files are organized by domain:

```text
db/migration/
├── culture/
├── destinations/
├── education/
├── geography/
├── healthcare/
├── history/
└── wildlife/
```

This keeps database evolution aligned with the domain boundaries of the application.

Production schema management is intentionally separated from automatic Hibernate schema generation.

---

# Request Lifecycle

A typical request follows this general flow:

```text
User
  │
  ▼
Next.js Frontend
  │
  │ HTTP / REST
  ▼
Spring Boot Controller
  │
  ▼
Domain Service
  │
  ▼
Repository
  │
  ▼
JPA / Hibernate
  │
  ▼
Domain Persistence Configuration
  │
  ▼
PostgreSQL
```

The response then travels back through the application to the frontend.

The exact request path differs depending on the domain and endpoint.

---

# Authentication

The backend contains JWT-based authentication.

The current authentication flow includes:

```text
Admin
  │
  ▼
Authentication Endpoint
  │
  ▼
JWT
  │
  ▼
Authenticated Request
```

The frontend also contains authentication handling for administrative routes.

Authentication and authorization should be considered separately:

* Authentication determines whether a request is associated with a valid identity.
* Authorization determines what that identity is allowed to do.

The current implementation contains authentication functionality, while backend authorization enforcement is an area that requires further hardening.

---

# Caching

The backend uses Caffeine for application-level caching.

Caching is local to the application process rather than being a distributed cache.

The current configuration uses bounded cache size and expiration settings.

Caching should therefore be understood as a performance optimization within the current monolithic deployment rather than as shared distributed state.

---

# API

The backend exposes HTTP APIs through Spring Boot controllers.

API documentation support is provided through OpenAPI / Springdoc.

The separate `know-nepal-api` repository is maintained independently within the Know Nepal organization. Its exact relationship to the current runtime architecture should be documented from that repository rather than assumed here.

---

# Infrastructure

Infrastructure is maintained separately from the application source code.

The infrastructure repository contains infrastructure-specific configuration and documentation.

This separation allows:

```text
Application
    │
    ├── Frontend
    └── Backend
            │
            ▼
       Infrastructure
            │
            ├── Hosting
            ├── Database infrastructure
            ├── Deployment
            └── Operational configuration
```

Infrastructure configuration should not be duplicated unnecessarily inside application documentation.

---

# Deployment Model

The backend includes Docker-based deployment configuration.

The application is packaged into a container and runs using a Java 21 runtime.

The frontend is a separately built Next.js application.

The exact production deployment topology is maintained through the infrastructure configuration and should be treated as deployment-specific rather than part of the domain architecture.

---

# Why a Modular Monolith?

The current architecture provides several advantages for the present development context:

* Clear domain boundaries
* One deployable backend application
* Lower operational complexity than multiple independently deployed services
* Easier local development
* Easier debugging across domain boundaries
* Shared application infrastructure where appropriate
* Ability to maintain domain-specific persistence
* A possible path toward future extraction if a domain eventually requires independent deployment

The architecture does not assume that every domain must become a separate service.

A separate service should only be introduced when there is a concrete architectural or operational reason to do so.

---

# Architecture Evolution

Know Nepal previously explored a microservices-oriented architecture.

That architecture is considered **historical**.

The current implementation is a modular monolith.

The move toward a modular monolith should be understood as an engineering trade-off involving factors such as:

* Infrastructure complexity
* Deployment overhead
* Operational complexity
* Maintainability
* Domain boundaries
* Current development constraints
* Future scalability requirements

The previous architecture should not be presented as the current runtime architecture.

---

# Source of Truth

When architecture documentation conflicts with the current implementation, the current source code and configuration should be treated as the primary source of truth.

Architecture documentation should be updated when significant implementation decisions change.

The project should explicitly distinguish between:

* Implemented
* Partial
* Planned
* Proposed
* Historical
* Deprecated
* Unknown

This prevents future documentation from accidentally describing planned or historical architecture as current.
