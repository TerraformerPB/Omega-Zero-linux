# IPOS Device Portability & Device Abstraction

## Zweck

IPOS wird nicht als einzelnes, an ein Gerät gebundenes Betriebssystem entwickelt. Die sicherheitskritische Plattform muss möglichst hardwareunabhängig bleiben; gerätespezifische Implementierungen werden über definierte Device-Abstraction-Schnittstellen eingebunden.

## Referenzziele

| Target | Rolle | Status |
|---|---|---|
| AOSP Cuttlefish | virtuelles Entwicklungs-/CI-Target | geplant |
| Google Pixel 8a | physisches Referenzgerät R1 | geplant |
| Weitere Pixel/Android-Geräte | Portierungsziele | geplant |
| Omega Hardware | eigenes zertifiziertes Gerät | langfristig |

Cuttlefish ist für AOSP insbesondere deshalb geeignet, weil es Framework-/Plattformcode ohne permanente Abhängigkeit von physischer Hardware testen kann. Die wesentlichen Unterschiede zu realer Hardware liegen laut AOSP vor allem in HALs und hardwarespezifischer Software. citeturn0search0turn0search1

## Architektur

    IPOS PLATFORM
          |
    Device Abstraction Layer
          |
    +-----+---------+
    |               |
 Cuttlefish       Pixel 8a       Omega Device
    |               |               |
 Virtual HAL    Vendor/HAL      Omega HAL
    |               |               |
    +---------------+---------------+
                    |
                 Hardware

Der Plattformcode darf keine gerätespezifischen Sonderfälle enthalten.

## Verantwortungsgrenze

### IPOS Platform
- Security Core
- Policy Engine
- Identity
- Permission Broker
- Attestation Client
- Session Manager
- Network Enforcement
- Data Flow Control / DLP
- Security Response
- Recovery
- Update Orchestration
- Web Runtime Policy
- Security Event Model

### Device Support
- Boot Chain Integration
- Device Tree
- Kernel Configuration
- Vendor/HAL
- KeyMint/Keystore/TEE-Anbindung
- Secure Element
- Hardware Attestation
- Modem
- WLAN/Bluetooth
- Kamera
- Display
- Audio
- Sensoren
- Power Management
- Partitionierung
- Firmware
- gerätespezifische SELinux-Regeln

## Device Security Interface

Der Security Core erhält nur definierte Fähigkeiten, niemals direkten Zugriff auf gerätespezifische Implementierungsdetails.

Beispielhafte abstrakte Operationen:

    get_device_identity()
    get_hardware_security_state()
    get_security_capabilities()
    verify_boot_state()
    get_integrity_state()
    attest_device(challenge)
    seal_key(key_reference, policy)
    unseal_key(key_reference, policy)
    revoke_device()

Die tatsächliche API wird in einer separaten Spezifikation formalisiert.

## Security Capability Profile

Jedes Target muss seine Fähigkeiten deklarieren und nachweisen.

Beispiel:

    target: pixel8a
    security_profile:
      secure_boot: required
      verified_boot: required
      hardware_key_storage: required
      tee: required
      hardware_attestation: required
      anti_rollback: required
      secure_element: preferred
      tamper_detection: optional
      hardware_privacy_switches: optional

`required` bedeutet: Ohne Nachweis keine Zertifizierung als voll vertrauenswürdiges IPOS-Target.
`preferred` verbessert die Sicherheitsbewertung, ist aber nicht automatisch Voraussetzung.
`optional` darf nicht als vorhandene Fähigkeit angenommen werden.

## Security Profiles

Ein Gerät erhält einen expliziten Status:
- UNSUPPORTED
- EXPERIMENTAL
- LIMITED
- TRUSTED
- CERTIFIED

Ein Gerät darf nur mit nachgewiesenen Fähigkeiten auf kritische Services zugreifen.

## Portierungsregel

    Device Candidate
        ↓
    Hardware Inventory
        ↓
    Boot / Integrity Review
        ↓
    Security Capability Profile
        ↓
    Device Abstraction Implementation
        ↓
    Platform Test Suite
        ↓
    Hardware Security Tests
        ↓
    Security Review
        ↓
    Trusted / Certified Target

## Cuttlefish-first

Neue Security-Core-Funktionen sollen zuerst auf Cuttlefish implementiert und getestet werden, sofern keine echte Hardwareabhängigkeit besteht.

Cuttlefish unterstützt eigene Gerätekonfigurationen und kann mehrere virtuelle Geräte parallel betreiben, was für reproduzierbare CI-/Integrationstests genutzt werden kann. citeturn0search6turn0search9

Hardwareabhängige Funktionen werden anschließend auf dem Referenzgerät validiert.

## Hybrid Testing

Wo sinnvoll soll später geprüft werden, ob Cuttlefish Hybrid Devices eingesetzt werden können. AOSP beschreibt CHDs als Möglichkeit, ein physisches System-Image auf Cuttlefish-HALs zu testen und damit Hardware-unabhängige Systemsoftware früher zu validieren. citeturn0search2

## Ziel

Ein Entwickler soll für ein neues Smartphone nicht das gesamte IPOS-System portieren müssen.

Er soll primär:
1. Hardware dokumentieren.
2. Security Capability Profile erstellen.
3. Device-Abstraction-Implementierung bereitstellen.
4. Hardware-/Vendor-Integration durchführen.
5. Security Gates bestehen.
6. Das Gerät durch den Zertifizierungsprozess führen.

Die Sicherheitsarchitektur bleibt dabei identisch.