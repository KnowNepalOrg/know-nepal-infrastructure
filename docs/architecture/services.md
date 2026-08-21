# Know Nepal Services and Application Components

This document describes the major application components and domain modules that make up the current Know Nepal platform.

> **Important:** Know Nepal is currently implemented as a **modular monolith**. The domains described below are modules within the Spring Boot backend rather than independently deployed microservices.

---

# Component Overview

The current Know Nepal platform can be viewed as:

```text
Know Nepal
│
├── Frontend
│   └── Next.js / React
│
├── Backend
│   └── Spring Boot Modular Monolith
│       │
│       ├── Geography
│       ├── History
│       ├── Culture
│       ├── Destinations
│       ├── Wildlife
│       ├── Education
│       └── Healthcare
│
├── Database
│   └── PostgreSQL
│
└── Infrastructure
    └── Deployment and operational resources
```

The frontend and backend are currently maintained within the `know-nepal-platform` repository.

---

# Frontend

## Responsibility

The frontend provides the user-facing application for Know Nepal.

## Main responsibilities

* Present information to users
* Provide navigation
* Communicate with backend APIs
* Render knowledge-domain pages
* Provide administrative interfaces
* Handle client-side interactions

## Technology

The current frontend uses:

* Next.js
* React
* TypeScript
* Tailwind CSS
* TanStack React Query
* Axios

The frontend is organized around the same major knowledge domains represented by the backend.

---

# Backend

## Responsibility

The backend provides the application API and domain logic for Know Nepal.

It is implemented as a single Spring Boot application with internal domain modules.

## Technology

The current backend uses:

* Java 21
* Spring Boot
* Spring MVC
* Spring Data JPA
* Hibernate
* PostgreSQL
* Flyway
* Spring Security
* JWT
* Caffeine
* OpenAPI / Springdoc

The backend source is located under:

```text
know-nepal-platform/
└── know-nepal-monolith/
```

---

# Geography

## Responsibility

The Geography module manages structured geographical information about Nepal.

The current implementation includes concepts such as:

* Provinces
* Districts
* Municipalities
* Wards
* Emergency contacts

The module contains its own controllers, services, repositories, domain models, DTOs, and persistence configuration.

Database migrations for the domain are maintained separately under the geography migration directory.

---

# History

## Responsibility

The History module manages structured historical information about Nepal.

The current implementation includes concepts such as:

* Historical eras
* Historical figures
* Dynasties
* Historical events

The module contains domain-specific application and persistence components.

---

# Culture

## Responsibility

The Culture module manages structured information related to Nepal's cultural heritage and diversity.

The current implementation includes concepts such as:

* Languages
* Ethnic groups
* Festivals
* Art forms
* Traditional attire
* Culture media

The module has its own domain logic, repositories, DTOs, and database migrations.

---

# Destinations

## Responsibility

The Destinations module manages structured information about destinations and related travel information.

The current implementation contains concepts including:

* Tourist destinations
* Destination categories
* Destination tags
* Trekking routes
* Destination highlights
* Destination reviews
* Destination media
* Destination fees
* Itineraries
* Nearby destinations
* Destination weather

This is one of the larger domain modules in the current backend.

The module contains its own persistence configuration and migration history.

---

# Wildlife

## Responsibility

The Wildlife module manages information related to Nepal's wildlife and biodiversity.

The current implementation includes concepts such as:

* Wildlife species
* Flora species
* National parks
* Lakes
* Wildlife media

The module maintains its own domain logic and database migrations.

---

# Education

## Responsibility

The Education module manages structured information related to education in Nepal.

The current implementation contains concepts including:

* Schools
* Colleges
* Universities
* Programs
* Exam boards
* Entrance examinations
* Scholarships
* Teacher profiles
* Academic calendars
* Fee breakdowns
* Rankings

The Education module is one of the larger modules in the current backend and contains extensive domain-specific application and persistence code.

---

# Healthcare

## Responsibility

The Healthcare module manages structured healthcare-related information.

The current implementation includes concepts such as:

* Hospitals
* Specialties
* Hospital-specialty relationships

The module contains its own domain logic, persistence configuration, repositories, and migrations.

---

# Domain Module Structure

The backend follows a domain-oriented organization.

A typical module contains components such as:

```text
Domain
│
├── controller
├── service
├── repository
├── model
├── dto
├── mapper
├── specification
├── exception
└── validation / utility components
```

Not every domain necessarily contains exactly the same set of packages.

The structure should follow the needs of the domain rather than enforcing unnecessary uniformity.

---

# Persistence Components

Each major domain has domain-specific persistence configuration.

The persistence configuration can include:

* DataSource
* EntityManagerFactory
* TransactionManager
* Repository configuration
* Flyway migration configuration

This allows the modular monolith to maintain stronger persistence boundaries between domains.

A simplified representation is:

```text
Geography
    │
    └── Geography persistence

History
    │
    └── History persistence

Culture
    │
    └── Culture persistence

Destinations
    │
    └── Destinations persistence

Wildlife
    │
    └── Wildlife persistence

Education
    │
    └── Education persistence

Healthcare
    │
    └── Healthcare persistence

                │
                ▼

            PostgreSQL
```

---

# Communication Between Modules

The domains currently exist within the same backend application.

They therefore do not communicate through HTTP as independently deployed microservices would.

Instead, relationships between domains can be handled within the same application process where required.

This reduces network and service-discovery overhead while preserving logical domain boundaries.

Cross-domain dependencies should remain deliberate and limited.

---

# Authentication Component

Authentication is implemented at the backend application level.

Relevant components include:

* Authentication controller
* JWT utility
* JWT authentication filter
* Spring Security configuration

The frontend also provides authentication handling for administrative routes.

Authentication and authorization should remain clearly separated in future development.

---

# Caching Component

The backend uses Caffeine for application-level caching.

The cache is local to the running backend process.

It is therefore appropriate for data that can safely be cached within an individual application instance.

It should not be treated as a shared distributed data store.

---

# Database

PostgreSQL is the primary persistent data store.

Database schema changes are managed through Flyway migrations.

Migration files are separated by domain:

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

This structure keeps database evolution aligned with the domain modules.

---

# API Layer

The backend exposes REST APIs through Spring Boot controllers.

OpenAPI / Springdoc is included for API documentation support.

The separate `know-nepal-api` repository exists within the organization, but its exact current runtime responsibility should be documented from that repository itself rather than inferred from the application repository.

---

# Infrastructure

Infrastructure is maintained separately from the application code.

The infrastructure repository is responsible for infrastructure-specific concerns such as deployment and operational configuration.

The application architecture should therefore be understood independently from the infrastructure implementation.

```text
Application Architecture
        │
        ▼
know-nepal-platform
        │
        ├── Frontend
        └── Backend
               │
               ▼
       Infrastructure Layer
               │
               ▼
     Deployment / Hosting / Data
```

---

# Service vs Module

The terminology used in the current architecture is important.

A **module** is a logical domain boundary inside the modular monolith.

A **service** normally refers to a separately deployable application or a specific application-layer component.

The current seven knowledge domains should therefore be described as **domain modules**, not independent microservices.

For example:

```text
Current:

Know Nepal Monolith
    └── Geography Module


Not current:

Know Nepal
    └── Geography Microservice
```

This distinction should be maintained throughout project documentation.

---

# Adding a New Domain Module

A new domain should only be introduced when there is a meaningful domain boundary.

Before adding one, consider:

1. What problem does the domain represent?
2. What data does it own?
3. What functionality belongs inside it?
4. Does the functionality already belong to an existing domain?
5. What relationships does it have with other domains?
6. Does the domain require its own persistence boundary?
7. Is a new module actually necessary?

The goal is to maintain useful domain boundaries rather than maximize the number of modules.

---

# Future Extraction

The current modular architecture does not prevent a future domain from being extracted into a separate service if there is a concrete reason to do so.

Possible reasons could include:

* Independent scaling requirements
* Independent deployment requirements
* Strong operational boundaries
* Different infrastructure requirements
* Clear ownership boundaries
* Significant workload differences

However, a domain should not be extracted merely because it is possible.

The current modular monolith remains the source of truth until such a decision is actually made and implemented.

---

# Historical Architecture

Know Nepal previously explored a microservices-oriented architecture in which domains were represented as separate services behind an API Gateway.

That architecture is **historical**.

The current implementation does not use the previous service-per-domain deployment model.

Historical architecture documentation may be retained for understanding architectural evolution, but it should be clearly marked as historical.

---

# Architectural Principle

The current architecture follows a simple principle:

> **Maintain clear domain boundaries without introducing distributed-system complexity unless there is a concrete reason to do so.**

This allows Know Nepal to evolve its domain structure while keeping the operational model manageable for the current project.

---

# Status

**Current architecture:** Implemented

**Architecture style:** Modular monolith

**Domain modules:** Geography, History, Culture, Destinations, Wildlife, Education, Healthcare

**Independent domain microservices:** Historical / not current

**PostgreSQL persistence:** Implemented

**Flyway migrations:** Implemented

**JWT authentication:** Implemented

**Application caching:** Implemented
