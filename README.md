<p align="center">
  <h1 align="center">🔐 Trustless Hiring Marketplace</h1>
  <p align="center">
    <strong>A privacy-first, two-sided hiring marketplace with zero-knowledge identity verification and AI-powered multi-criteria matching.</strong>
  </p>
  <p align="center">
    <a href="#-current-status">Status</a> •
    <a href="#-vision">Vision</a> •
    <a href="#-architecture">Architecture</a> •
    <a href="#-getting-started">Getting Started</a> •
    <a href="#-roadmap">Roadmap</a> •
    <a href="docs/EVOLUTION.md">Evolution Log</a>
  </p>
  <p align="center">
    <img src="https://img.shields.io/badge/Phase-0%20Foundation-blue?style=flat-square" alt="Current Phase" />
    <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="License" />
    <img src="https://img.shields.io/badge/ZK-SNARKs-purple?style=flat-square" alt="ZK Protocol" />
    <img src="https://img.shields.io/badge/Status-Research%20%26%20Design-orange?style=flat-square" alt="Status" />
  </p>
</p>

---

## 📋 Current Status

> **Phase 0: Foundation & Research** — Requirements gathering, ZK protocol selection, and documentation scaffolding complete.

This project is being built live — from concept to working prototype — with every decision, trade-off, and iteration tracked in the [Evolution Log](docs/EVOLUTION.md). It demonstrates not just the final product, but the _process_ of building a complex system from scratch.

---

## 🎯 Vision

### The Problem

The hiring industry is fundamentally broken:

- **Candidates** have no privacy — their data is harvested, shared without consent, and used against them
- **Recruiters** can't verify candidate credentials without invasive background checks
- **Companies** leak strategic hiring plans to competitors when posting roles publicly
- **Everyone** wastes time on mismatches because there's no intelligent filtering
- **Trust** is nonexistent — fake resumes, ghost jobs, and data breaches are the norm

### The Solution

A **trustless hiring marketplace** where:

1. **Zero-Knowledge Identity Verification** — Candidates prove credentials (degrees, certifications, employment history) without revealing the underlying data. Companies verify recruiter authority without exposing org charts.

2. **Multi-Criteria Smart Matching** — Goes beyond keyword matching. Evaluates technical skills, culture fit indicators, growth trajectory, compensation alignment, and work-style preferences using a weighted, configurable algorithm.

3. **Privacy-Preserving Interactions** — Candidates and companies interact through verified but pseudonymous profiles. Real identities are revealed only when both parties consent.

4. **Tiered Verification** — Progressive trust levels from basic email verification to full NFC passport-backed ZK proofs, giving users control over how much they verify.

---

## 🏗️ Architecture

### High-Level System Design

```
┌─────────────────────────────────────────────────────────────────┐
│                        Frontend (React/Next.js)                  │
│   Candidate Portal  │  Recruiter Dashboard  │  Company Admin    │
└──────────┬──────────┴───────────┬───────────┴──────────┬────────┘
           │                      │                       │
           ▼                      ▼                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                         API Gateway (Express)                    │
│   Auth  │  Matching Engine  │  Messaging  │  Verification       │
└────┬────┴────────┬──────────┴──────┬──────┴──────────┬──────────┘
     │             │                  │                 │
     ▼             ▼                  ▼                 ▼
┌─────────┐ ┌───────────┐ ┌──────────────┐ ┌────────────────────┐
│ Auth DB │ │ PostgreSQL │ │  Message Bus  │ │  ZK Verification   │
│ (JWT)   │ │ (Primary)  │ │  (Events)     │ │  Service (Mock→Real)│
└─────────┘ └───────────┘ └──────────────┘ └────────────────────┘
```

### Technology Stack

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| **Frontend** | React + Next.js | SSR for SEO, component-based for marketplace UI |
| **Backend** | Node.js + Express | JavaScript ecosystem consistency, async I/O for matching |
| **Database** | PostgreSQL | Relational integrity for multi-entity marketplace |
| **ZK Proofs** | zk-SNARKs (Semaphore) | Smaller proofs, faster verification, simpler integration |
| **Auth** | JWT + ZK Credentials | Hybrid: traditional auth + ZK verification layers |
| **Matching** | Custom weighted algorithm | Multi-criteria with configurable weights |

### ZK Protocol Selection

We selected **zk-SNARKs with Semaphore** over alternatives after evaluating:

| Criteria | zk-SNARKs | zk-STARKs | Polygon ID |
|----------|-----------|-----------|------------|
| Proof Size | ~288 bytes ✅ | ~45-200 KB | ~1-2 KB |
| Verification Speed | ~10ms ✅ | ~50ms | ~20ms |
| Trusted Setup | Required ⚠️ | Not needed | Not needed |
| POC Complexity | Low ✅ | Medium | High |
| Ecosystem Maturity | High ✅ | Growing | Growing |

> Full protocol comparison: [`docs/research/zk-identity-protocols.md`](docs/research/zk-identity-protocols.md)

---

## 📁 Project Structure

```
trustless-hiring-marketplace-poc/
├── docs/
│   ├── architecture/          # System design documents
│   ├── evolution-log/         # Phase-by-phase build diary
│   │   └── 00-inception.md    # Requirements gathering & decisions
│   ├── research/              # Protocol comparisons & analysis
│   │   ├── zk-identity-protocols.md
│   │   └── verification-tiers.md
│   ├── EVOLUTION.md           # Master evolution log
│   └── PROJECT-CHARTER.md     # Goals, constraints, success criteria
├── src/                       # Application source code (Phase 2+)
├── tests/                     # Test suites (Phase 2+)
├── .gitignore
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

---

## 🚀 Getting Started

> ⚠️ **Phase 0** — The codebase is currently in the research and documentation phase. Implementation begins in Phase 2.

### Prerequisites

- Node.js 18+ (when implementation begins)
- PostgreSQL 15+
- Git

### Explore the Documentation

```bash
git clone https://github.com/mpommrehn/trustless-hiring-marketplace-poc.git
cd trustless-hiring-marketplace-poc

# Start with the evolution log to understand the journey
open docs/EVOLUTION.md

# Dive into ZK protocol research
open docs/research/zk-identity-protocols.md

# Review the project charter
open docs/PROJECT-CHARTER.md
```

---

## 🗺️ Roadmap

| Phase | Focus | Status |
|-------|-------|--------|
| **Phase 0** | Foundation & Research | ✅ Complete |
| **Phase 1** | Core Architecture & Design | 🔲 Planned |
| **Phase 2** | MVP Implementation | 🔲 Planned |
| **Phase 3** | Smart Matching Engine | 🔲 Planned |
| **Phase 4** | Communication & Polish | 🔲 Planned |
| **Phase 5** | Documentation & Portfolio | 🔲 Planned |

### Phase Highlights

- **Phase 1**: Database schema, API contracts, component hierarchy, ZK interface design
- **Phase 2**: Core CRUD operations, authentication, job posting/withdrawal, candidate profiles
- **Phase 3**: Multi-criteria matching algorithm, weighted scoring, match explanations
- **Phase 4**: In-app messaging, notifications, UI polish, accessibility
- **Phase 5**: API docs, deployment guide, demo video, portfolio presentation

---

## 🤝 Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to contribute to this project.

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [Semaphore Protocol](https://semaphore.pse.dev/) — Privacy-preserving signaling on Ethereum
- [snarkjs](https://github.com/iden3/snarkjs) — zkSNARK implementation in JavaScript
- [circom](https://github.com/iden3/circom) — Circuit compiler for ZK proofs
- Built as part of the **"Beat Claude" Portfolio Challenge** — demonstrating the full evolution from concept to prototype

---

<p align="center">
  <sub>Built with 🔐 privacy-first principles • Every decision documented • From concept to prototype</sub>
</p>
