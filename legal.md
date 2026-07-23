# PECI -> Legal Considerations

**Project:** PECI (PEI Concealed Identifier)
**Author:** Emil Karlsson (pablo-chacon)
**Date:** June 2026

---

## Nature of this work

PECI is a protocol specification. It describes a formal application of cryptographic concealment techniques already defined and deployed in the 3GPP 5G security architecture (TS 33.501) to an adjacent identifier class (PEI) within the same standards framework.

This document contains no software, no executable code, no implementation, and no instructions for unauthorized access to any system. It is a research and academic contribution to the field of mobile network privacy architecture.

---

## Standards compliance

PECI is derived entirely from published, publicly available international telecommunications standards:

- 3GPP TS 33.501 -> Security architecture and procedures for 5G
- 3GPP TS 23.003 -> Numbering, addressing and identification
- 3GPP TS 23.501 -> System architecture for 5G
- 3GPP TS 29.511 -> Equipment Identity Register Services
- 3GPP TS 33.106 / 33.107 -> Lawful interception requirements

All referenced standards are published by 3GPP and freely available at www.3gpp.org. No proprietary, confidential, or restricted technical information is contained in this specification.

---

## Lawful interception

PECI is explicitly designed to preserve lawful interception capabilities. The de-concealment function (SIDF within UDM) allows the home network operator to recover the underlying PEI from any PECI transmission using the home network's private key.

This design is intentional and consistent with the existing SUCI model in 3GPP TS 33.501, which similarly preserves home network de-concealment capability while protecting subscriber identity from third-party observers.

PECI does not circumvent, disable, or degrade lawful interception infrastructure. Any implementation of PECI that removes or bypasses the de-concealment capability defined in this specification is outside the scope of this document and contrary to its design intent.

---

## No liability

This specification is provided as-is, for research and informational purposes only.

The author makes no representations or warranties of any kind, express or implied, regarding:

- The accuracy, completeness, or fitness for purpose of this specification
- The legality of implementing or deploying this specification in any jurisdiction
- The security properties of any implementation derived from this specification
- The compatibility of this specification with any carrier, network operator, regulatory framework, or device

The author accepts no liability for any direct, indirect, incidental, consequential, or special damages arising from the use, misuse, implementation, or inability to implement this specification, or from any action taken in reliance on its contents.

---

## Implementer responsibility

Any party implementing, deploying, or operating systems based on this specification bears sole and full responsibility for:

- Ensuring compliance with all applicable laws and regulations in their jurisdiction
- Ensuring compliance with applicable telecommunications regulations, including but not limited to regulations governing device identity, network access, and lawful interception
- Obtaining any required approvals, licenses, or authorizations from relevant regulatory authorities
- Ensuring their implementation preserves required lawful interception capabilities as mandated by applicable law
- Any consequences arising from their implementation or deployment

The author is not responsible for implementations that deviate from this specification, implement only partial components of this specification, or are deployed in contexts not anticipated by this specification.

---

## Research and academic use

This specification is a contribution to academic and technical research in mobile network privacy. It is intended to:

- Identify and formally document a gap in the 3GPP 5G security architecture
- Propose a standards-consistent approach to closing that gap
- Stimulate academic discussion and peer review
- Contribute to the development of future telecommunications standards

Use of this specification for academic research, security analysis, standards development, and educational purposes is explicitly encouraged.

---

## No authorization for unlawful use

Nothing in this specification authorizes or encourages:

- Unauthorized access to telecommunications networks or infrastructure
- Interference with carrier operations or network integrity
- Circumvention of lawful interception requirements
- Any activity that violates applicable telecommunications law, criminal law, or network operator terms of service

Any use of this specification or derivative works for unlawful purposes is expressly prohibited and is the sole responsibility of the party undertaking such use.

---

## Jurisdiction

This specification is published from Sweden. Swedish law and European Union telecommunications regulations, including the European Electronic Communications Code (Directive 2018/1972/EU) and relevant ETSI standards, form part of the regulatory context in which this research was conducted.

The author makes no representation that this specification is appropriate for use or implementation in any specific jurisdiction outside Sweden and the European Union. Implementers outside these jurisdictions are responsible for determining the applicability of local law.

---

## Export control

This specification contains cryptographic concepts derived from publicly available international standards. The author makes no determination regarding export control classifications under any jurisdiction's export control laws, including but not limited to the EU Dual Use Regulation (2021/821) or equivalent frameworks.

Implementers are responsible for determining the export control status of any implementation and for obtaining any required export authorizations.

---

## Contact

For legal inquiries related to this specification:

pablo-chacon-ai@proton.me

*Emil Karlsson (pablo-chacon) -> June 2026*
