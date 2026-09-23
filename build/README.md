# Reproducible Build System

The build system is a security component.

## Required properties

1. Pinned toolchain
2. Pinned dependencies
3. Hermetic build inputs where practical
4. Recorded source commit
5. Recorded build environment
6. SBOM
7. Cryptographic artifact hashes
8. Signature generation outside ordinary development CI
9. Independent rebuild
10. Published verification instructions

## Initial workflow

~~~text
Git commit
   ↓
Source manifest
   ↓
Hermetic build
   ↓
Security tests
   ↓
SBOM
   ↓
Artifact hash
   ↓
Independent rebuild
   ↓
Compare
   ↓
Release signing
~~~

The Linux kernel build system documents several sources of non-reproducibility, including timestamps, build host/user data, absolute paths and module signing. These must be controlled explicitly. citeturn0search0
