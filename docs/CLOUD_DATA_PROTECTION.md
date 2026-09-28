# Cloud Data Protection & Zero Alpha

Status: DRAFT / NORMATIVE REQUIREMENTS
Bezug: Codex RULE-041, RULE-042; Threat Model A4, A4a, T10, TB7

## 1. Grundsatz

Die Omega Cloud speichert Nutzdaten **ausschließlich Ende-zu-Ende-verschlüsselt**. Ver- und Entschlüsselung finden nur auf berechtigten Endgeräten (IPOS-Geräten) statt. Cloud-Betreiber, Gateway und Administratoren sehen nur Chiffrat und dokumentierte, minimierte Metadaten.

> Die Cloud ist für die **Vertraulichkeit und Integrität der Inhalte nicht vertrauenswürdig**. Sie ist nur für Verfügbarkeit zuständig.

Damit wird der Widerspruch zwischen „Minimize Trust“ und „wertvolle Daten liegen in der Cloud“ aufgelöst: Der Wert liegt in der Cloud, das Vertrauen nicht.

## 2. Zero Alpha

**Zero Alpha** ist das Ende-zu-Ende-verschlüsselte Datenpaket des Omega-Ökosystems, das sich als Service anbietet: Ein Zero-Alpha-Paket bündelt verschlüsselte Nutzdaten mit den Informationen, die berechtigte Endgeräte zum Entschlüsseln und Verarbeiten benötigen, und wird von der Cloud als Service bereitgestellt – ohne dass die Cloud den Inhalt lesen kann.

    ┌──────────────────── Zero-Alpha-Paket ────────────────────┐
    │ Header (Klartext, minimal)                                │
    │   format_version, package_id (zufällig, nicht sprechend), │
    │   crypto_suite, key_epoch                                 │
    ├───────────────────────────────────────────────────────────┤
    │ Key Slots                                                 │
    │   Paketschlüssel, verschlüsselt je berechtigtem Gerät /   │
    │   Empfänger / Recovery-Key                                │
    ├───────────────────────────────────────────────────────────┤
    │ Verschlüsselte Service-Beschreibung                       │
    │   Typ, Policy, Klassifikation (PUBLIC … CRITICAL),        │
    │   erlaubte Datenflüsse                                    │
    ├───────────────────────────────────────────────────────────┤
    │ Verschlüsselter Inhalt (AEAD)                             │
    ├───────────────────────────────────────────────────────────┤
    │ Signatur des Erzeugers (über Header + Key Slots + Inhalt) │
    └───────────────────────────────────────────────────────────┘

Das Layout ist ein Entwurf. Die verbindliche Spezifikation erfolgt in Modul Z-01 (`MODULE_CATALOG.md`).

### 2.1 Offene Punkte zur Klärung

Die bestehende Cloud-Implementierung verschlüsselt laut Projektangabe bereits Ende-zu-Ende. Sie liegt nicht in diesem Repository und muss gegen die Anforderungen in Abschnitt 4 nachgewiesen werden. Zu klären sind:

1. Welche Kryptobibliothek und welche Verfahren nutzt die bestehende Implementierung?
2. Wo werden Schlüssel erzeugt und gespeichert (Hardware-Keystore oder Software)?
3. Was bedeutet „als Service anbieten“ technisch: Stellt die Cloud nur das Paket bereit, oder läuft Verarbeitung in einer Umgebung, die Klartext sieht? Serverseitige Verarbeitung von Klartext wäre **nicht** mit RULE-041 vereinbar. Zulässig sind nur Verarbeitung auf dem Endgerät oder – nach gesondertem Security Review – vertrauliche Ausführung (Confidential Computing), die als eigene Vertrauensannahme dokumentiert werden muss.
4. Welche Metadaten sieht der Server (Größe, Zeitpunkte, Zugriffsmuster, Empfänger)?
5. Wie funktionieren Teilen und Widerruf zwischen Nutzern?

## 3. Schlüsselhierarchie

    Benutzer-Identität (Passkey / hardwaregebunden)
            │
            ├── Geräteschlüssel Gerät 1 (KeyMint/TEE, nicht exportierbar)
            ├── Geräteschlüssel Gerät 2
            └── Offline-Recovery-Key (Papier/Hardware-Token, beim Nutzer)
                    │
                    ▼
            Paketschlüssel (je Zero-Alpha-Paket, zufällig)
                    │
                    ▼
            Inhalt (AEAD-verschlüsselt)

- Jeder Paketschlüssel wird für jedes berechtigte Gerät und den Recovery-Key separat eingewickelt (Key Slots).
- Gerät widerrufen = Key Slot entfernen + Schlüsselrotation (`key_epoch` erhöhen) für künftige Versionen. Bereits heruntergeladene Daten können durch Widerruf nicht zurückgeholt werden – das ist eine dokumentierte Grenze.
- Crypto-Shredding (RULE-019): Vernichten aller Key Slots macht das Paket unlesbar, auch in Cloud-Backups.

## 4. Anforderungen (normativ)

| ID | Anforderung | Stufe |
|---|---|---|
| E2E-01 | Klartext und Paketschlüssel verlassen berechtigte Endgeräte nie unverschlüsselt. | MUST |
| E2E-02 | Nur etablierte Verfahren und Bibliotheken (RULE-013), z. B. HPKE (RFC 9180) für Key Wrapping, AES-256-GCM oder ChaCha20-Poly1305 als AEAD, Ed25519/ECDSA für Signaturen; für Gruppen MLS (RFC 9420). Auswahl orientiert an BSI TR-02102. | MUST |
| E2E-03 | Post-Quanten-Vorsorge: hybride Schlüsseleinigung (klassisch + ML-KEM, FIPS 203) für langlebige Daten einplanen („store now, decrypt later“). | SHOULD |
| E2E-04 | Geräteschlüssel hardwaregebunden und nicht exportierbar (RULE-005/014). | MUST für TRUSTED-Targets |
| E2E-05 | Öffentliche Schlüssel werden überprüfbar verteilt (Key Transparency oder Out-of-band-Verifikation), damit der Server keine eigenen Schlüssel unterschieben kann. | MUST |
| E2E-06 | Integrität wird auf dem Endgerät geprüft (Signatur + AEAD); Rollback auf alte Paketversionen wird erkannt (Versionszähler). | MUST |
| E2E-07 | Server-sichtbare Metadaten sind in einer Tabelle dokumentiert und minimiert (RULE-042). | MUST |
| E2E-08 | Recovery ohne Betreiberzugriff auf Klartext (RULE-030); kein Master-Key beim Betreiber (RULE-031). | MUST |
| E2E-09 | Verlust aller Geräte **und** des Recovery-Keys führt zu Datenverlust. Das wird dem Nutzer vor der Einrichtung klar kommuniziert. | MUST |
| E2E-10 | Offline-Verhalten: definierte, verschlüsselte lokale Caches mit Ablaufzeit je Klassifikation. | SHOULD |
| E2E-11 | Die bestehende Cloud-Implementierung wird vor Produktivbetrieb extern geprüft. | MUST |

## 5. Rechtliche Einordnung (Kurzfassung)

- **DSGVO Art. 25 und 32:** E2E-Verschlüsselung ist eine starke technische Maßnahme im Sinne von Privacy by Design und Sicherheit der Verarbeitung.
- **Personenbezug:** Verschlüsselte Inhalte bleiben für den Verantwortlichen, der den Schlüssel hat, personenbezogen. Für einen Cloud-Betreiber ohne Schlüsselzugriff kann die Einordnung nach der Rechtsprechung des EuGH (relativer Ansatz, u. a. C-413/23 P) anders ausfallen. **Metadaten bleiben regelmäßig personenbezogen.** Auftragsverarbeitungsverträge (Art. 28) sind daher weiterhin einzuplanen.
- **Drittlandübermittlung (Art. 44 ff.):** E2E senkt das Risiko erheblich, ersetzt aber nicht die Prüfung für Metadaten. EU-Hosting der Metadaten wird empfohlen.
- **Art. 34 Abs. 3 lit. a:** Bei einer Datenpanne kann die Benachrichtigung Betroffener entfallen, wenn die Daten für Unbefugte wirksam unzugänglich (verschlüsselt) sind – setzt E2E-01 voraus.
- **Behördliche Herausgabe:** Der Betreiber kann nur Chiffrat und Metadaten herausgeben. Das ist in der Datenschutzerklärung transparent zu beschreiben.

Details: [`COMPLIANCE.md`](COMPLIANCE.md). Ablaufpläne: [`FLOWCHARTS.md`](FLOWCHARTS.md).
