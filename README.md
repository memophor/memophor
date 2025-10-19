# Memophor Open Source Hub

Memophor is building an open knowledge platform that keeps organizational intelligence discoverable, contextual, and governed across time. This repository is the entry point for the Memophor open-source ecosystem: it curates project documentation, highlights active codebases, and explains how to get involved.

## Ecosystem at a Glance

- [**synagraph**](https://github.com/memophor/synagraph) — Core synaptic graph engine that powers knowledge storage, temporal reinforcement, and policy-aware provenance. Licensed Apache-2.0.
- [**scedge**](https://github.com/memophor/scedge) — Edge delivery and caching layer that serves low-latency knowledge responses while enforcing policy-aware invalidation.
- **knowlemesh** (commercial) — Orchestration layer that exposes unified APIs, tenancy, analytics, and lifecycle automation for SynaGraph clusters.
- [**docs/**](docs) — Architecture notes, strategy briefs, design decisions, and roadmap material for the platform.

Each codebase publishes its own README with language-specific setup instructions, tooling, and contribution guidance. Start with `synagraph` for the foundational storage engine, then explore the others as your use case expands.

## Documentation & Learning Paths

- [Architecture Overview](docs/architecture.md) — High-level system view and service boundaries.
- [Technical Blueprint](docs/memophor-technical-blueprint.md) — Deep dive into components, data flows, and platform guarantees.
- [Open Core Strategy](docs/open-core-strategy.md) — How we balance open-source foundations with commercial offerings.
- [Roadmap](docs/roadmap.md) — Near-term priorities and longer-term milestones.
- [Concept Exploration](docs/IDEA_MEMOPHOR_STACK.md) — Early explorations and future-looking concepts.

We recommend reading the architecture overview first, then following the blueprint and roadmap to understand where contributions can have the most impact.

## Getting Started

1. **Clone the repos you need.** Grab this hub repository for documentation and the specific service repository you intend to work on (for example, `synagraph`).
2. **Install prerequisites per repo.** Follow the setup steps in each repository’s README, including language toolchains, linting, and testing utilities.
3. **Review the architecture.** Align your changes with the system design, data contracts, and policy requirements outlined in the documentation above.
4. **Run tests locally.** Use the provided Makefiles or scripts in each repo before opening a pull request.

## Contributing

We welcome issues, discussions, docs improvements, and code contributions.

- **Cross-project topics:** Use this repository’s issue tracker to propose ideas, ask questions, or coordinate work that spans multiple components.
- **Component-specific work:** File issues and pull requests in the corresponding code repository (for example, feature requests in `synagraph`).
- **Design proposals:** For significant changes, start with an issue or lightweight design doc so maintainers can align on approach and impact.
- **Pull requests:** Keep changes focused, include tests where practical, and describe how your work was validated. Reference related issues for traceability.
- **Documentation contributions:** Update or add docs alongside code to keep the knowledge mesh cohesive and up to date.

If you are new to the project, consider starting with documentation or tooling improvements before moving into core graph or orchestration work.

## Community & Governance

- **Maintainers:** The Memophor core team currently stewards the ecosystem and reviews contributions.
- **Releases:** Tagged releases happen in the component repositories; this hub tracks announcements and roadmap updates.
- **Meetups & updates:** We will publish community call schedules and release notes here as they become available. Watch the repository to stay informed.

Interested in becoming a maintainer or leading an initiative? Reach out via an issue so we can coordinate next steps.

## Licenses

Open-source components specify their licenses in their respective repositories (`synagraph` is Apache-2.0, others note their terms in-repo). Commercial offerings such as `knowlemesh` follow Memophor’s commercial licensing.

---

Thanks for exploring the Memophor platform. We look forward to building the future of knowledge infrastructure together.
