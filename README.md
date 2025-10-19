# Memophor Platform Workspace

Welcome to the local workspace for the Memophor platform. This directory holds the source code for the core components that power the Knowlemesh distributed knowledge mesh and its supporting services.

## Repositories

- **synagraph/** – [Open-source synaptic graph engine](https://github.com/memophor/synagraph) responsible for storage, similarity search, temporal reinforcement, and policy-aware provenance. Licensed under Apache-2.0.
- **knowlemesh/** – Commercial orchestration layer that exposes unified APIs, tenancy, policy enforcement, analytics, and lifecycle automation on top of SynaGraph clusters.
- **scedge/** – Edge cache layer (“Scedge”) that delivers low-latency knowledge responses and enforces policy-aware invalidation closer to the user.
- **docs/** – Shared documentation, architecture notes, and design decisions.

## Getting Started

Each repository maintains its own tooling and setup instructions. In general:

1. Review the architecture overview in `docs/architecture.md`.
2. Follow the language-specific prerequisites documented in each repo (`README.md`).
3. Use the provided Makefiles and scripts to run tests, start services, and develop locally.

## Development Principles

- **Open interfaces** – All services expose gRPC + HTTP APIs with shared protobuf contracts.
- **Deterministic builds** – Reproducible tooling with lockfiles and pinned versions.
- **Observability first** – Standardized logging, metrics, and tracing across the stack.
- **Testability** – Lightweight smoke tests included from day one to keep scaffolds healthy.

## Next Steps

- Flesh out the feature roadmap per repo (see `docs/roadmap.md`).
- Connect CI/CD pipelines and dockerized development environments.
- Expand integration tests across the mesh once the service surfaces stabilize.

