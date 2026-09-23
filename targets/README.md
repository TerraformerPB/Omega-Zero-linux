# IPOS Targets

Targets bilden konkrete virtuelle oder physische Geräte ab.

## Struktur

    targets/
    ├── virtual/
    │   └── cuttlefish/
    └── physical/
        ├── pixel8a/
        └── omega1/

## Regel

Hardwareabhängige Implementierungen gehören in ein Target und nicht in den hardwareunabhängigen IPOS-Plattformkern.

## Target Lifecycle

    UNKNOWN → EXPERIMENTAL → LIMITED → TRUSTED → CERTIFIED

Jedes Target benötigt ein Security Capability Profile.

## Referenzziele

- Cuttlefish: virtuelles Plattform-/CI-Ziel
- Pixel 8a: physisches Referenzgerät R1
- Omega 1: langfristiges eigenes Hardwareziel

Cuttlefish soll als erstes Plattformziel dienen; physische Hardware validiert anschließend die hardwareabhängigen Schichten.