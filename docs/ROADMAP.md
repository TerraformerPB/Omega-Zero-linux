# Omega-Zero / IPOS Roadmap

## P0 — Foundation

### P0.1 Architecture
- [x] Initial security architecture
- [x] Threat model
- [x] Trust model
- [x] Security Codex
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

## P3 — Omega-Zero Gateway

- [ ] device enrollment API
- [ ] device attestation verification
- [ ] service authorization
- [ ] short-lived credentials
- [ ] revocation
- [ ] audit events
- [ ] self-hosted deployment

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
