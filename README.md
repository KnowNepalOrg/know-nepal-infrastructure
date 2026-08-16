# Know Nepal Infrastructure

Central infrastructure, deployment, architecture, and operational configuration for the **Know Nepal** platform.

Know Nepal is being developed as a long-term digital platform for organizing and making information about Nepal more accessible across areas such as geography, history, culture, education, healthcare, wildlife, and destinations.

This repository contains the infrastructure and documentation required to understand, operate, and evolve the Know Nepal platform.

> **Note:** The core Know Nepal application and service repositories are currently private. This repository does not expose private source code, credentials, secrets, or sensitive production configuration.

---

## Purpose

`know-nepal-infrastructure` exists to provide a central place for:

* System architecture documentation
* Infrastructure configuration
* Deployment configuration
* Environment documentation
* Networking and service communication
* CI/CD configuration
* Operational procedures
* Development and setup documentation
* Technical decisions and standards
* Contributor guidance

The goal is to make the platform understandable and maintainable as the project grows.

---

## Architecture

Know Nepal is organized around separate application services and platform components.

At a high level:

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
   Domain Services     Domain Services     Domain Services
        │                   │                   │
        └───────────────────┼───────────────────┘
                            │
                            ▼
                       Data Layer
```

The detailed architecture is documented in:

`docs/architecture/`

Architecture documentation may describe private services and their relationships without exposing their source code or credentials.

---

## Repository Structure

```text
know-nepal-infrastructure/
│
├── README.md
│
├── docs/
│   ├── architecture/
│   ├── development/
│   ├── operations/
│   └── governance/
│
├── docker/
│
├── deployment/
│
├── config/
│
├── scripts/
│
├── .github/
│   └── workflows/
│
├── .env.example
├── .gitignore
└── LICENSE
```

### `docs/`

Documentation for architecture, development, operations, and project governance.

### `docker/`

Container and local infrastructure configuration.

### `deployment/`

Deployment-related configuration and documentation.

### `config/`

Infrastructure and service configuration that is safe to keep in version control.

### `scripts/`

Utility scripts used for development, deployment, maintenance, or verification.

### `.github/`

GitHub Actions and repository automation.

---

## Core Principles

### 1. Keep secrets out of Git

Never commit:

* Passwords
* API keys
* Access tokens
* Private keys
* Production `.env` files
* Database credentials
* Cloud credentials
* Service secrets

Use `.env.example` or documented environment variables instead.

### 2. Infrastructure should be reproducible

Infrastructure configuration should be documented and version controlled whenever practical.

A new contributor or maintainer should be able to understand how the system is assembled without relying entirely on undocumented personal knowledge.

### 3. Documentation is part of the system

Important architectural and operational knowledge should not exist only in private conversations or Discord messages.

Permanent decisions should be documented in GitHub.

### 4. Keep responsibilities separated

Application repositories contain application code.

This repository contains infrastructure, deployment, architecture, and operational concerns.

---

## Development

Development and local environment instructions are available under:

```text
docs/development/
```

Before running infrastructure locally, check the required environment variables and dependencies.

Do not use production credentials for local development.

---

## Deployment

Deployment documentation is maintained under:

```text
docs/operations/
```

The deployment configuration may include multiple environments and services.

Production credentials and secrets are intentionally excluded from this repository.

---

## Contributing

Know Nepal is being built incrementally.

The project currently keeps its core implementation repositories private while the architecture, infrastructure practices, and community structure continue to develop.

If you are interested in contributing:

1. Explore the public documentation.
2. Join the Know Nepal community through the organization's Discord.
3. Discuss an area you would like to contribute to.
4. Review the relevant documentation and repository requirements.
5. Coordinate with the maintainers before making substantial changes.
6. Follow the project's contribution and review process.

Community discussion should happen through Discord when real-time communication is useful, while important technical decisions should be recorded in GitHub so they remain accessible to future contributors.

---

## Project Status

Know Nepal is an actively developing project.

The architecture and infrastructure are expected to evolve as the platform grows, new domains are introduced, and more contributors become involved.

Documentation may therefore change alongside the system.

---

## Security

If you discover a security issue, **do not publish sensitive details in a public issue**.

Contact the project maintainers privately with enough information to reproduce and assess the issue.

Never commit credentials or other sensitive information to this repository.

---

## License

See the repository license for the current terms of use and contribution.

---

## Community

Know Nepal has a community Discord for contributors and people interested in the project.

The organization profile contains the current community link and project information.

---

## Vision

Know Nepal aims to build a structured, maintainable, and continuously improving digital knowledge platform focused on Nepal.

The infrastructure is designed to support that goal while allowing the project to grow from a small development effort into a larger collaborative platform over time.

---

## License

This repository is licensed under the **MIT License**.

The MIT License permits others to use, copy, modify, merge, publish, distribute, sublicense, and sell copies of the contents of this repository, subject to the conditions of the license.

The license applies **only to the contents of this repository** unless explicitly stated otherwise. It does not grant access to or rights over Know Nepal's private repositories, proprietary source code, credentials, infrastructure secrets, trademarks, domains, or other resources that are not included in this repository.

See the [`LICENSE`](LICENSE) file for the complete license text.

Copyright (c) 2026 Know Nepal
