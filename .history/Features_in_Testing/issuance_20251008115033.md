---
title: Issuance
description: How Verifiable Credentials are issued in the Tradeverifyd platform.
---

# Issuance

This page describes how **Verifiable Credentials (VCs)** are issued within the Tradeverifyd platform — from **template selection to signing and storage** — and how this process supports **global interoperability, U.S. Customs modernization, and tamper-proof trade compliance**.

---

## 🔍 Overview

**Issuance** is the process of creating a **digitally signed Verifiable Credential (VC)** that can be independently verified by anyone, anywhere.

In the W3C’s “triangle of trust” model, three key roles participate:

| Role | Function |
|------|-----------|
| **Issuer** | Creates and signs the credential. (Tradeverifyd Platform) |
| **Holder** | Receives and stores the credential. (Customer or Partner) |
| **Verifier** | Validates authenticity and integrity. (CBP, partner systems, or internal compliance team) |

Every Tradeverifyd-issued credential follows the **W3C Verifiable Credentials Data Model 2.0**, ensuring it is:
- **Cryptographically signed** (Ed25519, JOSE/COSE)
- **Tamper-evident**
- **Globally interoperable**
- **Machine-readable and human-verifiable**

---

## 🌐 Issuance in Context

Tradeverifyd’s issuance flow is designed to support key initiatives that are transforming global trade and compliance:

- **CBP’s 21st Century Customs Framework (21CCF):** Enables real-time, tamper-proof data exchange across the supply chain.
- **Global Interoperability Standards:** Aligns with efforts to test and implement Verifiable Credentials and Decentralized Identifiers (DIDs) across industries such as steel, oil, food safety, and e-commerce.
- **Tradeverifyd’s Role:** Provides a compliant, standards-based credentialing service that integrates with **ACE**, **partner government agencies (PGAs)**, and private-sector verification systems using open APIs and JSON-LD data models.

---

## ⚙️ End-to-End Issuance Flow

The issuance process follows six key stages, each built for transparency and interoperability.

### 1) Template Selection

Each credential starts with a **W3C-aligned JSON-LD template** defining its data structure and semantics.

Example templates:
- `urn:tradeverifyd:template:mill-test-report`
- `urn:tradeverifyd:template:object-event`
- `urn:tradeverifyd:template:ctpat`

```json
{
  "template_id": "urn:tradeverifyd:template:mill-test-report",
  "issuer": "did:web:tradeverifyd.com",
  "credentialSubject": {
    "batchNumber": "MTR-1234",
    "originCountry": "JP",
    "manufacturer": "Sample Test Mill"
  }
}
```

---

### 2) Schema Validation

The payload is validated against the referenced schema (for example, `credentialSchema.id`), ensuring data integrity and alignment with open vocabularies.

```json
{ "detail": "claimset is missing credentialSchema.id" }
```

Tradeverifyd enforces schema validation before any VC is signed to maintain interoperability with W3C standards and CBP credential formats.

---

### 3) Credential Creation

Tradeverifyd constructs a complete Verifiable Credential containing:

- `@context` – references VC vocabulary and extensions
- `issuer` – the entity asserting the claims
- `issuanceDate` – ISO 8601 timestamp
- `credentialSubject` – the factual claims
- Optional: `expirationDate`, `termsOfUse`, and `status`

```json
{
  "@context": ["https://www.w3.org/ns/credentials/v2"],
  "type": ["VerifiableCredential", "MillTestReport"],
  "issuer": "did:web:tradeverifyd.com",
  "issuanceDate": "2025-10-06T18:23:00Z",
  "credentialSubject": {
    "batch": "MTR-1234",
    "grade": "ASTM A615",
    "heatNumber": "A1007"
  }
}
```

---

### 4) Digital Signing

The credential is cryptographically signed using **Ed25519Signature2020** or a JOSE/COSE-based proof to ensure tamper-resistance.

```json
{
  "proof": {
    "type": "Ed25519Signature2020",
    "created": "2025-10-06T18:23:12Z",
    "proofPurpose": "assertionMethod",
    "verificationMethod": "https://tradeverifyd.com/keys/1",
    "proofValue": "z3JQ9BYR..."
  }
}
```

This signing method allows **offline verification**, meaning the verifier can check authenticity without contacting the issuer.

---

### 5) Storage & Status Tracking

Signed VCs are stored in Tradeverifyd’s **Verifiable Data Registry**, enabling:

- Retrieval by file, shipment, or entity ID
- Access through `/v1/reports` and `/v1/reports/{report_id}/content`
- Continuous status checks (revoked, expired, superseded)

Each credential can include a **status list entry** to indicate revocation or replacement state.

---

### 6) Holder Access & Presentation

Once issued, the credential is available to the **Holder** via:

- The Tradeverifyd API
- Partner integrations (for example, via `/v1/reports`)
- Verifiable Presentation bundles for submission to CBP or third parties

VCs can be combined into a single **Verifiable Presentation (VP)**. For example, a shipment might include:

- Mill Test Report
- Bill of Lading
- Intent to Import Credential
- Entry Number
- CTPAT Credential

Each is independently verifiable and cryptographically bound to its issuer.

---

## 🧩 Technical Diagram: Issuance Flow

```
┌────────────────────────┐
│  Tradeverifyd Issuer   │
│  (VC API + Templates)  │
└──────────┬─────────────┘
           │  Issue Request
           ▼
┌────────────────────────┐
│ Validate Schema & Data │
│  + Add Proof Metadata  │
└──────────┬─────────────┘
           │  Sign VC
           ▼
┌────────────────────────┐
│ Sign + Store (Ed25519) │
│  VC Registry (Status)  │
└──────────┬─────────────┘
           │
           ▼
┌────────────────────────┐
│ Holder Wallet/API      │
│  (Customer or Partner) │
└──────────┬─────────────┘
           │  Verifiable Presentation
           ▼
┌────────────────────────┐
│ Verifier (CBP / PGA)   │
│  Validate Signature &  │
│  Schema Conformance    │
└────────────────────────┘
```



---

## 🧭 Compliance & Operations Overview

For compliance and operations users, issuance represents the **moment a document becomes verifiable** — transforming static PDFs or trade forms into **digitally signed, traceable assets**.

### Key Benefits

- **Tamper-proof documentation** across suppliers and brokers
- **Instant verification** by Customs or auditors
- **Reduced clearance times** through pre-arrival validation
- **Aligned with U.S. CBP 21CCF and ACE modernization goals**
- **Supports USMCA, Trade Mod Act 2.0, and IFSA compliance**

Tradeverifyd’s open standards approach ensures that verifiable documents can be shared securely and privately — no proprietary systems required, no data lock-in.

---

## 📚 Standards & References

- [W3C Verifiable Credentials Data Model 2.0](https://www.w3.org/TR/vc-data-model-2.0/)
- [W3C Decentralized Identifiers (DID) Core Specification](https://www.w3.org/TR/did-core/)
- [CBP: Leading with Global Interoperability Standards](https://www.cbp.gov/trade/innovation/leading-global-interoperability-standards)
- [U.S. 21st Century Customs Framework (21CCF)](https://www.cbp.gov/trade/21st-century-customs-framework)

---

> In short: Issuance is the foundation of trust. Each credential issued by Tradeverifyd is globally interoperable, cryptographically secured, and designed to help importers, manufacturers, and regulators operate with confidence in a transparent, verifiable trade environment.
