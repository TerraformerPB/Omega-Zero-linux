# Component Trust Registry

Every component entering the IPOS trusted foundation receives a record.

## Status values

- UNKNOWN
- REVIEW
- SECURITY_TEST
- AUDITED
- APPROVED
- TRUSTED_FOUNDATION
- REJECTED
- DEPRECATED

## Required fields

~~~yaml
component:
version:
source:
license:
build:
dependencies:
binary_inputs:
network_access:
privileges:
security_history:
audit_status:
reproducibility:
sbom:
maintainer:
review_status:
approved_by:
notes:
~~~

## Initial candidates

| Component | Intended role | Initial status |
|---|---|---|
| Linux kernel | kernel | REVIEW |
| SELinux | mandatory access control | REVIEW |
| dm-verity | filesystem integrity | REVIEW |
| Android Verified Boot | boot integrity | REVIEW |
| KeyMint/Keystore | hardware-backed keys/attestation | REVIEW |
| Chromium | controlled web runtime | REVIEW |
| A/B update mechanism | secure updates | REVIEW |

No candidate is considered trusted merely because it is widely used.
