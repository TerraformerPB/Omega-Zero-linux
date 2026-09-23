# Omega-Zero-linux

Omega-Zero-linux is the open-source foundation for the Omega security ecosystem.

## Vision

Build a verifiable, minimal, zero-trust mobile computing platform:

**Hardware Root of Trust → Verified Boot → Minimal OS → IPOS Security Core → Web Runtime → Omega-Zero → Cloud Services**

IPOS is intended to function primarily as a secure thin client and hardware-backed authenticator. Valuable application data should remain in controlled services whenever practical.

## Current status

**Phase: P0 Foundation / Architecture**

This repository currently contains the security and architecture foundation. No production device should be considered trusted yet.

## Core principles

- Default deny
- Least privilege
- Zero Trust
- Hardware-backed identity
- Verified boot
- Continuous/periodic attestation
- Strong isolation
- Data minimisation
- Reproducible builds
- Signed releases
- Supply-chain transparency
- No custom cryptography
- Open security review

## Repository structure

- `docs/` – architecture, threat model and security specifications
- `codex/` – normative machine-readable project rules
- `os/` – future IPOS operating-system implementation
- `security/` – future security-core implementation
- `network/` – future gateway and network-policy implementation
- `runtime/` – future controlled web runtime
- `cloud/` – self-hostable Omega-Zero components
- `hardware/` – reference-device and Omega hardware specifications
- `build/` – reproducible build and signing infrastructure
- `tests/` – security, fuzzing and integration tests
- `targets/` – virtual and physical device targets
- `docs/DEVICE_PORTABILITY.md` – hardware-independent platform and device abstraction strategy
- `docs/MODULE_CATALOG.md` – normative module inventory and implementation priorities

## Security status

This is a research/development project. Until independent security review, reproducible-build verification and hardware validation are complete, the software must not be treated as a security-certified product.

See [SECURITY.md](SECURITY.md).

## Device Portability

IPOS is designed as a hardware-independent platform with a strict Device Abstraction Layer. Cuttlefish is the virtual development target and the Pixel 8a is the initial physical reference target. Device-specific hardware integration must remain isolated from the platform security core.

See [docs/DEVICE_PORTABILITY.md](docs/DEVICE_PORTABILITY.md) and [docs/MODULE_CATALOG.md](docs/MODULE_CATALOG.md).
