# Zero-Knowledge Identity Protocols: Comparison & Recommendations

> **Date**: May 18, 2026
> **Author**: Trustless Hiring Marketplace Team
> **Status**: Research Complete — Recommendation Selected
> **Decision**: zk-SNARKs with Semaphore Protocol

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Why Zero-Knowledge Proofs for Hiring?](#why-zero-knowledge-proofs-for-hiring)
3. [Protocol Deep Dive](#protocol-deep-dive)
   - [zk-SNARKs (Groth16 / PLONK)](#zk-snarks-groth16--plonk)
   - [zk-STARKs](#zk-starks)
   - [Polygon ID (Iden3)](#polygon-id-iden3)
   - [Semaphore Protocol](#semaphore-protocol)
   - [Worldcoin / World ID](#worldcoin--world-id)
4. [Head-to-Head Comparison](#head-to-head-comparison)
5. [Application to Hiring Marketplace](#application-to-hiring-marketplace)
6. [NFC Passport Verification](#nfc-passport-verification)
7. [Recommendation & Rationale](#recommendation--rationale)
8. [Implementation Strategy](#implementation-strategy)
9. [References](#references)

---

## Executive Summary

This document evaluates zero-knowledge proof protocols for use in a trustless hiring marketplace. The goal is to enable candidates, recruiters, and companies to verify credentials and identity claims without revealing sensitive personal data.

**Our recommendation**: Use **zk-SNARKs** (specifically Groth16 for the POC, with a migration path to PLONK) combined with the **Semaphore protocol** for group membership and uniqueness proofs. This combination offers the best balance of proof efficiency, developer experience, and POC feasibility.

For the POC phase, ZK verification interfaces will be **fully designed but mock-implemented**, allowing us to focus on marketplace mechanics while establishing the correct abstraction boundaries for real ZK integration later.

---

## Why Zero-Knowledge Proofs for Hiring?

### Current Problems

| Problem | Example | ZK Solution |
|---------|---------|-------------|
| **Credential fraud** | Fake degrees, inflated titles | Prove you hold a degree from an accredited institution without revealing GPA, dates, or institution name |
| **Privacy invasion** | Background checks expose full history | Prove employment at a Fortune 500 company without revealing which one |
| **Identity theft** | SSN/passport data leaked in breaches | Prove you're a unique human without revealing any identity document |
| **Recruiter fraud** | Unauthorized representatives | Prove you're an authorized recruiter for Company X without revealing your employee ID |
| **Sybil attacks** | One person creating multiple profiles | Prove uniqueness (one person = one profile) without biometric data exposure |

### What ZK Proofs Enable

A zero-knowledge proof allows a **prover** (e.g., a candidate) to convince a **verifier** (e.g., an employer) that a statement is true without revealing _any_ information beyond the truth of the statement itself.

**Properties**:
- **Completeness**: If the statement is true, an honest prover can convince the verifier
- **Soundness**: If the statement is false, no cheating prover can convince the verifier (except with negligible probability)
- **Zero-Knowledge**: The verifier learns nothing beyond the truth of the statement

---

## Protocol Deep Dive

### zk-SNARKs (Groth16 / PLONK)

**Succinct Non-interactive Arguments of Knowledge**

#### How It Works

zk-SNARKs convert computational statements into polynomial equations over elliptic curves. The prover demonstrates knowledge of a satisfying assignment without revealing it.

**Groth16** (2016):
- Most widely deployed SNARK system
- Requires a **circuit-specific trusted setup ceremony** — a one-time process where toxic waste (random values) must be destroyed. If any participant is honest, the setup is secure.
- Produces the smallest proofs in the SNARK family

**PLONK** (2019):
- Uses a **universal trusted setup** — one ceremony works for all circuits up to a certain size
- Slightly larger proofs than Groth16 but much more flexible
- Growing adoption (used by zkSync, Aztec, Mina)

#### Performance Characteristics

| Metric | Groth16 | PLONK |
|--------|---------|-------|
| **Proof Size** | ~128-288 bytes | ~400-600 bytes |
| **Prover Time** | ~2-10 seconds (depends on circuit) | ~5-30 seconds |
| **Verification Time** | ~5-10 ms | ~10-20 ms |
| **Trusted Setup** | Per-circuit | Universal (one-time) |
| **Recursion Support** | Limited | Native |

#### Ecosystem & Tooling

- **circom** — Domain-specific language for writing ZK circuits (Iden3)
- **snarkjs** — JavaScript library for generating and verifying proofs
- **circomlib** — Library of pre-built circuits (hash functions, comparators, etc.)
- **Hardhat/Foundry** — Solidity verifier generation for on-chain verification

#### Strengths for Hiring Marketplace
- ✅ Tiny proof sizes (excellent for web/mobile UX)
- ✅ Fastest verification (responsive marketplace interactions)
- ✅ Mature JavaScript tooling (snarkjs, circomlib)
- ✅ Battle-tested in production (Tornado Cash, Zcash, many DeFi protocols)
- ✅ Largest developer community and documentation

#### Weaknesses
- ⚠️ Trusted setup requirement (manageable with ceremonies or PLONK migration)
- ⚠️ Not quantum-resistant (not a concern for a POC timeline)
- ⚠️ Circuit design requires specialized knowledge

---

### zk-STARKs

**Scalable Transparent Arguments of Knowledge**

#### How It Works

zk-STARKs use hash functions (instead of elliptic curves) and polynomial IOPs. The "transparent" means no trusted setup — all randomness is derived from public data via the Fiat-Shamir heuristic.

#### Performance Characteristics

| Metric | Value |
|--------|-------|
| **Proof Size** | ~45-200 KB (significantly larger) |
| **Prover Time** | ~1-60 seconds |
| **Verification Time** | ~50-100 ms |
| **Trusted Setup** | None required ✅ |
| **Quantum Resistance** | Yes (hash-based) ✅ |

#### Ecosystem & Tooling

- **StarkNet / Cairo** — Smart contract language and L2 (StarkWare)
- **Winterfell** — Rust STARK prover library
- **Miden VM** — STARK-based virtual machine (Polygon)
- Limited JavaScript tooling compared to SNARKs

#### Strengths for Hiring Marketplace
- ✅ No trusted setup (simpler security model)
- ✅ Quantum-resistant (future-proof)
- ✅ Better for large computations (batch verification of many credentials)

#### Weaknesses
- ❌ **Large proof sizes** (45-200KB vs 288 bytes for SNARKs) — poor for web UX
- ❌ **Slower verification** — 5-10x slower than SNARKs
- ❌ **Limited JavaScript tooling** — Cairo/Rust-focused ecosystem
- ❌ **Higher complexity** for a POC project

---

### Polygon ID (Iden3)

**Decentralized Identity with ZK Proofs**

#### How It Works

Polygon ID is a full identity framework built on the Iden3 protocol. It implements W3C Verifiable Credentials with ZK proof generation, allowing holders to selectively disclose attributes from their credentials.

#### Architecture

```
Issuer (e.g., University)  →  Credential  →  Holder's Wallet
                                                    │
                                            ZK Proof Generation
                                                    │
                                                    ▼
                                            Verifier (Marketplace)
```

#### Performance Characteristics

| Metric | Value |
|--------|-------|
| **Proof Size** | ~1-2 KB |
| **Prover Time** | ~3-15 seconds (mobile) |
| **Verification Time** | ~15-25 ms |
| **Trusted Setup** | Not needed (uses Iden3 circuits) |
| **Standards Compliance** | W3C VC, DID ✅ |

#### Ecosystem & Tooling

- **Polygon ID SDK** — Mobile and web SDKs
- **Iden3 circuits** — Pre-built ZK circuits for credential proofs
- **Polygon ID Wallet** — Reference mobile wallet
- **On-chain Issuer** — Smart contract for credential issuance

#### Strengths for Hiring Marketplace
- ✅ Full identity framework (credentials, issuance, verification)
- ✅ W3C standards compliant (interoperable with other identity systems)
- ✅ Mobile wallet support (good UX for candidates)
- ✅ Selective disclosure built-in

#### Weaknesses
- ❌ **High integration complexity** — requires understanding the full Iden3 stack
- ❌ **Vendor coupling** — deeply tied to Polygon ecosystem
- ❌ **Overkill for POC** — we'd spend more time on identity infra than marketplace logic
- ❌ **Blockchain dependency** — requires Polygon network for credential anchoring

---

### Semaphore Protocol

**Privacy-Preserving Signaling on Ethereum**

#### How It Works

Semaphore is a ZK protocol that allows users to:
1. **Prove group membership** without revealing which member they are
2. **Signal once per external nullifier** (preventing double-signaling / Sybil attacks)
3. **Maintain anonymity** within a group while proving they belong to it

This maps perfectly to hiring marketplace needs:
- "Prove you're a verified candidate without revealing your identity"
- "Prove you're an authorized recruiter for this company without revealing who"
- "Ensure one person = one profile (Sybil resistance)"

#### Architecture

```
Identity Commitment = hash(secret, nullifier)
                           │
                    Added to Merkle Tree (group)
                           │
                    ZK Proof: "I know a secret whose
                    commitment is in this tree"
                           │
                    Nullifier prevents double-use
```

#### Performance Characteristics

| Metric | Value |
|--------|-------|
| **Proof Size** | ~288 bytes (Groth16-based) |
| **Prover Time** | ~2-5 seconds (browser) |
| **Verification Time** | ~10 ms |
| **Group Size Support** | Up to 2^30 members |
| **Trusted Setup** | Uses Groth16 (Hermez ceremony) |

#### Ecosystem & Tooling

- **@semaphore-protocol/core** — TypeScript/JavaScript SDK
- **@semaphore-protocol/contracts** — Solidity verifier contracts
- **@semaphore-protocol/proof** — Browser-compatible proof generation
- Active development by PSE (Privacy & Scaling Explorations, Ethereum Foundation)

#### Strengths for Hiring Marketplace
- ✅ **Perfect abstraction** — group membership + uniqueness is exactly what hiring verification needs
- ✅ **Excellent JavaScript SDK** — first-class TypeScript support
- ✅ **Browser-compatible** — proof generation works in web browsers
- ✅ **Active maintenance** — Ethereum Foundation-backed
- ✅ **Simple mental model** — groups, members, signals
- ✅ **Built-in Sybil resistance** — nullifiers prevent duplicate profiles

#### Weaknesses
- ⚠️ Limited to group membership proofs (can't do arbitrary computation)
- ⚠️ Requires Groth16 trusted setup (uses Hermez ceremony — well-established)
- ⚠️ Primarily Ethereum-focused (but proof verification is chain-agnostic)

---

### Worldcoin / World ID

**Proof of Personhood via Iris Scanning**

#### How It Works

Worldcoin uses iris biometrics to generate a unique identity commitment, then provides ZK proofs of uniqueness ("I am a unique human") without revealing biometric data.

#### Relevance to Hiring Marketplace

| Aspect | Assessment |
|--------|-----------|
| **Unique human verification** | Excellent — strongest Sybil resistance |
| **Credential verification** | Not supported — only proves personhood |
| **Privacy model** | Strong — biometrics never leave device |
| **Hardware requirement** | Requires Orb device for registration ❌ |
| **POC feasibility** | Very low — hardware dependency |

**Verdict**: Interesting for future integration as a "proof of personhood" layer, but **not suitable for the POC** due to hardware requirements and limited scope (personhood only, not credentials).

---

## Head-to-Head Comparison

### Performance Matrix

| Criteria | zk-SNARKs (Groth16) | zk-STARKs | Polygon ID | Semaphore | World ID |
|----------|---------------------|-----------|------------|-----------|----------|
| **Proof Size** | ~288 B ⭐ | ~45-200 KB | ~1-2 KB | ~288 B ⭐ | ~1 KB |
| **Verification Speed** | ~10 ms ⭐ | ~50-100 ms | ~15-25 ms | ~10 ms ⭐ | ~15 ms |
| **Prover Time (browser)** | ~2-10 s | ~10-60 s | ~3-15 s | ~2-5 s ⭐ | N/A |
| **Trusted Setup** | Required ⚠️ | None ✅ | None ✅ | Required ⚠️ | None ✅ |
| **Quantum Resistant** | No | Yes ✅ | No | No | No |
| **JS/TS Tooling** | Excellent ⭐ | Limited | Good | Excellent ⭐ | Good |

### Suitability Matrix

| Criteria | zk-SNARKs | zk-STARKs | Polygon ID | Semaphore | World ID |
|----------|-----------|-----------|------------|-----------|----------|
| **POC Complexity** | Low ⭐ | Medium | High | Low ⭐ | Very High |
| **Identity Verification** | Custom circuits | Custom circuits | Built-in ⭐ | Group membership | Personhood only |
| **Sybil Resistance** | Custom | Custom | Built-in | Built-in ⭐ | Built-in ⭐ |
| **Credential Proofs** | Flexible ⭐ | Flexible ⭐ | Built-in ⭐ | Limited | None |
| **Hiring Use Case Fit** | High | Medium | High | Very High ⭐ | Low |
| **Community / Docs** | Extensive ⭐ | Growing | Good | Good | Limited |
| **Migration Path** | SNARKs → PLONK → STARKs | — | — | Can add SNARKs circuits | — |

### Cost of Integration (Developer Hours Estimate)

| Protocol | Mock Integration | Real Integration | Total POC Risk |
|----------|-----------------|------------------|---------------|
| **Semaphore** | ~4 hours | ~20-40 hours | Low ⭐ |
| **zk-SNARKs (raw)** | ~4 hours | ~40-80 hours | Medium |
| **Polygon ID** | ~8 hours | ~80-120 hours | High |
| **zk-STARKs** | ~8 hours | ~60-100 hours | High |
| **World ID** | ~2 hours | N/A (hardware) | Very High |

---

## Application to Hiring Marketplace

### Verification Scenarios

#### Scenario 1: Candidate Proves Degree

```
Statement: "I hold a bachelor's degree from an accredited university"
Revealed:  Nothing about the specific university, GPA, graduation date, or major
Protocol:  zk-SNARK circuit that checks:
           - Credential signature is valid (from accredited issuer)
           - Degree level >= bachelor's
           - Credential is not expired/revoked
```

#### Scenario 2: Recruiter Proves Company Authorization

```
Statement: "I am an authorized recruiter for a Fortune 500 company"
Revealed:  Nothing about which company, their role, or employee ID
Protocol:  Semaphore group membership:
           - Company creates a Semaphore group
           - Authorized recruiters are added as members
           - Recruiter proves membership without revealing identity
```

#### Scenario 3: Company Proves Legitimacy (Stealth Mode)

```
Statement: "We are a legitimate company with 100+ employees"
Revealed:  Nothing about company name, industry, or location
Protocol:  zk-SNARK circuit that checks:
           - Company registration is valid (from government issuer)
           - Employee count >= threshold
           - Registration is current
```

#### Scenario 4: Candidate Uniqueness (Anti-Sybil)

```
Statement: "I am a unique person who hasn't registered before"
Revealed:  Nothing about identity
Protocol:  Semaphore nullifier:
           - Each person generates a unique nullifier from their identity
           - System checks nullifier hasn't been used before
           - Person is added to the "verified unique candidates" group
```

---

## NFC Passport Verification

### Overview

Modern passports (post-2006) contain NFC chips with digitally signed biometric data. This enables a powerful verification flow:

```
Physical Passport (NFC chip)
        │
        ▼
Mobile Device (NFC reader)
        │
        ▼
Extract signed data (without storing raw data)
        │
        ▼
Generate ZK proof: "I hold a valid passport from country X"
        │
        ▼
Submit proof to marketplace (no PII transmitted)
```

### Technical Flow

1. **Data Extraction**: NFC reader extracts the Document Security Object (SOD) containing:
   - Signed hash of biographical data
   - Signed hash of facial image
   - Issuing country's digital signature chain

2. **ZK Circuit**: Verifies:
   - SOD signature is valid (from a recognized issuing authority)
   - Document is not expired
   - Country is in an accepted list
   - _Without revealing_: name, DOB, passport number, nationality, photo

3. **Proof Output**: A compact proof (~288 bytes) that attests to passport validity

### POC Strategy

For the POC, we will:
- ✅ **Design the full interface** — API contracts, data flow, UI components
- ✅ **Create mock NFC verification service** — simulates the flow with test data
- ✅ **Define the ZK circuit specification** — what inputs, what's proven, what's revealed
- 🔲 **Defer real NFC integration** — requires mobile app, hardware testing, certificate chain validation

---

## Recommendation & Rationale

### Primary Choice: Semaphore Protocol

**For group membership and uniqueness proofs** (covers 70% of hiring verification needs):
- Candidate pool membership ("I'm a verified candidate")
- Recruiter authorization ("I'm authorized by Company X")
- Anti-Sybil ("I'm a unique person")
- Anonymous signaling ("I'm interested in this role")

### Secondary Choice: Custom zk-SNARK Circuits (via circom/snarkjs)

**For credential-specific proofs** (covers remaining 30%):
- Degree verification
- Employment history claims
- Salary range attestation
- Certification validation

### Why Not the Others?

| Protocol | Rejection Reason |
|----------|-----------------|
| **zk-STARKs** | Proof sizes too large for web UX; limited JS tooling; overkill for POC scale |
| **Polygon ID** | Integration complexity too high for POC; vendor lock-in; blockchain dependency for basic identity |
| **World ID** | Hardware requirement (Orb) makes it infeasible; only proves personhood, not credentials |

### Migration Path

```
POC (Now)                    v1.0                         v2.0
─────────────────────────────────────────────────────────────────
Mock ZK interfaces    →   Semaphore + Groth16    →   PLONK (universal setup)
Test data             →   Real proofs            →   + Polygon ID for VCs
No blockchain         →   Optional on-chain      →   Multi-chain support
```

---

## Implementation Strategy

### Phase 1 (Current): Mock Interfaces

```typescript
// Example: Mock ZK Verification Service Interface
interface ZKVerificationService {
  // Generate a mock proof for a credential claim
  generateProof(claim: CredentialClaim): Promise<ZKProof>;

  // Verify a mock proof
  verifyProof(proof: ZKProof): Promise<VerificationResult>;

  // Check uniqueness (anti-Sybil)
  checkUniqueness(nullifier: string): Promise<boolean>;

  // Add member to verification group
  addToGroup(groupId: string, commitment: string): Promise<void>;
}

// Mock implementation returns valid results with realistic delays
class MockZKService implements ZKVerificationService {
  async generateProof(claim: CredentialClaim): Promise<ZKProof> {
    await delay(2000); // Simulate proof generation time
    return {
      proof: '0x' + randomHex(256),
      publicSignals: [claim.type, claim.level],
      protocol: 'groth16',
      verified: true
    };
  }
  // ...
}
```

### Phase 2: Semaphore Integration

```typescript
// Real Semaphore integration (future)
import { Group, Identity, generateProof, verifyProof } from '@semaphore-protocol/core';

class SemaphoreVerificationService implements ZKVerificationService {
  async generateProof(claim: CredentialClaim): Promise<ZKProof> {
    const identity = new Identity(claim.secret);
    const group = await this.getGroup(claim.groupId);
    const proof = await generateProof(identity, group, claim.signal, claim.scope);
    return proof;
  }
  // ...
}
```

### Abstraction Boundary

The key architectural decision is defining a clean interface that both mock and real implementations satisfy:

```
┌─────────────────────────────┐
│     Marketplace Logic        │  ← Doesn't know/care if ZK is real or mock
│  (matching, messaging, etc.) │
└──────────────┬──────────────┘
               │
     ZKVerificationService (interface)
               │
      ┌────────┴────────┐
      │                  │
  MockZKService    SemaphoreService
  (POC)            (Production)
```

---

## References

1. **Groth16**: Groth, J. (2016). "On the Size of Pairing-based Non-interactive Arguments." EUROCRYPT 2016.
2. **PLONK**: Gabizon, A., Williamson, Z., Ciobotaru, O. (2019). "PLONK: Permutations over Lagrange-bases for Oecumenical Noninteractive arguments of Knowledge."
3. **STARKs**: Ben-Sasson, E., et al. (2018). "Scalable, transparent, and post-quantum secure computational integrity." IACR.
4. **Semaphore**: [https://semaphore.pse.dev/](https://semaphore.pse.dev/) — Privacy & Scaling Explorations, Ethereum Foundation.
5. **circom/snarkjs**: [https://github.com/iden3/circom](https://github.com/iden3/circom) — Iden3.
6. **Polygon ID**: [https://polygonid.com/](https://polygonid.com/) — Polygon Labs.
7. **NFC Passport**: ICAO Doc 9303 — Machine Readable Travel Documents.
8. **Worldcoin**: [https://worldcoin.org/](https://worldcoin.org/) — Tools for Humanity.

---

_This research document is part of Phase 0 of the [Trustless Hiring Marketplace POC](../../README.md). See the [Evolution Log](../EVOLUTION.md) for context on how these decisions were made._
