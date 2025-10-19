# Context Capsule Protocol (CCP) / Context Capsule Specification (CCS) v0.1

## 1. Purpose
Context Capsule Protocol (CCP), also referred to as the Context Capsule Specification (CCS), defines a vendor-neutral way for intelligent systems to package, exchange, and consume context. A Capsule is a signed, policy-aware object that allows one agent to hand off understanding to another without recomputing work.

## 2. Scope (v0.1)
- Portable Capsule object: JSON representation with optional binary attachments.
- Minimal HTTP API (`/capsules` surface) with parity gRPC methods.
- Security envelope covering tenancy, roles, TTL, provenance, and signatures.
- Reference flows for export, fetch, query, and revoke operations.

## 3. Core Concepts
### Capsule Object
```json
{
  "ccp_version": "0.1",
  "capsule_id": "ctx-2025-10-18-001",
  "created_at": "2025-10-18T12:34:56Z",
  "origin": { "agent_id": "agent-a", "system": "memophor/knowlemesh" },
  "tenant": "acme-health",
  "scope": ["project:x", "session:y"],
  "summary": "Resolved claim appeal …",
  "graph_refs": ["node:Q123", "node:A987"],
  "chunks": [
    { "type": "text", "id": "c1", "hash": "sha256:…", "bytes_b64": "…" }
  ],
  "embeddings": [
    { "ref": "vec:abc", "model": "text-embedding-3", "dim": 1536 }
  ],
  "policy": {
    "pii": false,
    "phi": true,
    "roles": ["agent", "analyst"],
    "ttl_sec": 604800,
    "shareable": true
  },
  "provenance": [
    { "source": "doc://handbook#42", "hash": "sha256:…", "ts": "2025-10-18T11:00:00Z" }
  ],
  "signature": {
    "alg": "ed25519",
    "kid": "tenant-acme-key-1",
    "sig_b64": "BASE64_SIGNATURE"
  }
}
```

### Field Notes
- `graph_refs` anchor the capsule back into SynaGraph (or another mesh).
- `chunks` are optional inline snippets; large payloads can be served via URIs.
- `embeddings` can be referenced by ID to avoid bloating the capsule.
- `policy` encodes tenant governance, retention, and sharing scope.
- `signature` proves integrity using tenant-managed keys.

## 4. Transport Endpoints (HTTP)
Base path: `/ccp/v0/`

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/capsules` | Export capsule (producer → mesh); validates and stores. |
| `GET` | `/capsules/{capsule_id}` | Fetch capsule content, enforcing policy. |
| `POST` | `/capsules/query` | Search by tenant, scope, time window, limit. |
| `POST` | `/capsules/{capsule_id}/revoke` | Revoke capsule and emit purge events. |
| `GET` | `/capsules` | List capsules via tenant/scope filters with pagination. |

*Equivalent gRPC service methods MUST mirror the semantics above.*

## 5. Security & Policy Envelope
- **Authentication**: Bearer JWT or mTLS; claims include `tenant`, `roles`, `sub`.
- **Authorization**: Enforce capsule `policy.roles`, tenant isolation, TTL expiry.
- **Signatures**: Canonical JSON (RFC 8785) hashed + signed (Ed25519). `kid` resolves to tenant KMS key.
- **Encryption (optional v0.1)**: Envelope encryption per tenant (AES-GCM). `encryption` stanza MAY extend the capsule.
- **Audit**: Log `{who, when, capsule_id, decision, reason}` for compliance (HIPAA/GDPR).

## 6. Encoding Strategy
- Default: `Content-Type: application/ccp+json` (human-readable, OSS-friendly).
- Planned: `application/ccp+cbor` or MessagePack for edge/internal use (v0.2).
- Future: Protobuf/Cap’n Proto schema for high-throughput channels (v1.0).
- Signatures operate over canonical JSON to preserve interoperability across encodings.

## 7. Integration with Memophor Stack
- **SynaGraph**: Persists `(:ContextCapsule)` nodes, links to referenced knowledge nodes, tracks provenance.
- **Knowlemesh**: Enforces tenancy, policy, lifecycle; exposes HTTP/gRPC endpoints; orchestrates revocations.
- **Scedge**: Optionally caches capsules for millisecond hand-offs; obeys TTL/policy; purges on revoke or supersede events.
- **GPU/LLM layer**: Invoked only on cache miss; results stored back as capsules to prevent redundant inference.

## 8. Reference Flows
### 8.1 Export Flow (Agent → Mesh)
1. Agent constructs Capsule (optionally pre-signed).
2. `POST /capsules` validates schema/policy, attaches signature, returns `capsule_id`.
3. Mesh emits optional edge warm-up event to Scedge.

### 8.2 Consume Flow (Agent B)
1. Agent queries via `POST /capsules/query` or directly `GET /capsules/{id}`.
2. Mesh checks auth/policy; returns Capsule.
3. Agent merges `summary`, `graph_refs`, and optional `chunks/embeddings` into its prompt or local memory.

### 8.3 Revoke Flow (Origin/Admin)
1. Origin calls `POST /capsules/{id}/revoke` (or TTL expires).
2. Capsule marked revoked; Knowlemesh emits purge events to Scedge and dependent services.
3. Future fetch attempts return `410 Gone` or policy-specific denial.

## 9. JSON Schema (excerpt)
```json
{
  "$id": "https://ccp.memophor.com/schema/v0.1/capsule.json",
  "type": "object",
  "required": ["ccp_version", "capsule_id", "origin", "tenant", "summary", "policy", "signature"],
  "properties": {
    "ccp_version": {"const": "0.1"},
    "capsule_id": {"type": "string"},
    "created_at": {"type": "string", "format": "date-time"},
    "origin": {
      "type": "object",
      "required": ["agent_id"],
      "properties": {
        "agent_id": {"type": "string"},
        "system": {"type": "string"}
      }
    },
    "tenant": {"type": "string"},
    "scope": {"type": "array", "items": {"type": "string"}},
    "summary": {"type": "string"},
    "graph_refs": {"type": "array", "items": {"type": "string"}},
    "policy": {
      "type": "object",
      "required": ["ttl_sec"],
      "properties": {
        "pii": {"type": "boolean"},
        "phi": {"type": "boolean"},
        "roles": {"type": "array", "items": {"type": "string"}},
        "ttl_sec": {"type": "integer", "minimum": 1},
        "shareable": {"type": "boolean"}
      }
    },
    "signature": {
      "type": "object",
      "required": ["alg", "kid", "sig_b64"],
      "properties": {
        "alg": {"type": "string"},
        "kid": {"type": "string"},
        "sig_b64": {"type": "string"}
      }
    }
  }
}
```

## 10. Versioning & Extension Policy
- `ccp_version` is mandatory; producers/consumers MUST reject unsupported versions.
- New optional fields should use `x_` prefixes to maintain forward compatibility.
- Binary attachments should use multipart uploads or object storage references to avoid oversized JSON bodies.

## 11. Reference Implementations (roadmap)
- **Rust** (`ccp-rs`): Capsule validator, signer, Axum middleware.
- **Python** (`ccp-py`): Client SDK for export/fetch, FastAPI integration.
- **Ruby** (`ccp-rb`): Rails helpers, policy middleware for Knowlemesh.
- **CLI** (`ccp`): `export`, `fetch`, `revoke`, `verify` commands.

## 12. Transfer Summary (Copy/Paste TL;DR)
Context Capsule Protocol (CCP) is the open standard for handing off understanding between AIs. A Capsule packages an agent’s context—summary, references into the knowledge graph, optional snippets/embeddings—inside a signed, policy-scoped JSON object with TTL and tenant/role rules. Authorized agents request a Capsule and continue reasoning without repeating expensive inference.

Within Memophor, SynaGraph persists Capsules, Knowlemesh enforces access and lifecycle, and Scedge can cache Capsules at the edge for millisecond hand-offs. CCP transforms inter-agent context sharing into a secure, auditable first-class operation—the “TCP/IP” of AI memory exchange.
