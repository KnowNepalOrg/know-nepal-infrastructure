# Know Nepal Infrastructure

Infrastructure and deployment configuration for the Know Nepal project.

This repository contains the operational configuration required to
deploy, connect, and maintain Know Nepal's application environments.

## Responsibilities

This repository manages:

- Deployment configuration
- Infrastructure configuration
- Environment configuration
- Networking
- Database infrastructure
- Containerization
- CI/CD
- Operational tooling

## Repository Boundary

Application source code is maintained in the appropriate application
repositories.

Project-wide technical documentation is maintained in
[`know-nepal-docs`](https://github.com/KnowNepalOrg/know-nepal-docs).

This repository focuses on the infrastructure required to run Know
Nepal.

## Environments

Infrastructure may be organized around:

- Development
- Staging
- Production

Environment-specific configuration should remain isolated from other
environments.

## Secrets

Secrets and credentials must never be committed to this repository.

Sensitive values should be provided through the appropriate secret or
environment configuration mechanism.

## Related Documentation

See
[`know-nepal-docs`](https://github.com/KnowNepalOrg/know-nepal-docs)
for project architecture, development practices, security architecture,
and other technical documentation.
