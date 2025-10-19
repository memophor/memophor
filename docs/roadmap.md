# Platform Roadmap (Initial)

## SynaGraph
- Implement persistent storage adapters (PostgreSQL + Redis) with migrations.
- Add vector index integration (HNSW / DiskANN) and benchmarking harness.
- Build policy-aware temporal decay scheduler and reinforcement API.
- Expose provenance and supersede logic via gRPC + REST.

## Knowlemesh
- Implement Rails domain models for tenants, policies, and API credentials (PostgreSQL-backed).
- Build orchestration workflows for cluster management, cache directives, and backup automation.
- Add analytics pipelines and dashboard endpoints (Rails views or SPA integration).
- Integrate billing, compliance tooling, and audit log exports.

## Scedge
- Extend Rust cache store with pluggable Redis/KeyDB backends and per-tenant limits.
- Connect to event bus for `SUPERSEDED_BY` invalidation handling and regional cache coordination.
- Add RAG plan/template caching and streaming transport (e.g., Server-Sent Events).
- Deploy edge observability stack (Prometheus metrics, OpenTelemetry traces/logs).

## Cross-Cutting
- Standardize protobuf contracts and OpenAPI definitions.
- Establish CI/CD pipelines and container build strategies.
- Expand integration and chaos testing across the mesh.

## Milestones
| Milestone | Focus | ETA |
|-----------|-------|-----|
| v0.1 OSS | SynaGraph + Scedge PoC + CCP JSON spec | Q1 2026 |
| v0.2 Cloud | Multi-tenant control plane, dashboards | Q2 2026 |
| v1.0 Fabric | Federation, CCP v1 (binary encodings), enterprise pilots | 2027 |
