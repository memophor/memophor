# Memophor — The Federated Knowledge Mesh for AI

Memophor is building an open knowledge platform that keeps organizational intelligence discoverable, contextual, and governed across time.  
This repository is the entry point for the Memophor open-source ecosystem: it curates project documentation, highlights active codebases, and explains how to get involved.

---

## 🧠 Ecosystem at a Glance

| Component | Description | Status |
|------------|--------------|---------|
| [**synagraph**](https://github.com/memophor/synagraph) | Core *synaptic graph engine* that powers knowledge storage, temporal reinforcement, and policy-aware provenance. | Active – OSS (Apache-2.0) |
| [**scedge**](https://github.com/memophor/scedge) | *Smart Cache on the Edge* — edge delivery and caching layer that serves low-latency knowledge responses while enforcing policy-aware invalidation. | Concept → Prototype |
| [**setgin**](https://github.com/memophor/setgin) | *Self-Tuning Graph Intelligence Network* — adaptive intelligence layer that learns from telemetry to optimize graph and cache performance in real time. | Concept Paper Drafted |
| **knowlemesh** (commercial) | Orchestration layer exposing unified APIs, tenancy, analytics, and lifecycle automation for SynaGraph clusters. | Commercial (Enterprise) |
| [**docs/**](docs) | Architecture notes, strategy briefs, concept papers, and roadmap material for the platform. | Updated |

Each codebase publishes its own README with setup instructions, tooling, and contribution guidance.  
Start with `synagraph` for the foundational storage engine, then explore `scedge` and `setgin` for advanced edge and adaptive intelligence capabilities.

---

## 📚 Documentation & Learning Paths

- [Architecture Overview](docs/architecture.md) — High-level system view and service boundaries.  
- [Technical Blueprint](docs/memophor-technical-blueprint.md) — Deep dive into components, data flows, and platform guarantees.  
- [Open Core Strategy](docs/open-core-strategy.md) — How we balance open foundations with commercial offerings.  
- [Roadmap](docs/roadmap.md) — Near-term priorities and longer-term milestones.  
- [Concept Exploration](docs/IDEA_MEMOPHOR_STACK.md) — Early explorations and future-looking concepts.  

**Recent Concept Papers**
- [Federated Knowledge Mesh](docs/FEDERATED_MESH_CONCEPT.md)  
- [Scedge: Smart Cache on the Edge](docs/SCEDGE_CONCEPT_PAPER.md)  
- [SeTGIN: Self-Tuning Graph Intelligence Network](docs/SETGIN_CONCEPT_PAPER.md)  
- [Research Summary](docs/RESEARCH_SUMMARY.md)

We recommend starting with the **Architecture Overview** and **Federated Knowledge Mesh**, then reading the **Scedge** and **SeTGIN** papers to understand the advanced research direction.

---

## 🧩 Research & Writing Progress

| Area | Deliverable | Progress |
|-------|--------------|----------|
| **Federated Knowledge Mesh** | Concept paper defining decentralized memory architecture. | ✅ Completed |
| **Context Capsule Protocol (CCP)** | Spec for secure context transfer between AIs. | 🧱 In progress |
| **Scedge (Smart Cache on the Edge)** | Concept paper + performance model draft. | ✅ Completed |
| **SeTGIN (Self-Tuning Graph Intelligence Network)** | Concept paper + architecture spec. | ✅ Completed |
| **Research Summary** | Unified view of all active research tracks. | ✅ Completed |
| **Knowlemesh Orchestration** | Enterprise architecture design & control plane research. | 🧱 Ongoing |
| **SynaGraph OSS** | Core engine + temporal memory enhancements. | 🚧 Active Development |

---

## 🚀 Getting Started

1. **Clone the repos you need.**  
   Grab this hub repository for documentation and the specific service repository you intend to work on (e.g., `synagraph`).

2. **Install prerequisites per repo.**  
   Follow the setup steps in each repository’s README (language toolchains, linters, tests).

3. **Review the architecture.**  
   Align your changes with system design, data contracts, and policy requirements.

4. **Run tests locally.**  
   Use the provided Makefiles or scripts before opening a pull request.

---

## 🤝 Contributing

We welcome issues, discussions, docs improvements, and code contributions.

- **Cross-project topics:** Use this hub’s issue tracker to propose ideas, ask questions, or coordinate work that spans multiple components.  
- **Component-specific work:** File issues and PRs in the relevant code repo.  
- **Design proposals:** For major changes, start with a lightweight design doc so maintainers can align early.  
- **Pull requests:** Keep changes focused, include tests, and reference related issues.  
- **Documentation contributions:** Update or add docs alongside code to keep the Knowledge Mesh cohesive and current.

If you’re new to the ecosystem, documentation or tooling improvements are great starting points before diving into core graph or orchestration work.

---

## 🧑‍🤝‍🧑 Community & Governance

- **Maintainers:** The Memophor core team stewards the ecosystem and reviews contributions.  
- **Releases:** Tagged releases occur in component repos; this hub tracks announcements and roadmap updates.  
- **Meetups & Updates:** Community calls and release notes will be published here.  
  Watch the repository to stay informed.

Interested in becoming a maintainer or leading an initiative? Open an issue so we can coordinate.

---

## ⚖️ Licenses

Open-source components specify licenses in their respective repositories (`synagraph` is Apache-2.0; others specify in-repo).  
Commercial offerings such as `knowlemesh` follow Memophor’s enterprise licensing.

---

### 🧩 Summary

Memophor’s ecosystem now includes:
- **SynaGraph** – the core knowledge engine  
- **Scedge** – the smart edge cache layer  
- **SeTGIN** – the self-optimizing intelligence network  
- **Knowlemesh** – orchestration and governance  

Together, these form the foundation of a **Federated Knowledge Mesh** that can remember, reason, and adapt across time.

---

Thanks for exploring and contributing to **Memophor** — we’re building the future of AI memory infrastructure together.
