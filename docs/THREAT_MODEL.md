# IPOS Threat Model

## 1. Purpose

This document defines what IPOS/Omega-Zero must protect, which attackers are considered, which trust boundaries exist and which security properties must be demonstrated.

The threat model is normative for architecture decisions. A feature that materially violates these assumptions requires an explicit security review.

## 2. Assets

### A0 — Hardware root of trust

Examples:
- boot verification keys
- secure-element secrets
- device-bound private keys
- hardware attestation keys

Required property: private root credentials must not be exportable through normal software interfaces.

### A1 — Device identity

The unique cryptographic identity of an IPOS device.

### A2 — User identity

Credentials, passkeys and authentication state.

### A3 — Service identity

Credentials used by Omega-Zero services and gateways.

### A4 — Application data

Finance, mail, ERP and other service data.

### A5 — Security policy

Rules determining which identities may access which resources.

### A6 — Software integrity

Bootloader, kernel, security core, runtime and system partitions.

### A7 — Update trust

Release metadata, update signatures, rollback state and update artifacts.

### A8 — Security telemetry

Integrity/security events needed for incident response.

## 3. Security properties

IPOS must preserve:

1. Authenticity – identities cannot be silently substituted.
2. Integrity – unauthorized software changes are detected.
3. Confidentiality – protected data and keys are inaccessible to unauthorized principals.
4. Availability – recovery and rollback prevent a single failed update from permanently bricking a device where technically possible.
5. Isolation – compromise of one application does not automatically compromise another.
6. Policy integrity – applications cannot weaken their own authorization.
7. Auditability – security-critical decisions produce verifiable evidence without unnecessary personal data.
8. Recoverability – a lost or compromised device can be revoked without destroying the user's cloud identity.

## 4. Attacker classes

### T1 — Remote unauthenticated attacker

Capabilities:
- sends network traffic
- probes exposed endpoints
- exploits remotely reachable vulnerabilities

Primary controls:
- minimal network exposure
- default deny
- TLS/mTLS
- gateway filtering
- rate limiting
- service isolation
- secure patching

### T2 — Malicious application / compromised web application

Capabilities:
- execute within its sandbox
- attempt privilege escalation
- attempt cross-app access
- attempt unauthorized network access

Primary controls:
- process/site isolation
- sandbox
- SELinux/LSM policy
- application identity
- network policy
- data-flow policy
- permission broker

### T3 — Compromised cloud service

Capabilities:
- valid service credentials
- access only to its explicitly authorized backend resources

Primary controls:
- separate service identities
- least privilege
- short-lived credentials
- service-to-service authorization
- independent audit logs

### T4 — Stolen powered-off device

Capabilities:
- physical possession
- offline storage attacks
- repeated authentication attempts
- hardware extraction attempts

Primary controls:
- full-disk/file-based encryption
- hardware-bound keys
- secure lock-screen credential derivation
- rate limiting
- secure boot
- cryptographic erasure

### T5 — Stolen unlocked device

This is a substantially stronger attacker.

Controls must include:
- short-lived service sessions
- rapid device revocation
- app isolation
- protected credentials
- re-authentication for high-value operations
- remote policy changes
- minimal persistent data

### T6 — Physical attacker with laboratory capabilities

Capabilities may include:
- board access
- debug interfaces
- flash extraction
- fault injection
- side-channel attempts
- component replacement

This threat cannot be eliminated purely in software.

Required design response:
- secure element/TEE
- debug lockdown
- tamper evidence/detection where feasible
- hardware-backed keys
- secure boot
- physical attack analysis
- explicit hardware security assumptions

### T7 — Supply-chain attacker

Targets:
- source repository
- dependency
- compiler/build host
- CI
- signing infrastructure
- release artifact

Controls:
- pinned dependencies
- SBOM
- provenance
- isolated build environments
- reproducible builds
- independent rebuilds
- protected signing keys
- four-eyes review

### T8 — Malicious maintainer

Controls:
- branch protection
- mandatory reviews
- protected release process
- separation of development/build/signing
- auditable changes
- independent verification

### T9 — Compromised update infrastructure

Controls:
- offline/isolated root of trust
- signed updates
- anti-rollback
- independent verification
- A/B rollback
- key rotation/revocation

## 5. Trust boundaries

### TB0 — Hardware ↔ boot chain

Highest-trust boundary.

### TB1 — Boot chain ↔ kernel

The kernel must only execute after verification.

### TB2 — Kernel/security enforcement ↔ userspace

Userspace is not trusted merely because it is installed by IPOS.

### TB3 — Security core ↔ applications

Applications request authority; they do not define their own authority.

### TB4 — Device ↔ gateway

Device identity and current integrity state must be evaluated.

### TB5 — Gateway ↔ cloud service

Every service connection requires explicit authorization.

### TB6 — Build system ↔ signing system

Signing must be isolated from ordinary CI.

## 6. Threat matrix

| Threat | Asset | Control | Detection | Recovery |
|---|---|---|---|---|
| Remote RCE | OS/service | sandbox, patching | telemetry | isolate/update |
| App escape | other apps | SELinux/sandbox | integrity events | kill/revoke |
| Credential theft | identity | hardware keys | attestation/session events | revoke |
| Offline extraction | local data | encryption | tamper signals | key destruction |
| Supply-chain injection | binaries | reproducible builds | hash mismatch | revoke release |
| Malicious update | OS | signed A/B update | verification failure | rollback |
| Cloud credential abuse | service identity | short-lived scoped credentials | anomaly/policy logs | revoke |
| Physical debug | device keys | hardware lockdown | boot/attestation | restricted state |

## 7. Security assumptions

The following are explicit assumptions rather than guarantees:

- hardware root of trust is correctly implemented
- secure hardware itself can contain vulnerabilities
- radio/baseband firmware may have separate trust boundaries
- a compromised kernel can bypass ordinary userspace controls
- cloud services can be compromised
- zero trust does not prevent every attack; it limits blast radius
- physical attacks can exceed the threat model
- availability cannot always be guaranteed

## 8. Non-goals

IPOS does not claim to:
- make a compromised remote service safe
- make insecure hardware secure by software alone
- guarantee anonymity
- guarantee immunity against unknown vulnerabilities
- provide absolute security

## 9. Required security review gates

No production release before:

- boot-chain review
- hardware-key review
- kernel/LSM review
- policy-engine review
- update-system review
- supply-chain review
- independent rebuild
- penetration testing
- fuzzing
- external security review
