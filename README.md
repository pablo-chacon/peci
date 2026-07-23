# PECI
### PEI Concealed Identifier

**Work in progress**

A formal protocol specification for applying 5G's existing subscriber identity concealment model to device identity, closing the hardware layer gap that no current cellular anonymization solution addresses.

**Author:** Emil Karlsson (pablo-chacon)
**Status:** Specification in progress
**Companion project:** [CAMO](https://github.com/pablo-chacon/camo), Cellular Anonymization and Mobile Onion-routing

> **Current state of this repository:** the specification currently lives in this README. The expanded documents and reference implementation described under [Planned structure](#planned-structure) are not yet published.

---

## The problem

5G's security architecture (3GPP TS 33.501) made a deliberate and correct decision: the subscriber's permanent identity (SUPI) must never be transmitted in plaintext. It is encrypted using ECIES into a Subscription Concealed Identifier (SUCI) before leaving the device.

The same architecture defines the Permanent Equipment Identifier (PEI), the 5G equivalent of IMEI, as the device's permanent identity. PEI transmission was placed inside the NAS security context, an improvement over 4G. But the concealment model applied to SUPI was not applied to PEI.

The result: subscriber identity is cryptographically concealed. Device identity is not.

This gap means that even on a fully compliant 5G network, with SUCI protecting subscriber identity and CAMO protecting routing and SIM identity, the hardware identifier remains a persistent, correlatable fingerprint visible to the network.

PECI closes that gap.

---

## The approach

SUPI and PEI are structurally equivalent identifiers at different layers:

| Layer | Permanent ID | 5G treatment |
|---|---|---|
| Subscriber | SUPI (replaces IMSI) | Concealed via SUCI (ECIES encryption) |
| Device | PEI (replaces IMEI) | Transmitted in NAS security context, no concealment |

PECI applies the SUCI concealment model to PEI:

```
PEI + Home network public key  ->  ECIES encryption  ->  PECI
```

PECI is probabilistic. The same PEI encrypted multiple times produces unlinkable ciphertexts. No observer outside the home network can correlate PECI transmissions to a device or to each other.

The home network retains the ability to de-conceal PECI via its private key, preserving legitimate network functions including device blocking and equipment identity validation, while removing persistent device identity from the observable radio layer.

---

## Why this is not a new invention

Every component of PECI exists in deployed 5G infrastructure:

| Component | Standard | Status |
|---|---|---|
| PEI definition | 3GPP TS 23.003 | Deployed |
| ECIES concealment scheme | 3GPP TS 33.501 (SUCI) | Deployed |
| NAS security context | 3GPP TS 33.501 | Deployed |
| SIDF de-concealment function | 3GPP TS 33.501 | Deployed |
| Curve25519 / secp256r1 | Profile A / Profile B | Deployed |

PECI is the recognition that the concealment model already specified and deployed for subscriber identity applies directly and completely to device identity, and the formal specification of that application.

---

## Relationship to SUCI

PECI follows SUCI's construction as closely as possible to minimize implementation surface and maximize compatibility with existing 5G core infrastructure.

SUCI structure (TS 33.501):

```
SUCI = SUPI type | Home network identifier | protection scheme | public key | encrypted MSIN
```

PECI structure (proposed):

```
PECI = PEI type | Home network identifier | protection scheme | public key | encrypted device identifier
```

Protection schemes: Profile A (Curve25519 + AES-128-CTR + HMAC-SHA-256) and Profile B (secp256r1 + AES-128-CTR + HMAC-SHA-256), identical to SUCI.

De-concealment: performed by SIDF within UDM, identical to SUCI de-concealment, extended to handle PEI type.

---

## What PECI does and does not protect

**Protects:**

- Device identity from passive observers on the radio interface
- Device identity from rogue base stations and IMSI/IMEI catchers
- Cross-session device correlation from network-layer observers
- Hardware fingerprinting across SIM rotations

**Does not protect:**

- Physical device characteristics observable at other layers (RF fingerprinting, timing characteristics)
- Device identity from the home network. This is by design, de-concealment is intentional
- Device identity when NAS security context cannot be established (emergency registration, the same limitation as SUCI)

---

## Threat model

### In scope

**Passive radio observer:** cannot correlate PECI transmissions. Each transmission is cryptographically unlinkable without the home network private key.

**Rogue base station:** receives PECI, cannot de-conceal without the home network private key. Device hardware identity is not exposed.

**Third-party network observer:** sees PECI, gains no information about device identity or prior sessions.

**Cross-carrier correlation:** without a stable device identifier visible across carriers, hardware-layer correlation across network boundaries is broken.

### Out of scope

**Compromised home network:** PECI provides no protection against an adversary with access to the home network's private key or SIDF. This is a deliberate design boundary, consistent with SUCI's threat model.

**Physical layer analysis:** RF fingerprinting and timing-based device identification operate below PECI's protection layer.

**Legal compulsion of home network:** equivalent to SUCI's limitation under lawful interception frameworks (3GPP TS 33.106, TS 33.107).

---

## Planned structure

The specification is currently contained in this README. The following is the intended layout as the work is expanded:

```
peci/
├── README.md
├── legal.md                        # legal considerations and standards compliance
│
├── docs/
│   ├── peci_spec.md                # protocol specification
│   ├── suci_peci_mapping.md        # formal mapping from SUCI model to PECI
│   ├── 5g_device_identity.md       # 5G PEI architecture analysis
│   └── threat_model.md             # full threat model
│
├── peci-core/                      # ECIES concealment implementation
├── peci-sidf/                      # de-concealment function (UDM extension)
└── peci-validation/                # test vectors and validation tooling
```

Published so far: `README.md`, `legal.md`, `LICENSE`.

---

## Specifications referenced

| Document | Description |
|---|---|
| 3GPP TS 33.501 | Security architecture and procedures for 5G: SUCI, ECIES profiles, NAS security |
| 3GPP TS 23.003 | Numbering, addressing and identification: PEI definition and structure |
| 3GPP TS 23.501 | System architecture for 5G: UDM, SIDF, identity framework |
| 3GPP TS 29.511 | Equipment Identity Register Services |
| 3GPP TS 33.106 / 33.107 | Lawful interception: out of scope boundary definition |

---

## Relation to CAMO

PECI and CAMO address adjacent, non-overlapping layers:

```
Application layer   ->  Tor (existing)
IP / routing layer  ->  CAMO, cellular onion routing
Hardware identity   ->  PECI, device identity concealment
```

CAMO's published threat model explicitly documents device identity (IMEI/PEI) as out of scope. PECI is the answer to that documented gap. The two are designed to compose: CAMO rotates SIM identity and routing chains, PECI removes the persistent hardware identifier that survives SIM rotation.

Neither project requires the other. Both are independently deployable against their respective threat surfaces.

---

**Contact:** <pablo-chacon-ai@proton.me>

*Emil Karlsson (pablo-chacon), June 2026*
