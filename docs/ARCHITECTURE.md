# 🏗️ Memophor Platform Architecture
> Revision: October 18, 2025  
> Author: Memophor Architecture Team

---

## 1. Purpose

This document defines the **technical architecture** of the Memophor Platform — a federated, self-optimizing knowledge infrastructure for AI memory.  
It describes the relationships between components, their responsibilities, interfaces, and deployment model across open-source and commercial layers.

---

## 2. Architectural Vision

Memophor provides an **AI memory fabric** composed of decentralized services that collectively form the **Federated Knowledge Mesh**:

- **SynaGraph** – core graph + vector + temporal engine (open source).  
- **Knowlemesh** – orchestration and governance control plane.  
- **Scedge** – smart cache deployed at the network edge.  
- **SeTGIN** – self-tuning intelligence network that adapts performance in real time.  
- **CCP (Context Capsule Protocol)** – the standard for inter-AI context exchange.

Together these layers enable long-term, context-aware memory for AI agents while reducing redundant GPU inference and ensuring tenant-scoped security.

---

## 3. High-Level Data Flow

### End-to-End Overview
```
Client / AI Agent
        │
        │ 1️⃣  Query or context request
        ▼
┌─────────────────────────────────────────────┐
│  Scedge – Smart Cache on the Edge           │
│  • Local semantic cache (intent + policy)   │
│  • /lookup, /store, /purge APIs             │
│  • Policy + provenance validation           │
│  • Returns cached result or forwards miss   │
└─────────────────────────────────────────────┘
        │
        │ 2️⃣  Cache miss → Capsule request
        ▼
┌─────────────────────────────────────────────┐
│  Knowlemesh – Control Plane / Orchestrator  │
│  • Tenant registry, RBAC, compliance        │
│  • Policy enforcement & API gateway         │
│  • Manages SynaGraph clusters + Scedge PoPs │
│  • Aggregates SeTGIN telemetry              │
└─────────────────────────────────────────────┘
        │
        │ 3️⃣  Validated request → Knowledge lookup
        ▼
┌─────────────────────────────────────────────┐
│  SynaGraph – Knowledge Engine               │
│  • Graph + vector + temporal storage        │
│  • Provenance & supersede logic             │
│  • Temporal decay + reinforcement scoring   │
│  • Publishes graph events (NATS/Kafka)      │
└─────────────────────────────────────────────┘
        │
        │ 4️⃣  Metrics + events → Optimization
        ▼
┌─────────────────────────────────────────────┐
│  SeTGIN – Self-Tuning Intelligence Network  │
│  • Collects latency / recall metrics        │
│  • Adjusts PerfPolicy (top_k, τ, λ, etc.)   │
│  • Federated learning across tenants        │
│  • Feedback loop → Knowlemesh registry      │
└─────────────────────────────────────────────┘
        │
        │ 5️⃣  Optional re-inference on miss
        ▼
┌─────────────────────────────────────────────┐
│  GPU / LLM Cluster – Fallback Layer         │
│  • Generates new answers when unknown       │
│  • Results stored → SynaGraph → Scedge      │
└─────────────────────────────────────────────┘
```

### Summary of Flow

1. **Client / Agent** sends query → Scedge tries semantic lookup.  
2. **Scedge** serves hit instantly; on miss forwards capsule to Knowlemesh.  
3. **Knowlemesh** validates policy and routes to SynaGraph.  
4. **SynaGraph** executes graph/vector retrieval; emits events.  
5. **SeTGIN** analyzes telemetry and tunes future performance.  
6. **GPU / LLM Cluster** handles only truly novel or low-confidence requests.


---

## 4. Core Components

### 4.1 SynaGraph

| Aspect | Detail |
|--------|---------|
| **Language** | Rust |
| **APIs** | gRPC + REST |
| **Responsibilities** | Graph persistence, vector similarity, temporal decay/reinforcement, provenance tracking |
| **Data Stores** | RocksDB (local), PostgreSQL (metadata), Redis (cache) |
| **Events** | Emits `UPSERT`, `SUPERSEDED_BY`, `REVOKE_CAPSULE` over NATS/Kafka |
| **Integration** | Subscribes to SeTGIN policy updates and Knowlemesh control signals |

---

### 4.2 Knowlemesh

| Aspect | Detail |
|--------|---------|
| **Language** | Ruby on Rails (API-first) |
| **Role** | Multi-tenant control plane for orchestration, compliance, and lifecycle automation |
| **Functions** | Tenant registry, RBAC, policy engine, audit, billing, analytics |
| **Endpoints** | `/api/v1/policies`, `/api/v1/capsules`, `/api/v1/nodes` |
| **Federation Services** | Discovery registry, node PKI management, SeTGIN aggregation |
| **UI / Cloud Offering** | Dashboards, analytics, compliance reports |

---

### 4.3 Scedge

| Aspect | Detail |
|--------|---------|
| **Language** | Rust (Axum / Tokio) |
| **Purpose** | Smart cache and knowledge CDN for sub-50ms responses |
| **Features** | Semantic key caching, ANN near-duplicate detection, policy enforcement |
| **APIs** | `/lookup`, `/store`, `/purge` |
| **Storage** | Redis or SQLite (edge), optional RocksDB for persistence |
| **Security** | Tenant JWT / HMAC validation, policy tagging, signed artifacts |
| **Integration** | Subscribes to SynaGraph events for invalidation; exports metrics to SeTGIN |

---

### 4.4 SeTGIN

| Aspect | Detail |
|--------|---------|
| **Language** | Rust + optional Python RL modules |
| **Purpose** | Adaptive intelligence loop that tunes performance dynamically |
| **Inputs** | Telemetry from SynaGraph (query metrics) + Scedge (edge metrics) |
| **Outputs** | Updated `PerfPolicy` objects applied per tenant and propagated via Knowlemesh |
| **Methods** | PID-like control, reinforcement learning, federated averaging |
| **Security** | Signed policy updates, rollback protection, audit trail |

---

### 4.5 Context Capsule Protocol (CCP)

| Aspect | Detail |
|--------|---------|
| **Object** | Capsule (summary, refs, embeddings, provenance, policy, signature) |
| **Transport** | `/ccp/v0/capsules` (HTTP / gRPC) |
| **Operations** | Export, Fetch, Query, Revoke |
| **Encodings** | v0.1 JSON, roadmap → CBOR / Protobuf |
| **Usage** | Enables AI→AI and node→node context sharing within the mesh |
| **Governance** | Versioned spec maintained under Memophor Open Standards |

---

## 5. Shared Architectural Concerns

| Concern | Approach |
|----------|-----------|
| **Contracts & Schemas** | Shared protobuf + JSON-Schema repo under `proto/` |
| **Observability** | OpenTelemetry traces, Prometheus metrics, health probes |
| **Policy Enforcement** | WASM-compiled policy engine for multi-language enforcement |
| **Authentication** | JWT + HMAC + tenant-scoped PKI |
| **Encryption** | AES-GCM for data at rest, TLS for in-transit |
| **Testing & CI** | GitHub Actions + container integration tests across services |

---

## 6. Deployment Model

| Layer | Deployment | Notes |
|-------|-------------|-------|
| **Edge (Scedge PoPs)** | Deployed to regional cloud edges or on-prem appliances | Nearest-user PoPs cache knowledge artifacts |
| **Regional Cache** | Aggregates PoPs within same geography | Syncs via message bus |
| **Knowlemesh Cloud** | SaaS or enterprise control plane | Handles registry, policy, analytics |
| **SynaGraph Nodes** | Self-hosted or managed clusters | Primary data storage |
| **SeTGIN** | Runs as background service within each node | Reports telemetry upstream |
| **GPU/LLM Backends** | Any inference provider | Optional fallback path |

---

## 7. Data Contracts and Event Flow

1. **Query:** Client → Scedge `/lookup`  
   - If hit → return artifact.  
   - If miss → forward to Knowlemesh.

2. **Inference:** Knowlemesh validates → SynaGraph executes query → optional GPU call.

3. **Storage:** SynaGraph stores result → emits `UPSERT` event.

4. **Propagation:** Knowlemesh distributes to relevant Scedge PoPs; SeTGIN logs metrics.

5. **Optimization:** SeTGIN updates `PerfPolicy`; new parameters applied to SynaGraph and PoPs.

6. **Invalidation:** On `SUPERSEDED_BY` or `REVOKE_CAPSULE`, Scedge purges related artifacts.

---

## 8. Architectural Goals

| Goal | Description |
|------|--------------|
| **Scalability** | Horizontal federation; no single central router. |
| **Low Latency** | Edge responses < 50 ms for known knowledge. |
| **Self-Optimization** | SeTGIN adapts graph parameters automatically. |
| **Compliance** | Tenant isolation + auditable policy enforcement. |
| **Resilience** | Eventual consistency with replayable logs. |
| **Interoperability** | Open standards (CCP, MCP, Protobuf). |

---

## 9. Future Enhancements

- **Dynamic Federation Protocol (DFP):** peer discovery and trust scoring.  
- **Zero-Trust Capsules:** encrypted CCP with proof-of-access.  
- **Edge Learning:** small models trained on cached answers.  
- **SeTGIN v2:** deep-RL controller for predictive graph tuning.  
- **Global Knowledge Mesh:** cross-organization federation and analytics.  

---

### ✨ Summary

Memophor’s architecture unifies **storage**, **orchestration**, **edge delivery**, and **adaptive intelligence** into a single cohesive system.  
Through SynaGraph, Scedge, Knowlemesh, SeTGIN, and CCP, the platform becomes a **self-optimizing knowledge mesh**—capable of remembering, reasoning, and evolving with every query.

> *“Memophor turns memory into infrastructure.”*
