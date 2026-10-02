# Know Nepal Infrastructure

Infrastructure configuration and operational resources for the
**Know Nepal** project.

This repository is intended for infrastructure-related configuration
that is maintained separately from the application source code.

## Scope

This repository may contain resources related to:

- Deployment
- Hosting
- Environment configuration
- Database infrastructure
- Networking
- Backups
- Containerization
- Operational tooling

Infrastructure resources should be added here when they are managed
separately from the application repositories.

## Repository Boundary

Application source code remains in the appropriate application
repositories.

Project-wide technical documentation is maintained in
[`know-nepal-docs`](https://github.com/KnowNepalOrg/know-nepal-docs).

Application-level CI/CD configuration may remain with the application
repository when it is tightly coupled to the application build and
deployment process.

This repository focuses on infrastructure concerns that are
independent from application source code.

## Environments

Infrastructure may support different environments such as:

- Development
- Staging
- Production

Environment-specific configuration should remain isolated where
necessary.

## Secrets

Secrets and credentials must never be committed to this repository.

Sensitive values should be provided through the appropriate environment
or secret-management mechanism.

## Related Documentation

See
[`know-nepal-docs`](https://github.com/KnowNepalOrg/know-nepal-docs)
for project architecture, development practices, security architecture,
and other technical documentation.
