# 💡 Memophor Stack — Idea & Architecture Document
> Revision 2025-10-18

---

## 1. Vision
Memophor builds infrastructure that gives AI systems **memory and continuity**.  
Instead of stateless model calls, every AI interacts with a **Knowledge Mesh** that remembers, connects, and reuses information.

> “From data → memory → understanding.”

---

## 2. Core Components

| Layer | Name | Description |
|-------|------|-------------|
| **Company** | **Memophor** | Parent organization; publishes open standards and operates cloud. |
| **Platform** | **Knowlemesh** | Distributed *Knowledge Mesh* (multi-tenant, temporal, semantic memory platform). |
| **Open Source Core** | **SynaGraph** | OSS *Synaptic Graph Engine* handling graph + vector + temporal storage. |
| **Edge Layer** | **Scedge** | *Smart Cache on the Edge* — a knowledge-aware CDN that bypasses GPU inference when answers are known. |
| **Protocol** | **CCP (Context Capsule Protocol)** | Open standard for packaging and transferring context between AIs. |

---

## 3. System Overview

```
Client / Agent
↓
Scedge (Edge PoP)
↓
Knowlemesh (Control Plane)
↓
SynaGraph (Knowledge / Memory Core)
↓
GPU / LLM Cluster → only on cache miss
```

- Scedge answers from edge cache using semantic + policy keys.  
- Knowlemesh orchestrates policies, tenants, analytics.  
- SynaGraph stores knowledge nodes, embeddings, and temporal weights.  
- LLM is used only when knowledge doesn’t yet exist.

---

## 4. Context Capsule Protocol (CCP)

### Purpose
A portable, signed JSON/Binary object that allows AIs to **hand off understanding**.  
Think of it as the “TCP/IP” for AI memory exchange.

### Capsule (simplified)
```json
{
  "ccp_version": "0.1",
  "capsule_id": "ctx-2025-10-18-001",
  "origin": {"agent_id":"agent-a","system":"knowlemesh"},
  "tenant": "acme-health",
  "scope": ["project:x"],
  "summary": "Resolved claim appeal ...",
  "graph_refs": ["node123","node124"],
  "policy": {"phi":true,"ttl_sec":86400,"roles":["agent"]},
  "signature": {"alg":"ed25519","kid":"tenant-acme","sig_b64":"..."}
}
```

### Core Operations

| Method | Description |
|--------|-------------|
| `POST /capsules` | Export capsule (agent → mesh). |
| `GET /capsules/{id}` | Fetch capsule (mesh → agent). |
| `POST /capsules/query` | Search capsules by scope/time. |
| `POST /capsules/{id}/revoke` | Revoke and purge. |

---

## 5. Encoding Strategy

Start with JSON (canonical) for readability and OSS adoption. Plan upgrade path:

1. JSON / JSON-Schema (v0.1)
2. CBOR or MessagePack (v0.2)
3. Protobuf / Cap’n Proto (v1.0)

Capsules include:

- `Content-Type: application/ccp+json`
- `Accept: application/ccp+cbor`

---

## 6. Technology Choices

| Component | Language | Notes |
|-----------|----------|-------|
| Scedge (Edge) | Rust | Axum/Hyper, Redis, hnswlib, Prometheus. |
| SynaGraph (Core) | Rust | RocksDB, Tantivy, FAISS/HNSW, gRPC. |
| Knowlemesh (Control Plane) | Ruby on Rails | Auth, tenants, billing, dashboards. |
| CCP SDKs | Rust, Python, Ruby | Capsule build/verify/export/import helpers. |
| GPU Layer | Any LLM provider | Used only on cache miss; write-back result. |

---

## 7. Scedge — Smart Cache on the Edge

Goal: reduce redundant GPU processing by caching known answers at the network edge.

### Key features

- Semantic + policy + tenant keying.
- Graph-aware invalidation (SUPERSEDED_BY events).
- Zero-token personalization (templates at edge).
- Latency 20–50 ms p50; token cost ↓ 70–95 %.

### APIs

- `GET /lookup`
- `POST /store`
- `POST /purge`

### Artifact example
```json
{
  "answer": "text or template",
  "policy": {"tenant":"acme","phi":true},
  "provenance": [{"source":"doc://handbook#42"}],
  "ttl_sec": 259200,
  "hash": "etag-abc123"
}
```

---

## 8. Context Capsule Transfer Flow

1. **Agent A → Mesh**: `POST /capsules` — stores capsule in SynaGraph.
2. **Mesh → Edge**: Mesh warms Scedge with artifact for low-latency access.
3. **Agent B → Mesh/Edge**: `GET /capsules/{id}` — retrieves capsule under policy scope.
4. **Agent B** merges capsule context into its prompt or local memory.

Result → smooth, secure AI-to-AI context transfer without re-processing.

---

## 9. Security / Compliance

- Tenant-scoped JWTs & KMS encryption.
- Role-based policy enforcement (HIPAA, GDPR).
- Provenance tracking and auditable capsule signatures.
- Default “no cache” for PHI/PII unless tenant approved.

---

## 10. Open-Source vs Enterprise

| Component | License | Purpose |
|-----------|---------|---------|
| SynaGraph | Apache-2.0 | Core engine, community contributions. |
| Scedge (core) | Apache-2.0 | Basic PoP features. |
| Knowlemesh / Cloud tools | Commercial | Multi-region orchestration, analytics, SLAs. |

---

## 11. Future Roadmap

| Milestone | Focus | ETA |
|-----------|-------|-----|
| v0.1 OSS | SynaGraph + Scedge PoC + CCP JSON spec | Q1 2026 |
| v0.2 Cloud | Multi-tenant control plane, dashboards | Q2 2026 |
| v1.0 Fabric | Federation, CCP v1 (binary), enterprise pilots | 2027 |

---

## 12. Quick Summary

- **Memophor**: the company and vision.
- **Knowlemesh**: distributed Knowledge Mesh.
- **SynaGraph**: open-source synaptic graph engine.
- **Scedge**: smart context-aware cache at the edge.
- **CCP**: Context Capsule Protocol — the new standard for inter-AI memory exchange.

Together they create a self-optimizing memory fabric that lets intelligent systems remember, reason, and collaborate.

---

Use this file as high-level context for Copilot CLI/Codex or future design documents.
