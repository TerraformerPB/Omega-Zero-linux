# IPOS Module Catalog

Dieses Dokument ist die Arbeitsgrundlage für die Zerlegung von IPOS in klar abgegrenzte Module.

## Modulprinzip

Jedes Modul erhält:
- eindeutige Verantwortung
- definierte Inputs/Outputs
- minimale Privilegien
- klare Trust Boundary
- definierte Datenflüsse
- Teststrategie
- Hardwareabhängigkeit: none, abstracted oder device-specific

Kein Modul darf Sicherheitslogik stillschweigend in ein anderes Modul verschieben.

## Plattformmodule

| ID | Modul | Aufgabe | Hardware |
|---|---|---|---|
| P-01 | Security Core | zentrale Sicherheitsdurchsetzung | abstracted |
| P-02 | Policy Engine | Default-Deny/Autorisierung | none |
| P-03 | Identity Core | Device/User/App/Service Identity | abstracted |
| P-04 | Device Abstraction Layer | Hardware-Sicherheitsfähigkeiten abstrahieren | abstracted |
| P-05 | Attestation | Gerätezustand kryptografisch nachweisen | abstracted |
| P-06 | Permission Broker | Zugriff auf geschützte Ressourcen | abstracted |
| P-07 | Session Manager | Sessions, Tokens, Ablauf, Revocation | none |
| P-08 | Network Enforcement | Egress/Ingress-Regeln | abstracted |
| P-09 | Data Flow Control | Datenflüsse zwischen Apps/Services kontrollieren | none |
| P-10 | DLP Engine | Schutz sensibler Daten gegen Export | none |
| P-11 | Security Event Engine | normalisierte Security Events | none |
| P-12 | Security Response | Quarantäne, Lock, Revocation | abstracted |
| P-13 | Recovery | sichere Wiederherstellung | abstracted |
| P-14 | Key Management Interface | Zugriff auf nicht exportierbare Schlüssel | abstracted |
| P-15 | Update Orchestrator | signierte Updates/A-B/Rollback | abstracted |
| P-16 | Integrity Manager | Systemintegrität und Messwerte | abstracted |
| P-17 | Storage Security | Verschlüsselung, Schlüsselbindung, Minimierung | abstracted |
| P-18 | Web Runtime Controller | Chromium/Web Runtime sicher betreiben | abstracted |
| P-19 | Application Sandbox | Isolation der Apps | abstracted |
| P-20 | Control Center | Benutzeroberfläche für Sicherheitsfunktionen | none |
| P-21 | Device Enrollment | sichere Geräteaufnahme | abstracted |
| P-22 | Service Authorization | Freigabe von Cloud-Services | none |
| P-23 | Certificate/Credential Manager | Zertifikate und kurzlebige Credentials | abstracted |
| P-24 | Audit/Logging | manipulationserschwerte Sicherheitsnachweise | abstracted |

## Device-Module

| ID | Modul | Aufgabe |
|---|---|---|
| D-01 | Boot Integration | Bootloader/Verified Boot |
| D-02 | Kernel Integration | Kernel, Treiber, Device Tree |
| D-03 | TEE Integration | Trusted Execution Environment |
| D-04 | KeyMint/Keystore | hardwaregebundene Schlüssel |
| D-05 | Secure Element | optionaler separater Hardware-Key-Store |
| D-06 | Attestation HAL | Hardware-Attestation |
| D-07 | Modem | Mobilfunk |
| D-08 | Connectivity | WLAN/Bluetooth/NFC |
| D-09 | Camera | Kamera |
| D-10 | Sensors | Sensoren |
| D-11 | Display/Input | Display, Touch, Buttons |
| D-12 | Audio | Audio/Mikrofon |
| D-13 | Power | Akku, Laden, Suspend/Resume |
| D-14 | Storage/Partitions | Partitionen, Verschlüsselung, Integrity |
| D-15 | Firmware | signierte Firmware-Komponenten |
| D-16 | Tamper | physische Manipulationsdetektion, sofern vorhanden |

## Cloud-/Zero-Trust-Module

| ID | Modul | Aufgabe |
|---|---|---|
| C-01 | Device Registry | Geräteidentitäten |
| C-02 | Attestation Verifier | Attestation prüfen |
| C-03 | Policy Service | zentrale Policies |
| C-04 | Authorization Service | Servicezugriff |
| C-05 | Credential Issuer | kurzlebige Credentials |
| C-06 | Revocation Service | Sperren von Geräten/Sessions |
| C-07 | Gateway | kontrollierter Zugang |
| C-08 | Audit Service | zentrale Security Events |
| C-09 | Enrollment Service | Provisioning |
| C-10 | Recovery Service | Wiederherstellung ohne Backdoor |

## Build-/Supply-Chain-Module

| ID | Modul |
|---|---|
| B-01 | Source Manifest |
| B-02 | Dependency Lock |
| B-03 | Reproducible Build |
| B-04 | SBOM |
| B-05 | Artifact Verification |
| B-06 | Release Signing |
| B-07 | Independent Rebuild |
| B-08 | Security Test Pipeline |
| B-09 | Provenance |
| B-10 | Release Gate |

## Testmodule

| ID | Test |
|---|---|
| T-01 | Boot Integrity |
| T-02 | Attestation |
| T-03 | Policy |
| T-04 | Permission |
| T-05 | Network |
| T-06 | Data Flow/DLP |
| T-07 | Sandbox |
| T-08 | Update/Rollback |
| T-09 | Recovery |
| T-10 | Cryptographic Erasure |
| T-11 | Fuzzing |
| T-12 | Supply Chain |
| T-13 | Device Portability |
| T-14 | Hardware Security |
| T-15 | E2E / Zero Alpha (kein Klartext serverseitig, Schlüsselverlust, Recovery) |

## Zero-Alpha-Module (E2E-Datenpakete)

Details: [`CLOUD_DATA_PROTECTION.md`](CLOUD_DATA_PROTECTION.md).

| ID | Modul | Aufgabe | Ort |
|---|---|---|---|
| Z-01 | Package Format | versioniertes, authentifiziertes Containerformat für Zero-Alpha-Pakete | Spezifikation |
| Z-02 | Client Crypto | Ver-/Entschlüsselung ausschließlich auf dem Gerät | Gerät |
| Z-03 | Key Hierarchy | Paket-, Benutzer- und Geräteschlüssel, Key Wrapping | Gerät (hardwaregebunden) |
| Z-04 | Sharing / Multi-Device | Schlüsselverteilung an Geräte und berechtigte Empfänger | Gerät + Cloud (nur Chiffrat) |
| Z-05 | Package Service | Speicherung, Versionierung, Bereitstellung als Service (nur Chiffrat + minimale Metadaten) | Cloud |
| Z-06 | Key Recovery | Wiederherstellung ohne Betreiberzugriff auf Klartext | Gerät + Offline-Recovery-Key |

## Priorisierung

Die Phasen entsprechen [`ROADMAP.md`](ROADMAP.md).

| Phase | Module |
|---|---|
| P0 – Foundation | P-04, P-14, P-15, P-16, P-17, D-01–D-06, B-01–B-10 |
| P1 – Security Core | P-01, P-02, P-03, P-05, P-06, P-07, P-08, P-11 |
| P2 – Web Runtime | P-09, P-18, P-19 |
| P3 – Gateway & Zero Alpha | P-21, P-22, P-23, C-01–C-09, Z-01–Z-06 |
| P4 – Control Center | P-20 |
| P5 – Security Response | P-10, P-12, P-13, P-24, C-10, D-16 |
| P7 – Verification | T-01–T-15 |
| P8 – Hardware | D-05, Privacy Controls, Hardware Kill Switches |

## Modulregel

Ein Modul gilt erst als abgeschlossen, wenn:
- Schnittstelle dokumentiert
- Trust Boundary dokumentiert
- Threats dokumentiert
- Tests vorhanden
- Security Review abgeschlossen
- Hardwareabhängigkeiten gekennzeichnet
- Logging/Audit-Verhalten definiert
- Fehlerverhalten definiert
- Fail-Closed-Verhalten geprüft