# Compliance Overview – Deutsches und europäisches Recht

Status: DRAFT – Stand 2026-09
Bezug: Codex RULE-044, Roadmap Release Gate 11

> **Hinweis:** Dieses Dokument ist eine technische Arbeitsgrundlage und **keine Rechtsberatung**. Vor Markteinführung, kommerziellem Angebot oder Einsatz bei Kunden ist eine anwaltliche Prüfung erforderlich. Fristen und Rechtsstand sind vor jeder Freigabe erneut zu prüfen.

## 1. Übersicht

| # | Regelwerk | Betrifft | Status | Maßnahme / Dokument |
|---|---|---|---|---|
| L-01 | Urheberrecht / Open-Source-Lizenzen | gesamtes Projekt | ✅ Apache-2.0 festgelegt | `LICENSE`, `NOTICE`, Abschnitt 2 |
| L-02 | Cyber Resilience Act (EU) 2024/2847 | IPOS als Produkt mit digitalen Elementen | 🟡 teilweise | Abschnitt 3, `SECURITY.md` |
| L-03 | DSGVO / BDSG | Cloud, Attestation, Logging, DLP | 🟡 teilweise | Abschnitt 4, `CLOUD_DATA_PROTECTION.md` |
| L-04 | TDDDG § 25 | Zugriff auf Informationen im Endgerät | 🔴 offen | Abschnitt 4.4 |
| L-05 | BetrVG § 87 Abs. 1 Nr. 6 | Einsatz auf Firmengeräten | 🔴 offen | Abschnitt 5 |
| L-06 | NIS2 / deutsches Umsetzungsgesetz (BSIG) | Kunden als Betreiber | ⚪ Chance | Abschnitt 6 |
| L-07 | Produkthaftungsrichtlinie (EU) 2024/2853 | Software als Produkt | 🔴 offen | Abschnitt 7 |
| L-08 | Funkanlagenrichtlinie 2014/53/EU + Delegierte VO (EU) 2022/30 | eigene Omega-Hardware | ⚪ ab P8 | Abschnitt 8 |
| L-09 | Dual-Use-VO (EU) 2021/821, Kat. 5 Teil 2 | Kryptografie, Export | 🔴 offen | Abschnitt 9 |
| L-10 | UWG (Irreführung) | Werbeaussagen, „CERTIFIED“ | 🟡 teilweise | Abschnitt 10 |

Legende: ✅ erledigt · 🟡 teilweise · 🔴 offen · ⚪ später relevant

## 2. Lizenzen (L-01)

- **Projektlizenz:** Apache License 2.0 (`LICENSE`). Sie enthält eine ausdrückliche Patentlizenz und einen Haftungsausschluss.
- **Beiträge:** Nach Apache-2.0 Abschnitt 5 gelten Beiträge unter derselben Lizenz, sofern nichts anderes vereinbart ist. Empfehlung: Developer Certificate of Origin (DCO, `Signed-off-by`) für Beiträge verlangen.
- **Drittkomponenten – Pflichten:**

| Komponente | Lizenz | Pflicht |
|---|---|---|
| Linux-Kernel | GPL-2.0-only | Quellcode der ausgelieferten Kernel-Version inkl. Änderungen bereitstellen; Kernel-Änderungen stehen unter GPL-2.0, nicht Apache-2.0 |
| AOSP | überwiegend Apache-2.0 | NOTICE-Dateien übernehmen |
| Chromium | BSD-3-Clause + zahlreiche Drittlizenzen | Lizenzhinweise ausliefern |
| Firmware/Vendor-Blobs | proprietär | Weitergaberecht prüfen; im Component Trust Registry erfassen |

- Die SBOM (RULE-026) muss Lizenzangaben im SPDX-Format enthalten, damit Lizenzprüfung automatisierbar ist.
- Hinweis: Kernel-Module und GPL-Code werden durch die Apache-Lizenz des Projekts nicht umlizenziert.

## 3. Cyber Resilience Act (L-02)

- **Einordnung:** Betriebssysteme sind in Anhang III als „wichtige Produkte“ der Klasse I gelistet. Nicht kommerziell bereitgestellte Open-Source-Software fällt nicht unter den CRA; wird IPOS oder Zero Alpha im Rahmen einer Geschäftstätigkeit angeboten (auch als Service zu einem Gerät), gelten die Herstellerpflichten. Für Organisationen, die Open Source nachhaltig unterstützen, ohne Hersteller zu sein, gilt die erleichterte Rolle des „Open-Source-Software-Stewards“ (Art. 24).
- **Fristen:**
  - seit **11.09.2026**: Meldepflicht für aktiv ausgenutzte Schwachstellen und schwerwiegende Sicherheitsvorfälle (Art. 14) über die Single Reporting Platform der ENISA an das zuständige CSIRT (in Deutschland: BSI)
    - Frühwarnung innerhalb von **24 Stunden**
    - Meldung innerhalb von **72 Stunden**
    - Abschlussbericht: bei Schwachstellen spätestens **14 Tage** nach Verfügbarkeit einer Korrektur, bei schwerwiegenden Vorfällen innerhalb **eines Monats**
  - ab **11.12.2027**: alle übrigen Pflichten (Anhang I Sicherheitsanforderungen, Konformitätsbewertung, CE-Kennzeichnung, technische Dokumentation, Supportzeitraum)
- **Abdeckung durch das Konzept:**

| CRA-Anforderung | IPOS-Bezug | Stand |
|---|---|---|
| Security by Design/Default | Codex RULE-001–044 | ✅ konzipiert |
| SBOM | RULE-026, B-04 | 🟡 geplant |
| Schwachstellenbehandlung / CVD | `SECURITY.md` | 🟡 Prozess beschrieben, Kanal fehlt |
| Sicherheitsupdates, getrennt von Funktionsupdates | Update-System §12 | 🟡 geplant |
| Supportzeitraum (i. d. R. mind. 5 Jahre) | – | 🔴 festlegen |
| Konformitätsbewertung (Klasse I: harmonisierte Normen oder Dritte) | – | 🔴 ab 2027 |
| Meldeprozess 24 h / 72 h | `SECURITY.md`, `FLOWCHARTS.md` §7 | 🟡 beschrieben |

## 4. Datenschutz (L-03, L-04)

### 4.1 Rollen
- Selbst gehostete Installationen: Das betreibende Unternehmen ist Verantwortlicher (Art. 4 Nr. 7).
- Von Omega betriebene Cloud: Omega ist in der Regel Auftragsverarbeiter (Art. 28) → Auftragsverarbeitungsvertrag, TOM-Beschreibung, Unterauftragsverarbeiter-Liste.
- Rollen je Betriebsmodell vor Produktivbetrieb verbindlich festlegen.

### 4.2 Erforderliche Dokumente
| Dokument | Rechtsgrundlage | Stand |
|---|---|---|
| Verzeichnis von Verarbeitungstätigkeiten | Art. 30 | 🔴 |
| Datenschutz-Folgenabschätzung für Attestation, Security Logging, DLP, Fernlöschung | Art. 35 | 🔴 |
| TOM-Beschreibung | Art. 32 | 🟡 (Threat/Trust Model als Basis) |
| Auftragsverarbeitungsvertrag | Art. 28 | 🔴 |
| Datenschutzhinweise | Art. 13/14 | 🔴 |
| Löschkonzept (inkl. Security-Logs, Crypto-Shredding) | Art. 5 Abs. 1 lit. e, Art. 17 | 🔴 |
| Meldeprozess Datenpannen (72 h an Aufsichtsbehörde) | Art. 33/34 | 🟡 `FLOWCHARTS.md` §7 |

### 4.3 Technische Umsetzung
- Art. 25 (Privacy by Design/Default): RULE-038, RULE-039, RULE-042, `TRUST_MODEL.md` §6.
- Art. 32: E2E-Verschlüsselung (RULE-041), Hardware-Keys, Zugriffskontrolle.
- Drittland: Metadaten und Logs bevorzugt in der EU. US-Anbieter (z. B. Cloudflare) nur nach Prüfung von EU-US Data Privacy Framework bzw. Standardvertragsklauseln und Transfer Impact Assessment.
- Security-Logs: Rechtsgrundlage in der Regel Art. 6 Abs. 1 lit. c/f; Speicherdauer begrenzen; Pseudonymisierung.

### 4.4 TDDDG § 25
Das Auslesen von Geräteinformationen (Attestation, Patchlevel, Kennungen) ist ein Zugriff auf Informationen in der Endeinrichtung. Ohne Einwilligung zulässig nur, wenn er für den vom Nutzer ausdrücklich gewünschten Dienst unbedingt erforderlich ist (§ 25 Abs. 2 Nr. 2). Attestation für Zero-Trust-Zugriff ist voraussichtlich erforderlich – die Begründung ist je Datenfeld zu dokumentieren; nicht erforderliche Felder entfallen (Datenminimierung).

## 5. Einsatz im Unternehmen (L-05)

Security Logging, DLP, Standort-/Netzwerk-Policies und Fernlöschung sind technische Einrichtungen, die geeignet sind, Verhalten oder Leistung von Beschäftigten zu überwachen. Folgen:
- **Mitbestimmung des Betriebsrats** nach § 87 Abs. 1 Nr. 6 BetrVG → Betriebsvereinbarung vor Einführung.
- Beschäftigtendatenschutz: Verarbeitung nur, soweit erforderlich (Art. 6, Art. 88 DSGVO); bei § 26 BDSG die Rechtsprechung des EuGH (C-34/21) beachten.
- Private Nutzung von Firmengeräten gesondert regeln; Fernlöschung darf private Daten nur nach klarer Vereinbarung betreffen.
- Das Projekt sollte eine **Muster-Betriebsvereinbarung** und eine Konfiguration „Datensparsam“ als Vorlage bereitstellen.

## 6. NIS2 (L-06)

Das Projekt ist selbst voraussichtlich keine besonders wichtige/wichtige Einrichtung. Kunden, die unter NIS2 fallen, können IPOS/Omega-Zero als Baustein für Risikomanagementmaßnahmen nutzen (Zugriffskontrolle, Kryptografie, Zero Trust, Lieferkettensicherheit). Empfehlung: Mapping auf die Maßnahmen nach NIS2 Art. 21 bzw. BSIG erstellen.

## 7. Produkthaftung (L-07)

Nach der neuen Produkthaftungsrichtlinie (EU) 2024/2853, anwendbar auf ab dem 09.12.2026 in Verkehr gebrachte Produkte, gilt Software als Produkt; fehlende Sicherheitsupdates können einen Fehler begründen. Außerhalb einer Geschäftstätigkeit entwickelte oder bereitgestellte freie Open-Source-Software ist ausgenommen. Folgen für kommerzielle Angebote:
- Supportzeitraum und Update-Zusagen klar festlegen.
- Nachweis-Dokumentation (Tests, Reviews, Audit Trail) aufbewahren – das Konzept (RULE-033) unterstützt das.
- Der Haftungsausschluss der Apache-Lizenz wirkt nicht gegen zwingende gesetzliche Produkthaftung.

## 8. Funkanlagen (L-08)

Für eigene Omega-Hardware gelten die grundlegenden Anforderungen nach Art. 3 Abs. 3 lit. d, e, f der Funkanlagenrichtlinie (Netzschutz, Datenschutz, Betrugsschutz), anwendbar seit 01.08.2025; harmonisierte Normen EN 18031-1/-2/-3. Für Referenzgeräte (Pixel 8a) liegt die Konformität beim Hersteller, solange die Hardware nicht verändert wird.

## 9. Exportkontrolle (L-09)

Kryptografische Funktionen fallen grundsätzlich unter Kategorie 5 Teil 2 der Dual-Use-VO. Für öffentlich verfügbare Software („in the public domain“) und Massenmarktprodukte gelten Ausnahmen bzw. Erleichterungen. Vor Auslieferung von Binärbuilds oder Hardware an Kunden außerhalb der EU: Prüfung mit dem BAFA bzw. Fachberatung.

## 10. Werbeaussagen (L-10)

- Keine absoluten Sicherheitsversprechen (bereits Grundsatz, siehe Architektur-Präambel und Threat Model §8).
- Die Status „TRUSTED“ und „CERTIFIED“ sind **projektinterne Bewertungen**. In externer Kommunikation muss klar sein, dass damit keine behördliche Zertifizierung (BSI, Common Criteria, CRA-Konformitätsbewertung) gemeint ist, solange keine vorliegt.
- „Ende-zu-Ende-verschlüsselt“ nur verwenden, wenn E2E-01 bis E2E-08 nachweislich erfüllt sind.

## 11. Referenzrahmen

- BSI TR-02102 (kryptografische Verfahren)
- BSI IT-Grundschutz / ISO/IEC 27001 (Betrieb)
- BSI C5 (Cloud-Dienste)
- ISO/IEC 29147 und 30111 (Offenlegung und Behandlung von Schwachstellen)
- SLSA und OpenSSF Scorecard (Lieferkette)
- Common Criteria / BSZ als mögliches späteres Zertifizierungsziel
