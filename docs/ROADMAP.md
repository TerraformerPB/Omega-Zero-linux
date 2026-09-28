# Omega-Zero / IPOS Roadmap

This roadmap is the single authoritative source for project phases. Other documents reference it.

## P0 — Foundation

### P0.1 Architecture
- [x] Initial security architecture
- [x] Threat model
- [x] Trust model
- [x] Security Codex
- [x] Licence decision (Apache-2.0)
- [x] Compliance overview (`docs/COMPLIANCE.md`)
- [x] Program flowcharts (`docs/FLOWCHARTS.md`)
- [ ] Formal component inventory
- [ ] Hardware target decision
- [ ] Threat-model review

### P0.2 Build foundation
- [ ] Source manifest
- [ ] pinned toolchain
- [ ] reproducible build container
- [ ] SBOM generation
- [ ] artifact hashing
- [ ] signature infrastructure
- [ ] independent rebuild workflow

### P0.3 OS foundation
- [ ] select Linux/Android base
- [ ] boot-chain design
- [ ] kernel configuration baseline
- [ ] SELinux/LSM baseline
- [ ] filesystem encryption design
- [ ] dm-verity/integrity strategy
- [ ] minimal userspace

### P0.4 Hardware
- [ ] reference development device
- [ ] boot ROM/root-of-trust documentation
- [ ] TEE/secure-element capability matrix
- [ ] hardware attestation capability
- [ ] debug-port policy

### P0.5 Device Portability
- [ ] Device Abstraction Layer specification
- [ ] Security Capability Profile schema
- [ ] target lifecycle and certification states
- [ ] Cuttlefish target
- [ ] Pixel 8a target
- [ ] new-device porting guide
- [ ] hardware/platform boundary enforcement
- [ ] multi-target CI

### P0.6 Module Architecture
- [ ] platform module contracts
- [ ] device module contracts
- [ ] cloud/Zero-Trust module contracts
- [ ] build/supply-chain module contracts
- [ ] security test module contracts
- [ ] dependency graph and trust boundaries

## P1 — Security Core

- [ ] Device Identity
- [ ] User Identity
- [ ] Application Identity
- [ ] Service Identity
- [ ] Policy Engine
- [ ] Permission Broker
- [ ] Attestation Client
- [ ] Session Manager
- [ ] Network Enforcement
- [ ] Security Event Model

## P2 — Web Runtime

- [ ] Chromium architecture decision
- [ ] isolated web-app profiles
- [ ] sandbox model
- [ ] permission integration
- [ ] network policy integration
- [ ] controlled updates

## P3 — Omega-Zero Gateway & Zero Alpha

- [ ] device enrollment API
- [ ] device attestation verification
- [ ] service authorization
- [ ] short-lived credentials
- [ ] revocation
- [ ] audit events
- [ ] self-hosted deployment
- [ ] Zero Alpha package format specification (Z-01)
- [ ] client-side encryption and key hierarchy (Z-02, Z-03)
- [ ] multi-device and sharing key distribution (Z-04)
- [ ] Zero Alpha package service – ciphertext only (Z-05)
- [ ] key recovery without operator access (Z-06)
- [ ] verification of existing cloud E2E implementation against `CLOUD_DATA_PROTECTION.md`

## P4 — Control Center

- [ ] Security Center
- [ ] Device
- [ ] Identity
- [ ] Applications
- [ ] Permissions
- [ ] Network
- [ ] Sessions
- [ ] Updates
- [ ] Recovery
- [ ] Emergency

## P5 — Security Response

- [ ] tamper events
- [ ] quarantine
- [ ] session revocation
- [ ] credential revocation
- [ ] cryptographic wipe
- [ ] recovery workflow
- [ ] incident evidence
- [ ] DLP engine

## P6 — Applications

- [ ] Office
- [ ] Mail
- [ ] Finance
- [ ] Maps
- [ ] ERP
- [ ] other Omega services

## P7 — Verification

- [ ] unit security tests
- [ ] integration tests
- [ ] fuzzing
- [ ] sandbox escape testing
- [ ] network policy testing
- [ ] supply-chain testing
- [ ] reproducible-build verification
- [ ] external audit

## P8 — Hardware

- [ ] reference board
- [ ] secure-element integration
- [ ] hardware switches
- [ ] tamper detection
- [ ] production boot keys
- [ ] device certification

## P9 — Omega-Zero Open Source

- [ ] Zero-Trust gateway release
- [ ] identity/policy infrastructure
- [ ] self-hosting guide
- [ ] infrastructure as code

## P10 — Ecosystem

- [ ] Omega Secure Device
- [ ] IPOS-verified hardware and services programme
- [ ] public audits
- [ ] developer ecosystem

## Release gates

No production security release before:
1. threat-model approval
2. trusted-component review
3. boot-chain verification
4. key-management review
5. update-system review
6. supply-chain review
7. reproducible-build verification
8. security test suite
9. external security review
10. documented residual risks
11. legal/compliance review (`docs/COMPLIANCE.md`), incl. CRA conformity and data-protection impact assessment
