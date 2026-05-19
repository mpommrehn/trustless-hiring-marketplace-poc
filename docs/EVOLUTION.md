# 📖 Evolution Log — Trustless Hiring Marketplace

> Tracking every decision, iteration, and lesson learned from concept to prototype.

This is the master index of the project's evolution. Each phase has a detailed log documenting user inputs, AI-assisted research, decisions made, and build results.

---

## Why an Evolution Log?

Most portfolios show the final product. This project shows the **process**:

- How requirements were gathered and refined
- What trade-offs were considered and why
- Where we changed direction and what we learned
- How the system evolved from idea to working code

This is not just documentation — it's a demonstration of **engineering judgment**.

---

## Phase Index

| Phase | Title | Status | Log Entry | Key Decisions |
|-------|-------|--------|-----------|---------------|
| **0** | Foundation & Research | ✅ Complete | [00-inception.md](evolution-log/00-inception.md) | ZK protocol selection, verification tier design, tech stack |
| **1** | Core Architecture & Design | 🔲 Planned | _Pending_ | DB schema, API contracts, component hierarchy |
| **2** | MVP Implementation | 🔲 Planned | _Pending_ | Auth, CRUD, job lifecycle, candidate profiles |
| **3** | Smart Matching Engine | 🔲 Planned | _Pending_ | Matching algorithm, scoring, explanations |
| **4** | Communication & Polish | 🔲 Planned | _Pending_ | Messaging, notifications, UI refinement |
| **5** | Documentation & Portfolio | 🔲 Planned | _Pending_ | API docs, demo video, deployment |

---

## Decision Registry

A quick-reference table of all major decisions across phases.

| # | Decision | Options Considered | Chosen | Phase | Rationale |
|---|----------|--------------------|--------|-------|-----------|
| D1 | ZK Protocol | zk-SNARKs, zk-STARKs, Polygon ID, Semaphore | Semaphore + SNARKs | 0 | Best POC feasibility, smallest proofs, JS-native |
| D2 | Verification Model | Flat (single tier), Progressive tiers, Binary (verified/not) | Progressive tiers | 0 | Incentivizes verification without gatekeeping |
| D3 | Tech Stack | Node.js, Python/Django, Go | Node.js + PostgreSQL + Next.js | 0 | JS ecosystem consistency, best ZK tooling |
| D4 | Matching Approach | ML model, Keyword matching, Weighted multi-criteria | Weighted multi-criteria | 0 | Explainable, configurable, portfolio-friendly |
| D5 | ZK Implementation | Real ZK first, Mock → Real, Mock only | Mock → Real (interface-first) | 0 | Focus on marketplace mechanics, clean abstraction boundary |

---

## Research Index

| Document | Topic | Key Finding |
|----------|-------|-------------|
| [zk-identity-protocols.md](research/zk-identity-protocols.md) | ZK protocol comparison | Semaphore + Groth16 is optimal for POC |
| [verification-tiers.md](research/verification-tiers.md) | Verification architecture | 3-tier progressive model for all user types |

---

## Changelog

### Phase 0 — May 18, 2026

- ✅ Repository created (public)
- ✅ Project structure scaffolded
- ✅ ZK protocol research completed
- ✅ Verification tier architecture designed
- ✅ Project charter established
- ✅ Evolution log initialized
- ✅ Contributing guidelines and MIT license added

---

_This log is updated with each phase. Start with [Phase 0: Inception](evolution-log/00-inception.md) to follow the journey from the beginning._
