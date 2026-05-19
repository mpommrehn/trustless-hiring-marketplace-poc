# 📋 Project Charter — Trustless Hiring Marketplace POC

> **Version**: 1.0
> **Date**: May 18, 2026
> **Status**: Active

---

## Project Identity

| Field | Value |
|-------|-------|
| **Name** | Trustless Hiring Marketplace POC |
| **Type** | Proof of Concept / Portfolio Project |
| **Repository** | [github.com/mpommrehn/trustless-hiring-marketplace-poc](https://github.com/mpommrehn/trustless-hiring-marketplace-poc) |
| **License** | MIT |
| **Context** | "Beat Claude" Portfolio Challenge |

---

## Mission Statement

Demonstrate that applicant-recruiter-company interactions can be **transparent, privacy-respecting, and trustless** via cryptography, while providing **superior AI-based job-candidate matching** — and document the entire journey from concept to working prototype.

---

## Goals

### Primary Goals

1. **Build a working two-sided hiring marketplace** with job posting, candidate profiles, and intelligent matching
2. **Design and mock ZK identity verification** interfaces that demonstrate privacy-preserving verification
3. **Implement a multi-criteria matching algorithm** that goes beyond keyword matching (the unique value proposition)
4. **Document every decision** in an evolution log that showcases engineering judgment

### Secondary Goals

5. **Create portfolio-quality documentation** that demonstrates technical communication skills
6. **Establish clean abstraction boundaries** so mock ZK can be swapped for real ZK later
7. **Build with production patterns** (proper auth, error handling, testing) even in a POC
8. **Demonstrate full-stack capability** across database, API, frontend, and cryptographic protocol design

### Non-Goals (Explicitly Out of Scope)

- ❌ Production deployment or scaling
- ❌ Real ZK proof generation (mock interfaces only for POC)
- ❌ Mobile app development
- ❌ Payment processing or escrow implementation
- ❌ Real NFC passport reading (interface designed, not implemented)
- ❌ Blockchain integration (designed for, not implemented)

---

## Success Criteria

### Must Have (POC is incomplete without these)

| # | Criterion | Measurable Target |
|---|-----------|-------------------|
| S1 | Job posting CRUD | Create, read, update, withdraw jobs |
| S2 | Candidate profiles | Create, edit, search profiles |
| S3 | Matching algorithm | Multi-criteria scoring with ≥5 weighted factors |
| S4 | Verification UI | Tier selection + mock verification flow for all 3 user types |
| S5 | Database with seed data | PostgreSQL with ≥50 realistic seed records |
| S6 | API documentation | OpenAPI/Swagger spec for all endpoints |
| S7 | Evolution log | ≥5 phase entries documenting the full journey |

### Should Have (significantly improves the POC)

| # | Criterion | Measurable Target |
|---|-----------|-------------------|
| S8 | In-app messaging | Basic message exchange between matched parties |
| S9 | Match explanations | Human-readable "why this match" for each result |
| S10 | Trust score visualization | Visual representation of verification tier impact |
| S11 | Responsive UI | Works on desktop and tablet viewports |

### Nice to Have (stretch goals)

| # | Criterion | Measurable Target |
|---|-----------|-------------------|
| S12 | Demo video | ≤5 minute walkthrough of key features |
| S13 | One-click deployment | Docker Compose for full stack |
| S14 | Performance benchmarks | Matching algorithm benchmarked at scale |
| S15 | Accessibility | WCAG 2.1 AA compliance on key pages |

---

## Constraints

### Technical Constraints

| Constraint | Impact | Mitigation |
|-----------|--------|------------|
| No real ZK computation | Can't demonstrate actual proofs | Design interfaces that accept real proofs; mock responses match real proof formats |
| Single developer | Limited implementation bandwidth | Phase-based approach with clear priorities |
| POC scope | No production hardening | Document what would change for production in each area |
| No blockchain | Can't demonstrate on-chain verification | Design Solidity interfaces; document deployment strategy |

### Design Constraints

| Constraint | Impact | Mitigation |
|-----------|--------|------------|
| Privacy-first | Every feature must default to minimal data exposure | Review each data flow for PII leakage |
| No secrets in repo | Can't hardcode API keys or credentials | `.env` files with `.gitignore`, `.env.example` for documentation |
| Portfolio quality | Code and docs must be presentable | Code review checklist, documentation standards |

---

## Stakeholders

| Role | Who | Interest |
|------|-----|----------|
| **Owner/Developer** | mpommrehn | Portfolio project, skill demonstration |
| **Evaluators** | Potential employers, collaborators | Code quality, decision-making, communication |
| **Open Source Community** | GitHub visitors | Reusable patterns, ZK research, marketplace architecture |

---

## Timeline

| Phase | Target Duration | Key Milestone |
|-------|----------------|---------------|
| Phase 0: Foundation | ✅ Complete | Repo, research, documentation |
| Phase 1: Architecture | ~1-2 sessions | DB schema, API contracts, component designs |
| Phase 2: MVP | ~2-3 sessions | Working CRUD, auth, basic flows |
| Phase 3: Matching | ~1-2 sessions | Algorithm implementation with explanations |
| Phase 4: Polish | ~1-2 sessions | Messaging, UI refinement, testing |
| Phase 5: Documentation | ~1 session | API docs, demo, deployment |

---

## Risk Register

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| Scope creep (adding real ZK) | Medium | High | Strict mock-first approach; clean interfaces |
| Over-engineering the matching algorithm | Medium | Medium | Start simple (5 criteria), iterate |
| Documentation debt | Low | High | Document as we build, not after |
| ZK research becoming outdated | Low | Low | Date-stamp all research; link to sources |
| Losing momentum between phases | Medium | High | Phase-based approach with clear deliverables |

---

## Technology Decisions

| Decision | Choice | Alternatives Considered | Rationale |
|----------|--------|------------------------|-----------|
| Language | TypeScript | JavaScript, Python | Type safety for complex data models |
| Backend | Node.js + Express | NestJS, Fastify, Django | Simplicity for POC, easy to extend |
| Frontend | React + Next.js | Vue, Svelte, plain React | SSR for marketplace SEO, rich ecosystem |
| Database | PostgreSQL | MongoDB, SQLite | Relational integrity for multi-entity marketplace |
| ZK Protocol | Semaphore + SNARKs | STARKs, Polygon ID | POC feasibility, JS tooling, proof size |
| Matching | Custom weighted algorithm | ML model, Elasticsearch | Explainability, configurability |

---

## Communication

- **Evolution Log**: Primary record of all decisions and progress ([docs/EVOLUTION.md](EVOLUTION.md))
- **GitHub Issues**: Task tracking and bug reports
- **README**: Project overview and current status
- **Code Comments**: In-code documentation for complex logic

---

_This charter is a living document updated as the project evolves. See the [Evolution Log](EVOLUTION.md) for detailed phase-by-phase progress._
