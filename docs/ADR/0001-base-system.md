# ADR-0001: Base System Strategy

## Decision

IPOS will initially evaluate an **AOSP/Linux-derived architecture** rather than building a smartphone operating system from a generic Linux distribution.

The kernel remains Linux-based. Android security components and established mobile mechanisms are preferred where they reduce the amount of security-critical code that IPOS must invent.

## Rationale

Smartphone security depends on more than a kernel. The platform needs an integrated chain covering:

- hardware-backed key storage
- verified boot
- attestation
- filesystem/data encryption
- SELinux
- application isolation
- mobile hardware integration
- update and rollback mechanisms

AOSP already documents and implements mature mechanisms in these areas. Verified Boot establishes a cryptographic chain from hardware root of trust through boot and system partitions, while hardware-backed Keystore/KeyMint can expose attestation information about keys and boot state. citeturn0search1turn0search3

Linux-native mechanisms such as dm-verity and reproducible kernel builds remain relevant to the foundation. citeturn0search4turn0search0

## Consequence

The first prototype will not attempt to replace:
- Linux kernel security primitives
- SELinux
- established verified-boot mechanisms
- hardware-backed key services
- standard cryptographic libraries

Instead, IPOS will build its differentiation in:
- policy enforcement
- device/service identity
- zero-trust authorization
- controlled web runtime
- data-flow control
- security response
- reproducible/open verification

## Rejected alternatives

### Generic Ubuntu/Debian smartphone distribution

Rejected as the first production-oriented baseline because the mobile hardware/security integration burden would be substantially higher.

### Completely custom kernel/security stack

Rejected because it would unnecessarily enlarge the trusted computing base and introduce new security-critical code.

### Fully custom cryptographic stack

Rejected categorically.
