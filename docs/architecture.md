# Memophor Platform Architecture

## Overview

Memophor delivers a distributed knowledge mesh built on the open-source SynaGraph engine (graph + vector + temporal storage) and surfaced through Knowlemesh APIs and Scedge edge caches. The platform supports long-term, context-aware memory for AI systems while minimizing redundant inference.

```
Client → Scedge (Edge PoP) → Regional Cache → Knowlemesh Origin → SynaGraph Engine → GPU/LLM Cluster (fallback)
```

## SynaGraph

- Rust-based engine exposing both gRPC and REST APIs.
- Manages graph-structured knowledge, ANN embeddings, temporal decay/reinforcement, and provenance metadata.
- Modular storage adapters (PostgreSQL, Redis, disk-based ANN index) with async abstractions.
- Emits policy events over NATS/Kafka for cache invalidation and audit trails.

## Knowlemesh

- Ruby on Rails control plane (API-first) orchestrating tenants, policies, and lifecycle automation across SynaGraph clusters and Scedge caches.
- Handles API key management, RAG planning, analytics, billing integration, and compliance workflows (HIPAA/GDPR).
- Exposes REST endpoints for cache directives (`/api/v1/artifacts/*`), policy management, and future UI modules.
- Integrates with identity providers and billing systems.

## Scedge

- Rust + Axum edge service optimized for low-latency responses.
- Maintains semantic caches keyed by intent, slots, tenant, policy, locale, model version, and content version.
- Artifact payloads capture provenance, policy, confidence, TTL, and ETag to align with governance requirements.
- `/lookup`, `/store`, and `/purge` endpoints handle cache hits, origin pushes, and graph-aware invalidations.
- Subscribes (planned) to SynaGraph supersede events and regional cache layers for distributed invalidation.

## Context Capsule Protocol (CCP / CCS)

- Defines the portable Capsule object: summaries, graph references, embeddings, provenance, and policy envelope.
- Transported via `/ccp/v0/capsules` HTTP/gRPC surface with export, fetch, query, and revoke operations.
- Enables AI→AI context transfer; SynaGraph stores canonical capsules, Knowlemesh enforces tenancy, Scedge caches for low-latency continuity.
- Versioned (v0.1 JSON) with roadmap toward binary encodings (CBOR/Protobuf) for high-throughput channels.

## Shared Concerns

- **Protobuf contracts** shared across repos under `proto/` ensuring consistent data shapes; CCP schemas published alongside JSON-Schema definitions.
- **Observability**: OpenTelemetry instrumentation, structured logging, and health probes.
- **Policy enforcement**: Central policy definitions compiled to WASM and enforced across services.

## Deployment Model

- Knowlemesh Cloud manages multi-tenant clusters, RBAC, and compliance (HIPAA/GDPR).
- SynaGraph supports federation and replication, exposed internally via gRPC.
- Scedge instances deployed in edge PoPs, peered with a regional message bus for invalidation events.

## Roadmap

- Harden storage adapters with benchmarking.
- Implement policy DSL + enforcement runtime.
- Expand CI orchestrations and integration testing across the mesh.
