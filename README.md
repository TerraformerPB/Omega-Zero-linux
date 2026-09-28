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
- `codex/` – normative project rules (a machine-readable YAML form is planned)
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

## Cloud data protection & Zero Alpha

Cloud data is end-to-end encrypted. **Zero Alpha** is the end-to-end encrypted data package of the Omega ecosystem that is offered as a service: the cloud stores and serves only ciphertext and minimal metadata, while decryption happens exclusively on authorized devices.

See [docs/CLOUD_DATA_PROTECTION.md](docs/CLOUD_DATA_PROTECTION.md).

## Documentation

| Document | Content |
|---|---|
| [docs/IPOS_SECURITY_ARCHITECTURE.md](docs/IPOS_SECURITY_ARCHITECTURE.md) | overall architecture |
| [docs/THREAT_MODEL.md](docs/THREAT_MODEL.md) | assets, attackers, trust boundaries |
| [docs/TRUST_MODEL.md](docs/TRUST_MODEL.md) | trust hierarchy and access decision |
| [codex/IPOS_SECURITY_CODEX.md](codex/IPOS_SECURITY_CODEX.md) | normative security rules |
| [docs/ROADMAP.md](docs/ROADMAP.md) | authoritative phase plan |
| [docs/MODULE_CATALOG.md](docs/MODULE_CATALOG.md) | module inventory |
| [docs/FLOWCHARTS.md](docs/FLOWCHARTS.md) | program flowcharts (boot, enrollment, access, Zero Alpha, update, response, disclosure, recovery) |
| [docs/CLOUD_DATA_PROTECTION.md](docs/CLOUD_DATA_PROTECTION.md) | end-to-end encryption and Zero Alpha |
| [docs/COMPLIANCE.md](docs/COMPLIANCE.md) | German/EU legal overview (licences, CRA, GDPR, BetrVG, …) |

## Security status

This is a research/development project. Until independent security review, reproducible-build verification and hardware validation are complete, the software must not be treated as a security-certified product.

See [SECURITY.md](SECURITY.md).

## Device Portability

IPOS is designed as a hardware-independent platform with a strict Device Abstraction Layer. Cuttlefish is the virtual development target and the Pixel 8a is the initial physical reference target. Device-specific hardware integration must remain isolated from the platform security core.

See [docs/DEVICE_PORTABILITY.md](docs/DEVICE_PORTABILITY.md) and [docs/MODULE_CATALOG.md](docs/MODULE_CATALOG.md).

## License

Licensed under the [Apache License, Version 2.0](LICENSE). See [NOTICE](NOTICE).

Third-party components keep their own licenses (e.g. the Linux kernel remains GPL-2.0-only). See [docs/COMPLIANCE.md](docs/COMPLIANCE.md).
