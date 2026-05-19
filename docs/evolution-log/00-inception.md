# Phase 0: Inception — Requirements Gathering & Foundation

> **Date**: May 18, 2026
> **Phase**: 0 — Foundation & Research
> **Status**: ✅ Complete

---

## 📌 Summary

This document captures the initial requirements gathering session that launched the Trustless Hiring Marketplace POC. It records the user's vision, the AI-assisted research process, key decisions made, and the rationale behind each choice.

---

## 🗣️ The Starting Vision

### User's Core Idea

> Build a trustless two-sided hiring marketplace that demonstrates evolution from concept to working prototype — a "Beat Claude" portfolio challenge.

### Key Pain Points Identified

1. **Lack of trust** between candidates and recruiters — fake credentials, ghost jobs, data harvesting
2. **Privacy violations** — candidate data shared without consent across recruiting networks
3. **Poor matching** — keyword-based systems miss nuanced fit (culture, growth trajectory, work-style)
4. **Opacity** — candidates don't know who views their data; companies can't verify recruiter authority
5. **No accountability** — no way to verify claims without invasive, centralized background checks

### Initial Requirements

The user specified a system that should support:

- **Job posting and withdrawal** with lifecycle management
- **Candidate profiles** with privacy-preserving verification
- **Multi-criteria matching algorithm** (the unique value proposition)
- **In-app messaging** between matched parties
- **Mock ZK identity verification** — design real interfaces, mock the cryptography for POC
- **Verification tiers** for candidates, recruiters, and companies
- **PostgreSQL database** with realistic seed data

---

## 🔬 Research & Decision Process

### ZK Protocol Selection

**Question posed**: Which ZK protocol ecosystem should we target for the POC?

**Options evaluated**:

| Protocol | Pros | Cons | POC Suitability |
|----------|------|------|-----------------|
| zk-SNARKs (Groth16) | Tiny proofs (~288B), fast verification (~10ms), mature ecosystem | Requires trusted setup | ✅ Best for POC |
| zk-STARKs | No trusted setup, quantum-resistant | Large proofs (45-200KB), slower verification | ⚠️ Overkill for POC |
| Polygon ID | Full identity framework, W3C VC support | Complex integration, vendor coupling | ❌ Too complex for POC |
| Semaphore | Simple group membership proofs, ETH-native | Limited to specific proof types | ✅ Ideal for identity POC |

**Decision**: **zk-SNARKs with Semaphore protocol**
- Smaller proof sizes = better UX in a web app
- Faster verification = more responsive matching
- Simpler integration = faster to prototype
- Trusted setup concern is acceptable for POC (can migrate to Plonk/STARKs later)

### Verification Architecture

**Question posed**: How should we structure identity verification across the three user types?

**Decision**: A **tiered verification model** where each user type has progressive trust levels:

- **Candidates**: Basic → Enhanced → Full (email → ZK uniqueness → NFC passport)
- **Recruiters**: Corporate vs Independent tracks
- **Companies**: Stealth → Public → Enterprise

> Full details: [`../research/verification-tiers.md`](../research/verification-tiers.md)

### Technical Stack

**Decision**: Node.js + PostgreSQL + React/Next.js

**Rationale**:
- JavaScript/TypeScript ecosystem has the best ZK tooling (snarkjs, circomlib)
- PostgreSQL provides relational integrity needed for multi-entity marketplace
- Next.js offers SSR for SEO (important for a marketplace)
- Single-language stack reduces context switching for a POC

### Matching Algorithm Approach

**Decision**: Custom multi-criteria weighted scoring (not off-the-shelf ML)

**Rationale**:
- Explainable — can show users _why_ a match was recommended
- Configurable — weights can be tuned per user preference
- Transparent — aligns with the "trustless" ethos
- Demonstrable — easier to showcase in a portfolio than a black-box model

---

## 📐 Project Structure Decisions

### Phase-Based Development

The project follows a deliberate phased approach:

| Phase | Focus | Key Deliverables |
|-------|-------|-----------------|
| **0** | Foundation & Research | This document, ZK research, project charter |
| **1** | Core Architecture & Design | DB schema, API contracts, component hierarchy |
| **2** | MVP Implementation | CRUD operations, auth, basic job/candidate flows |
| **3** | Smart Matching Engine | Weighted algorithm, match scoring, explanations |
| **4** | Communication & Polish | Messaging, notifications, UI refinement |
| **5** | Documentation & Portfolio | API docs, demo video, portfolio presentation |

### Evolution Log Convention

Every phase produces an evolution log entry documenting:
- **User inputs** — what was requested
- **AI questions** — clarifications and alternatives explored
- **Decisions made** — with rationale
- **Build results** — what was actually implemented
- **Lessons learned** — what worked, what didn't

---

## ✅ Phase 0 Deliverables

- [x] GitHub repository created (public)
- [x] Project structure scaffolded
- [x] README.md with vision, architecture, and roadmap
- [x] ZK protocol comparison research ([`zk-identity-protocols.md`](../research/zk-identity-protocols.md))
- [x] Verification tier architecture ([`verification-tiers.md`](../research/verification-tiers.md))
- [x] Project charter ([`PROJECT-CHARTER.md`](../PROJECT-CHARTER.md))
- [x] Evolution log initialized (this document)
- [x] Contributing guidelines and license

---

## 🔮 Next Steps (Phase 1)

1. Design PostgreSQL database schema with all entities and relationships
2. Define API contracts (OpenAPI/Swagger) for all endpoints
3. Create component hierarchy for the frontend
4. Design ZK verification interface contracts (mock implementations)
5. Set up development environment (Docker, linting, testing framework)

---

_This document is part of the [Evolution Log](../EVOLUTION.md) — tracking every step from concept to prototype._
