# IPOS / Omega-Zero – Programmablaufpläne

Status: DRAFT
Die Diagramme sind in Mermaid geschrieben und werden auf GitHub direkt dargestellt.

Grundregel für alle Abläufe (RULE-017, Fail Closed): **Jeder unbekannte, ungültige, fehlende, abgelaufene oder widerrufene Zustand führt zu DENY.** Jede sicherheitsrelevante Entscheidung erzeugt ein datensparsames Audit-Event (RULE-029).

## Übersicht

| # | Ablauf | Module |
|---|---|---|
| 1 | Secure Boot / Verified Boot | D-01, P-16 |
| 2 | Geräteregistrierung (Enrollment) | P-21, C-09, C-02 |
| 3 | Zugriffsentscheidung (Zero Trust) | P-01–P-09, C-03, C-04 |
| 4 | Zero Alpha – Paket schreiben und lesen | Z-01–Z-05 |
| 5 | Update (A/B) | P-15 |
| 6 | Security Response / Notfall | P-11, P-12, C-06 |
| 7 | Schwachstellen- und Vorfallmeldung (CRA/DSGVO) | Prozess |
| 8 | Gerätewechsel und Recovery | P-13, Z-06, C-10 |

---

## 1. Secure Boot / Verified Boot

```mermaid
flowchart TD
    A([Einschalten]) --> B["Boot ROM prüft Bootloader-Signatur"]
    B -->|ungültig| X1["Boot abbrechen"]
    B -->|gültig| C{"Bootloader gesperrt?"}
    C -->|nein| D["Zustand UNLOCKED: Entwicklungsprofil, keine Produktiv-Credentials (RULE-037)"]
    C -->|ja| E["Verified Boot: vbmeta, Kernel, System per dm-verity prüfen"]
    E -->|Fehler| X2["Recovery / Slot-Wechsel; Attestation meldet Fehler"]
    E -->|ok| F{"Rollback-Index ≥ gespeicherter Index?"}
    F -->|nein| X3["Boot verweigern (RULE-022)"]
    F -->|ja| G["Kernel startet, SELinux enforcing"]
    G --> H["Security Core startet, Integrity Manager misst Zustand"]
    H --> I([System bereit])
```

## 2. Geräteregistrierung (Enrollment)

```mermaid
flowchart TD
    A([Neues Gerät]) --> B["Gerät erzeugt Geräteschlüssel im TEE/KeyMint (nicht exportierbar)"]
    B --> C["Enrollment Service sendet Nonce"]
    C --> D["Gerät erstellt Key-Attestation mit Nonce"]
    D --> E{"Attestation Verifier: Kette, Nonce, Boot-Zustand, AVB-Key-Hash, Patchlevel ok?"}
    E -->|nein| X1["DENY, Audit-Event"]
    E -->|ja| F["Benutzer authentifiziert sich (Passkey)"]
    F -->|fehlgeschlagen| X1
    F -->|ok| G["Policy zuweisen, Target-Status prüfen (TRUSTED / LIMITED)"]
    G --> H["Device Registry: Gerät mit zweckgebundener Kennung eintragen"]
    H --> I["Zero Alpha: Key Slots der Pakete für neues Gerät ergänzen (durch bereits berechtigtes Gerät)"]
    I --> J([Gerät aktiv])
```

## 3. Zugriffsentscheidung (Zero Trust)

```mermaid
flowchart TD
    A([App fordert Zugriff auf Service an]) --> B{"Gerät gesperrt oder widerrufen?"}
    B -->|ja| DENY["DENY + Audit-Event"]
    B -->|nein| C{"Attestation frisch (Nonce, innerhalb TTL)?"}
    C -->|nein| C2["Re-Attestation"]
    C2 -->|fehlgeschlagen| DENY
    C2 -->|ok| D
    C -->|ja| D{"Boot-Zustand, AVB-Key, Patchlevel zulässig?"}
    D -->|nein| Q["Quarantäne + DENY"]
    D -->|ja| E{"User-Session gültig?"}
    E -->|nein| E2["Re-Authentifizierung (Passkey)"]
    E2 -->|fehlgeschlagen| DENY
    E2 -->|ok| F
    E -->|ja| F{"App-Identität gültig und Policy erlaubt Aktion?"}
    F -->|nein| DENY
    F -->|ja| G{"Datenfluss / Klassifikation erlaubt?"}
    G -->|nein| DENY
    G -->|ja| H{"Netzwerk-Policy erlaubt Ziel, Protokoll, Port?"}
    H -->|nein| DENY
    H -->|ja| ALLOW["ALLOW: kurzlebiges, an Gerät + App + Service gebundenes Token"]
    ALLOW --> L["Audit-Event (signiert, datensparsam)"]
```

## 4. Zero Alpha – Paket schreiben und lesen

```mermaid
flowchart TD
    subgraph Schreiben
    W1([App erzeugt Daten]) --> W2["Klassifikation und Policy bestimmen"]
    W2 --> W3["Zufälligen Paketschlüssel erzeugen"]
    W3 --> W4["Inhalt mit AEAD verschlüsseln"]
    W4 --> W5["Paketschlüssel je berechtigtem Gerät und Recovery-Key einwickeln (HPKE)"]
    W5 --> W6["Paket signieren, Versionszähler erhöhen"]
    W6 --> W7["Upload: Cloud speichert nur Chiffrat + minimale Metadaten"]
    end
    subgraph Lesen
    R1([Anfrage Paket]) --> R2["Zugriffsentscheidung (Ablauf 3)"]
    R2 -->|DENY| RX["Abbruch"]
    R2 -->|ALLOW| R3["Download Chiffrat"]
    R3 --> R4{"Signatur gültig, Version nicht älter als bekannt?"}
    R4 -->|nein| RX2["Verwerfen, Security-Event (Manipulation / Rollback)"]
    R4 -->|ja| R5["Key Slot mit Geräteschlüssel im TEE öffnen"]
    R5 -->|kein Slot| RX
    R5 -->|ok| R6["Entschlüsseln nur auf dem Gerät, Anzeige in App-Sandbox"]
    end
```

## 5. Update (A/B)

```mermaid
flowchart TD
    A([Update verfügbar]) --> B["Download in inaktiven Slot"]
    B --> C{"Signatur und Hash gültig?"}
    C -->|nein| X1["Verwerfen, Security-Event"]
    C -->|ja| D{"Version ≥ Anti-Rollback-Index?"}
    D -->|nein| X1
    D -->|ja| E["Installation im inaktiven Slot"]
    E --> F["Neustart in neuen Slot"]
    F --> G{"Health Check ok?"}
    G -->|nein| H["Automatischer Rollback auf alten Slot"]
    G -->|ja| I["Slot als erfolgreich markieren, Rollback-Index erhöhen"]
    I --> J([Update abgeschlossen, Attestation meldet neue Version])
```

## 6. Security Response / Notfall

```mermaid
flowchart TD
    A([Security-Event]) --> B["Event normalisieren und bewerten"]
    B --> C{"Schwere"}
    C -->|niedrig| L0["Level 0: protokollieren"]
    C -->|mittel| L1["Level 1: Gerät sperren"]
    C -->|hoch| L2["Level 2–3: Sessions widerrufen, Netzwerk isolieren"]
    C -->|kritisch| L4["Level 4: Geräte-Credentials widerrufen, Key Slots entfernen"]
    L4 --> D{"Zerstörende Aktion (Level 5–6) angefordert?"}
    D -->|nein| FIN
    D -->|ja| E{"Starke Authentifizierung + Mehrpersonen-Freigabe (RULE-043)?"}
    E -->|nein| X["Ablehnen, Audit-Event, Eigentümer informieren"]
    E -->|ja| F["Level 5: Cryptographic Wipe / Level 6: Full Wipe"]
    F --> FIN([Audit-Event, Incident-Prozess Ablauf 7])
    L0 --> FIN
    L1 --> FIN
    L2 --> FIN
```

## 7. Schwachstellen- und Vorfallmeldung (CRA / DSGVO)

```mermaid
flowchart TD
    A([Meldung oder Erkennung]) --> B["Eingang über privaten Kanal (SECURITY.md), Bestätigung an Melder"]
    B --> C["Triage: Komponente, Schweregrad, Ausnutzung"]
    C --> D{"Aktiv ausgenutzt oder schwerwiegender Vorfall?"}
    D -->|ja| E["CRA: Frühwarnung ≤ 24 h, Meldung ≤ 72 h über ENISA-Plattform an CSIRT (BSI)"]
    D -->|nein| F["Behebung nach Priorität"]
    E --> F
    C --> G{"Personenbezogene Daten betroffen?"}
    G -->|ja| H["DSGVO: Verantwortlicher meldet ≤ 72 h an Aufsichtsbehörde (Art. 33); Betroffene nach Art. 34"]
    G -->|nein| F
    H --> F
    F --> I["Fix, Review (Vier Augen), signiertes Update"]
    I --> J["Security Advisory / CVE veröffentlichen"]
    J --> K["CRA-Abschlussbericht (Schwachstelle: ≤ 14 Tage nach Fix)"]
    K --> L([Lessons Learned, Regressionstest])
```

## 8. Gerätewechsel und Recovery

```mermaid
flowchart TD
    A([Gerätewechsel oder Verlust]) --> B{"Altes Gerät verfügbar?"}
    B -->|ja| C["Altes Gerät autorisiert neues Gerät: Key Slots ergänzen"]
    B -->|nein| D["Altes Gerät widerrufen: Sessions, Credentials, Key Slots"]
    D --> E["Neues Gerät: Enrollment (Ablauf 2)"]
    E --> F{"Recovery-Key vorhanden?"}
    F -->|nein| G{"Weiteres berechtigtes Gerät vorhanden?"}
    G -->|nein| X["Daten nicht wiederherstellbar (E2E-09) – kein Betreiber-Master-Key"]
    G -->|ja| C
    F -->|ja| H["Recovery-Key entschlüsselt Paketschlüssel lokal, neue Key Slots erzeugen"]
    C --> I["Schlüsselrotation (key_epoch) für künftige Versionen"]
    H --> I
    I --> J([Cloud-Daten verfügbar, altes Gerät ohne Zugriff])
```
