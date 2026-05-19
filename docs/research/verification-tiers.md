# Verification Tiers: Candidate, Recruiter & Company Architecture

> **Date**: May 18, 2026
> **Status**: Architecture Defined — Ready for Implementation
> **Related**: [ZK Identity Protocols](zk-identity-protocols.md) | [Project Charter](../PROJECT-CHARTER.md)

---

## Table of Contents

1. [Overview](#overview)
2. [Design Principles](#design-principles)
3. [Candidate Verification Tiers](#candidate-verification-tiers)
4. [Recruiter Verification Tiers](#recruiter-verification-tiers)
5. [Company Verification Tiers](#company-verification-tiers)
6. [Trust Score Calculation](#trust-score-calculation)
7. [Cross-Entity Trust Interactions](#cross-entity-trust-interactions)
8. [POC Implementation Plan](#poc-implementation-plan)

---

## Overview

The Trustless Hiring Marketplace uses a **tiered verification model** where each user type (candidate, recruiter, company) can progressively increase their trust level. Higher verification tiers unlock more marketplace features and increase match priority.

### Core Principle

> Users control how much they verify. More verification = more trust = better matches. But even basic accounts can participate meaningfully.

### Tier Summary

```
CANDIDATES                RECRUITERS               COMPANIES
──────────               ──────────               ─────────
Tier 1: Basic            Track A: Corporate       Tier 1: Stealth
  └── Email verified       └── Company + Employee   └── Verified but anonymous
                                                    
Tier 2: Enhanced         Track B: Independent     Tier 2: Public
  └── ZK uniqueness        └── Credential-based     └── Named & verified
      proof                                         
                                                  Tier 3: Enterprise
Tier 3: Full                                        └── Full organizational
  └── NFC passport                                      verification
      ZK proof
```

---

## Design Principles

1. **Progressive Trust**: Users start with minimal verification and add layers over time
2. **Privacy by Default**: Each tier reveals the _minimum_ necessary information
3. **No Central Authority**: Verification relies on cryptographic proofs, not a trusted admin
4. **Incentive Alignment**: Higher tiers get tangible marketplace benefits (better matching, priority listing)
5. **Graceful Degradation**: The marketplace functions at every tier — it's better with more verification, but never gatekept

---

## Candidate Verification Tiers

### Tier 1: Basic Verification

**What's Verified**: Email address ownership
**Method**: Standard email verification flow (link/code)
**ZK Component**: None
**Trust Score**: 20/100

| Feature | Access |
|---------|--------|
| Create profile | ✅ |
| Browse public jobs | ✅ |
| Apply to jobs | ✅ (marked as "Basic Verified") |
| Appear in matching | ✅ (lower priority) |
| Send messages | ❌ (can respond only) |
| View company details | Limited |

**Data Stored**:
```json
{
  "verification_level": "basic",
  "email_verified": true,
  "email_hash": "sha256(email)", // stored, not the email itself
  "verified_at": "2026-05-18T12:00:00Z"
}
```

### Tier 2: Enhanced Verification

**What's Verified**: Unique personhood (one person = one account)
**Method**: Semaphore-based ZK uniqueness proof
**ZK Component**: Semaphore identity commitment + nullifier
**Trust Score**: 55/100

| Feature | Access |
|---------|--------|
| All Tier 1 features | ✅ |
| Appear in matching | ✅ (standard priority) |
| Send messages | ✅ |
| View company details | Full (public companies) |
| Verified badge | ✅ "Identity Verified" |
| Credential claims | ✅ (self-attested, marked as unverified) |

**How It Works**:
```
1. Candidate generates a Semaphore identity (secret + nullifier)
2. Identity commitment = Poseidon(secret, nullifier)
3. Commitment added to "Verified Candidates" Merkle tree
4. Candidate can now prove membership without revealing identity
5. Nullifier ensures one identity per person
```

**Data Stored**:
```json
{
  "verification_level": "enhanced",
  "identity_commitment": "0x1a2b3c...", // Poseidon hash
  "nullifier_hash": "0x4d5e6f...",      // prevents duplicate registration
  "group_id": "verified-candidates",
  "verified_at": "2026-05-18T14:00:00Z"
}
```

### Tier 3: Full Verification

**What's Verified**: Government-issued identity (without revealing it)
**Method**: NFC passport reading → ZK proof generation
**ZK Component**: zk-SNARK proof of valid passport + Semaphore membership
**Trust Score**: 90/100

| Feature | Access |
|---------|--------|
| All Tier 2 features | ✅ |
| Appear in matching | ✅ (highest priority) |
| Premium badge | ✅ "Fully Verified" |
| Credential proofs | ✅ (ZK-backed, marked as verified) |
| Priority in matching | ✅ |
| Access to Enterprise jobs | ✅ |
| Escrow-eligible | ✅ (for contract payments) |

**How It Works**:
```
1. Candidate taps passport on NFC-enabled phone
2. App reads Document Security Object (SOD) from chip
3. ZK circuit verifies:
   - Passport signature chain is valid
   - Document is not expired
   - Country is in accepted list
4. Proof is generated (~288 bytes)
5. Proof submitted to marketplace (no PII transmitted)
6. Candidate added to "Passport Verified" Semaphore group
```

**Data Stored**:
```json
{
  "verification_level": "full",
  "passport_proof": "0x...",           // ZK proof (no PII)
  "passport_country_group": "accepted", // which country list
  "expiry_valid": true,                // proof that passport isn't expired
  "semaphore_groups": [
    "verified-candidates",
    "passport-verified",
    "country-accepted"
  ],
  "verified_at": "2026-05-18T16:00:00Z"
}
```

---

## Recruiter Verification Tiers

Recruiters follow two distinct tracks based on their relationship to the hiring company.

### Track A: Corporate Recruiter

**Who**: In-house recruiters employed by the hiring company
**Verification**: Dual proof — company membership + employee status

#### Level A1: Company-Linked

**What's Verified**: Employment at a specific company
**Method**: Company admin adds recruiter to their Semaphore group
**Trust Score**: 70/100

```
Company Admin creates Semaphore group "CompanyX-Recruiters"
     │
     ▼
Admin adds recruiter's identity commitment to group
     │
     ▼
Recruiter proves: "I am a member of CompanyX-Recruiters"
     │
     ▼
Marketplace shows: "Verified Corporate Recruiter" (company anonymous until revealed)
```

| Feature | Access |
|---------|--------|
| Post jobs for linked company | ✅ |
| Access candidate pool | ✅ (matched candidates) |
| Send messages to candidates | ✅ |
| Represent company anonymously | ✅ |
| Reveal company identity | Optional (with company approval) |

#### Level A2: Authorized Representative

**What's Verified**: Specific authorization scope (hiring manager, executive recruiter, etc.)
**Method**: Role-specific Semaphore sub-groups
**Trust Score**: 85/100

Additional features:
- Can make offers on behalf of company
- Can access salary range data
- Can participate in escrow transactions

### Track B: Independent Recruiter

**Who**: Third-party recruiters, staffing agencies, headhunters
**Verification**: Credential-based (industry certifications, track record)

#### Level B1: Credential-Verified

**What's Verified**: Professional credentials (AIRS, CDR, LinkedIn Recruiter certification, etc.)
**Method**: ZK proof of credential ownership
**Trust Score**: 50/100

| Feature | Access |
|---------|--------|
| Browse candidate profiles | ✅ (limited) |
| Send connection requests | ✅ (rate-limited) |
| Post jobs (with company linking) | ✅ |
| Independent badge | ✅ "Verified Independent Recruiter" |

#### Level B2: Track Record Verified

**What's Verified**: Historical placement success (without revealing specific placements)
**Method**: ZK proof aggregating placement data
**Trust Score**: 75/100

```
ZK Circuit proves:
  - Number of successful placements > threshold
  - Average placement tenure > minimum
  - Client satisfaction score > threshold
  WITHOUT revealing: specific companies, candidates, or deals
```

---

## Company Verification Tiers

### Tier 1: Stealth Mode

**What's Verified**: Company existence and legitimacy (without revealing identity)
**Method**: ZK proof of valid business registration
**Trust Score**: 40/100

**Use Cases**:
- Companies hiring for secret projects
- Startups not ready to announce
- Companies exploring talent pools before committing

| Feature | Access |
|---------|--------|
| Browse candidates | ✅ (anonymously) |
| Post stealth jobs | ✅ ("Verified Stealth Company") |
| Matching participation | ✅ (limited signals) |
| Recruiter assignment | ✅ (via Semaphore groups) |
| Company name visible | ❌ |

**What's Proven via ZK**:
```
- Valid business registration (from government registrar)
- Company size category (1-10, 11-50, 51-200, 201-1000, 1000+)
- Industry category (without specific company)
- Operating jurisdiction
```

### Tier 2: Public Company

**What's Verified**: Full company identity + legitimacy
**Method**: Business registration verification + domain ownership + ZK proofs
**Trust Score**: 75/100

| Feature | Access |
|---------|--------|
| All Tier 1 features | ✅ |
| Company profile page | ✅ (public) |
| Enhanced matching signals | ✅ (culture, benefits, growth data) |
| Candidate messaging | ✅ (direct outreach) |
| Employer branding | ✅ |
| Analytics dashboard | ✅ |

### Tier 3: Enterprise

**What's Verified**: Full organizational verification + compliance + multi-department
**Method**: Comprehensive verification including HR system integration
**Trust Score**: 95/100

| Feature | Access |
|---------|--------|
| All Tier 2 features | ✅ |
| Multi-department job posting | ✅ |
| Bulk candidate matching | ✅ |
| API access | ✅ |
| Custom matching weights | ✅ |
| Escrow & contract management | ✅ |
| Audit trail | ✅ |
| Dedicated support | ✅ |

---

## Trust Score Calculation

### Formula

```
Trust Score = Base Score (from tier) + Bonus Modifiers

Where:
  Base Score = { basic: 20, enhanced: 55, full: 90 }

  Bonus Modifiers (each +5, max +10):
    - Profile completeness > 80%
    - Account age > 30 days
    - Positive interaction history
    - No reported violations
```

### Trust Score Impact on Matching

```
Match Priority Weight = Trust Score × 0.15

Example:
  Candidate A (Trust: 90) + Company B (Trust: 75)
  Combined Trust Factor = (90 + 75) / 200 × 0.15 = 0.124

  Candidate C (Trust: 20) + Company B (Trust: 75)
  Combined Trust Factor = (20 + 75) / 200 × 0.15 = 0.071
```

Higher trust scores result in:
- Earlier appearance in match results
- Higher match confidence scores
- Access to more detailed match explanations

---

## Cross-Entity Trust Interactions

### Mutual Verification Flow

```
┌──────────────┐                    ┌──────────────┐
│  Candidate    │                    │  Company      │
│  (Tier 2)     │                    │  (Tier 2)     │
│               │                    │               │
│  Proves:      │◄──── Match ────►  │  Proves:      │
│  - Unique     │                    │  - Legitimate  │
│  - Has degree │                    │  - Size 200+   │
│  - 5yr exp    │                    │  - Tech sector  │
│               │                    │               │
│  Reveals:     │                    │  Reveals:     │
│  - Nothing    │                    │  - Nothing     │
└──────┬───────┘                    └──────┬───────┘
       │                                    │
       │        Both interested?            │
       │◄────────── Mutual ──────────►     │
       │           Consent                  │
       │                                    │
       ▼                                    ▼
  Reveal name,                      Reveal company,
  contact info                      role details
```

### Progressive Disclosure Protocol

1. **Match Phase**: Both parties see only verified attributes (no PII)
2. **Interest Phase**: Both express interest → unlock communication (still pseudonymous)
3. **Consent Phase**: Both agree to reveal → exchange real identities
4. **Engagement Phase**: Full interaction with verified identities

---

## POC Implementation Plan

### What We Build (Mock)

| Component | Implementation | Simulates |
|-----------|---------------|-----------|
| Verification UI | Real React components | Tier selection and verification flow |
| ZK proof generation | Mock service with delays | Real proof computation time |
| Proof verification | Mock validator (always pass for valid test data) | On-chain/off-chain verification |
| Trust score display | Real calculation from mock data | Trust score algorithm |
| Tier badges | Real UI components | Visual trust indicators |
| NFC passport flow | Simulated with test data | Full NFC → ZK → verify pipeline |

### What We Design (Interface Only)

| Component | Deliverable |
|-----------|------------|
| ZK circuit specifications | Input/output definitions for each proof type |
| Semaphore group architecture | Group hierarchy and membership rules |
| NFC data extraction | SOD parsing and ZK circuit design |
| On-chain verifier | Solidity contract interface |

### Database Schema Preview

```sql
-- Verification records (core of the tier system)
CREATE TABLE verifications (
  id              UUID PRIMARY KEY,
  user_id         UUID NOT NULL REFERENCES users(id),
  user_type       VARCHAR(20) NOT NULL, -- 'candidate', 'recruiter', 'company'
  tier            VARCHAR(20) NOT NULL, -- 'basic', 'enhanced', 'full', etc.
  method          VARCHAR(50) NOT NULL, -- 'email', 'semaphore', 'nfc_passport'
  proof_hash      VARCHAR(128),         -- ZK proof identifier (no PII)
  nullifier_hash  VARCHAR(128),         -- Sybil resistance
  group_id        VARCHAR(64),          -- Semaphore group
  trust_score     INTEGER NOT NULL DEFAULT 20,
  verified_at     TIMESTAMP NOT NULL,
  expires_at      TIMESTAMP,
  revoked_at      TIMESTAMP,
  metadata        JSONB                 -- Tier-specific data (no PII)
);

CREATE INDEX idx_verifications_user ON verifications(user_id);
CREATE INDEX idx_verifications_tier ON verifications(tier);
CREATE UNIQUE INDEX idx_verifications_nullifier ON verifications(nullifier_hash)
  WHERE nullifier_hash IS NOT NULL;
```

---

_This document is part of Phase 0 of the [Trustless Hiring Marketplace POC](../../README.md). For ZK protocol details, see [zk-identity-protocols.md](zk-identity-protocols.md)._
