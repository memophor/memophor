# Open-Core Strategy Overview

## Strategic Positioning
- **Memophor** operates an open-core model: foundational memory infrastructure is open-source, commercial services deliver enterprise orchestration, compliance, and SLAs.
- **SynaGraph** remains fully open-source (Apache-2.0) to drive community adoption and interoperability.
- **Knowlemesh** delivers the commercial control plane (Rails) and bundled cloud services.

## Scedge Licensing Decision
- **Scedge Core (OSS)**
  - License: Apache-2.0.
  - Scope: single-PoP deployment, semantic lookup/store/purge APIs, template rendering, policy-aware caching, basic metrics.
  - Rationale: Encourage edge experimentation, build trust in cache correctness, grow ecosystem integrations.
- **Scedge Enterprise (Commercial)**
  - License: Memophor commercial agreement.
  - Scope: multi-region orchestration, private PoPs, advanced analytics, tenant isolation tooling, hardened compliance integrations, managed SLA.
  - Rationale: Preserves differentiation, aligns revenue with mission-critical features, funds OSS development.

## Community vs. Enterprise Breakdown
| Capability | OSS | Enterprise |
|------------|-----|------------|
| Single PoP edge cache | ✅ | ✅ |
| Semantic TTL + policy metadata | ✅ | ✅ |
| Context Capsule hydration | ✅ | ✅ |
| Multi-region federation | ⛔ | ✅ |
| Private tenant PoPs | ⛔ | ✅ |
| Global analytics dashboards | ⛔ | ✅ |
| SLA-backed support | ⛔ | ✅ |
| Compliance pack (HIPAA/GDPR tooling) | ⛔ | ✅ |

## Governance Principles
- Maintain an active public roadmap and contribution guide for Scedge.
- Clearly publish feature matrix differentiating OSS vs. enterprise capabilities.
- Keep protocol-level standards (CCP/CCS) open to ensure ecosystem trust.
- Use Memophor Cloud to offer hosted Scedge + Knowlemesh bundles with managed operations.

## Next Actions
1. Publish Scedge OSS repo with Apache-2.0 license and contribution guidelines.
2. Draft enterprise feature matrix for sales/marketing collateral.
3. Automate release cadence (OSS core monthly; enterprise quarterly with LTS support).
4. Announce open-core stance alongside CCP v0.1 spec release.
