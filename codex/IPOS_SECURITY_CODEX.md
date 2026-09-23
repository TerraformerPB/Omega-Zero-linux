# IPOS SECURITY CODEX

Version: 0.1
Status: FOUNDATIONAL / NORMATIVE

This document defines mandatory architectural rules for Omega-Zero/IPOS.

## RULE-001 — DEFAULT DENY

Any permission not explicitly granted is denied.

## RULE-002 — LEAST PRIVILEGE

Every identity receives only the minimum authority required for its function.

## RULE-003 — NO IMPLICIT TRUST

Network location, username, application origin or organizational ownership never constitutes trust.

## RULE-004 — SEPARATE IDENTITIES

Device, user, application and service identities MUST remain logically distinct.

## RULE-005 — HARDWARE-BACKED ROOT

Production device identity and high-value private keys SHOULD be hardware-backed and non-exportable.

## RULE-006 — VERIFIED BOOT

Production hardware MUST establish a cryptographically verifiable boot chain.

## RULE-007 — LOCKED PRODUCTION STATE

A production security profile MUST distinguish unlocked/development devices from locked/verified devices.

## RULE-008 — SECURITY ENFORCEMENT BELOW UI

The graphical control application MUST NOT be the final authority for security decisions. Enforcement must exist below ordinary applications.

## RULE-009 — APPLICATION ISOLATION

Compromise of one application MUST NOT automatically grant access to another application's data or credentials.

## RULE-010 — SERVICE ISOLATION

Each security-sensitive backend service MUST have a distinct service identity and scoped authorization.

## RULE-011 — NETWORK DEFAULT DENY

Applications MUST NOT receive arbitrary network access by default.

## RULE-012 — DATA FLOW CONTROL

Network authorization alone is insufficient. Sensitive data flows MUST be policy-controlled.

## RULE-013 — NO CUSTOM CRYPTO

IPOS MUST NOT invent cryptographic algorithms or protocols where established, reviewed standards are available.

## RULE-014 — KEY NON-EXPORTABILITY

High-value private keys SHOULD remain inside hardware-backed or otherwise isolated key storage.

## RULE-015 — ATTESTATION

High-value cloud access SHOULD require acceptable device integrity evidence.

## RULE-016 — FRESHNESS

Security decisions based on attestation or authorization evidence MUST prevent replay through nonces, timestamps or equivalent mechanisms.

## RULE-017 — FAIL CLOSED

Security-critical unknown, invalid, expired or revoked states MUST default to denial.

## RULE-018 — REVOCATION

Device compromise MUST allow rapid device/session/credential revocation without requiring destruction of the user's entire cloud identity.

## RULE-019 — CRYPTOGRAPHIC ERASURE

Where local sensitive data exists, key destruction SHOULD be preferred over reliance on software file deletion.

## RULE-020 — MINIMIZE LOCAL DATA

Persistent sensitive user data SHOULD remain off-device whenever practical.

## RULE-021 — SIGNED UPDATES

Production update artifacts MUST be cryptographically authenticated.

## RULE-022 — ANTI-ROLLBACK

Production devices MUST prevent unauthorized rollback to known-vulnerable software versions.

## RULE-023 — A/B RECOVERY

A failed production update SHOULD permit automatic rollback to a known-good system.

## RULE-024 — SUPPLY-CHAIN TRANSPARENCY

Production components MUST have documented source, version, provenance and dependency information.

## RULE-025 — REPRODUCIBILITY

Security-critical release artifacts SHOULD be independently reproducible.

## RULE-026 — SBOM

Every production release MUST publish a machine-readable software bill of materials.

## RULE-027 — SEPARATE SIGNING

Release signing authority MUST be isolated from ordinary development and CI.

## RULE-028 — FOUR-EYES

Security-critical source and release changes SHOULD require independent review.

## RULE-029 — SECURITY LOGGING

Security-critical events MUST be recorded with integrity protection while minimizing personal data.

## RULE-030 — RECOVERY IS NOT A BACKDOOR

Recovery mechanisms MUST be subject to the same or stronger security controls as normal authentication.

## RULE-031 — NO SECRET ADMIN BYPASS

There MUST be no undocumented universal administrator credential or hidden bypass.

## RULE-032 — HARDWARE LIMITS ARE EXPLICIT

Software security claims MUST document hardware assumptions and cannot claim to defeat attacks outside the supported hardware threat model.

## RULE-033 — OPEN VERIFICATION

Security claims SHOULD be backed by source, build, test and audit evidence rather than trust in the project maintainers.

## RULE-034 — CHANGE CONTROL

Changes to the trusted computing base require explicit security impact analysis.

## RULE-035 — BLAST RADIUS

Every new component MUST document what it can access and what remains inaccessible if it is compromised.

## RULE-036 — FAIL-SAFE UPDATE

An update mechanism MUST prioritize preventing execution of unauthenticated or corrupted code over maintaining uninterrupted availability.

## RULE-037 — DEVELOPMENT/PRODUCTION SEPARATION

Development builds MUST NOT silently inherit production trust credentials.

## RULE-038 — NO TELEMETRY BY DEFAULT

Non-essential telemetry MUST be disabled by default.

## RULE-039 — PRIVACY BY DESIGN

Security mechanisms MUST minimize unnecessary identifiers and collected data.

## RULE-040 — SECURITY GATES

A component MUST NOT become part of the production trusted foundation until its defined security gates are satisfied.
