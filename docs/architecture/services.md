# Know Nepal Services

This document describes the major services and application components that make up the Know Nepal platform.

The purpose of this document is to define clear responsibilities and boundaries between components.

> **Note:** The core service repositories are currently private. This document provides architectural information without exposing private source code or sensitive configuration.

---

## Service Overview

Know Nepal is organized around multiple domain-specific services.

Each service focuses on a specific area of the platform.

```text
Know Nepal
│
├── Frontend
│
├── API Gateway
│
├── Culture Service
├── History Service
├── Geography Service
├── Education Service
├── Healthcare Service
├── Wildlife Service
└── Destinations Service
```

---

## Frontend

### Responsibility

The frontend provides the user-facing interface for Know Nepal.

### Main responsibilities

* Present information to users
* Provide navigation
* Consume backend APIs
* Handle user interactions
* Provide a consistent user experience

### Technology

The frontend is built using modern web technologies, including:

* Next.js
* React
* TypeScript
* Tailwind CSS

The frontend communicates with backend services through the API Gateway.

---

## API Gateway

### Responsibility

The API Gateway provides a centralized entry point for backend API requests.

### Main responsibilities

* Route requests to appropriate services
* Provide a consistent API boundary
* Handle authentication and authorization integration
* Centralize API-level concerns
* Reduce direct coupling between the frontend and individual services

The gateway does not own the domain data of individual services.

---

## Culture Service

### Responsibility

The Culture Service manages information related to the cultural aspects of Nepal.

Potential areas include:

* Traditions
* Festivals
* Languages
* Arts
* Music
* Cultural practices
* Heritage-related information

The service is responsible for the culture domain and its associated data and APIs.

---

## History Service

### Responsibility

The History Service manages historical information about Nepal.

Potential areas include:

* Historical periods
* Important events
* Historical figures
* Political history
* Social history
* Historical locations

The service is responsible for the history domain.

---

## Geography Service

### Responsibility

The Geography Service manages geographical information about Nepal.

Potential areas include:

* Provinces
* Districts
* Municipalities
* Mountains
* Rivers
* Lakes
* Geographic regions
* Coordinates and geographic relationships

The service is responsible for geographical data and related APIs.

---

## Education Service

### Responsibility

The Education Service manages information related to education in Nepal.

Potential areas include:

* Educational institutions
* Academic information
* Education systems
* Programs
* Educational resources
* Related educational data

The service is responsible for the education domain.

---

## Healthcare Service

### Responsibility

The Healthcare Service manages information related to healthcare resources and information in Nepal.

Potential areas may include:

* Healthcare institutions
* Hospitals
* Health services
* Healthcare-related resources
* Public health information

The exact scope of this service may evolve as the platform develops.

---

## Wildlife Service

### Responsibility

The Wildlife Service manages information about Nepal's wildlife and biodiversity.

Potential areas include:

* Wildlife species
* Habitats
* Conservation areas
* National parks
* Protected species
* Biodiversity information

The service is responsible for wildlife and biodiversity-related information.

---

## Destinations Service

### Responsibility

The Destinations Service manages information about places and destinations in Nepal.

Potential areas include:

* Tourist destinations
* Places of interest
* Attractions
* Travel-related information
* Destination metadata

The service is responsible for destination-related information.

---

## Service Boundaries

Each service should have a clearly defined responsibility.

A service should avoid taking ownership of data or functionality that belongs to another domain.

For example:

```text
Geography Service
    └── Geographic information

History Service
    └── Historical information

Culture Service
    └── Cultural information

Wildlife Service
    └── Wildlife and biodiversity information
```

When functionality crosses multiple domains, the architectural relationship should be evaluated before introducing direct dependencies.

---

## Communication

The frontend communicates with the backend through the API Gateway.

The gateway routes requests to the appropriate domain service.

A simplified communication flow is:

```text
Frontend
    │
    ▼
API Gateway
    │
    ├── Geography Service
    ├── History Service
    ├── Culture Service
    ├── Education Service
    ├── Healthcare Service
    ├── Wildlife Service
    └── Destinations Service
```

The exact communication patterns may evolve as the system develops.

---

## Technology

The current platform uses technologies including:

| Area                 | Technology                 |
| -------------------- | -------------------------- |
| Frontend             | Next.js, React, TypeScript |
| Backend              | Java, Spring Boot          |
| Database             | PostgreSQL                 |
| Build                | Maven                      |
| Containerization     | Docker                     |
| Hosting / Deployment | Cloud infrastructure       |
| DNS / Edge           | Cloudflare                 |

Technology choices may change as the platform evolves.

---

## Adding a New Service

A new service should only be introduced when there is a clear domain boundary or architectural reason for separating it.

Before creating a new service, consider:

1. What responsibility does it own?
2. Does that responsibility belong to an existing domain?
3. What data does it own?
4. What APIs does it expose?
5. What other services depend on it?
6. Does the additional operational complexity justify a separate service?

The goal is not to maximize the number of services.

The goal is to maintain clear and useful boundaries.

---

## Future Evolution

The current service structure is not necessarily permanent.

As Know Nepal grows, services may:

* Be expanded
* Be split into additional domains
* Be merged when separation no longer provides value
* Introduce new dependencies
* Adopt different infrastructure requirements

Architectural changes should be documented when they materially affect the structure of the platform.
