---
title: Anthropic Cowork der KI-Agent für den Desktop
date: 2026-03-27 09:31:00 +/-0000
categories: [Strategie, Arbeitsplatz, KI]
tags: [workplace,ki,processes,agentic] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/ec46cf0c-124c-45da-bac0-5c2f99689243.png
---

## Agentic AI für Desktop Workflows

[Anthropic Cowork](https://claude.com/product/cowork) ist eine agentenbasierte Erweiterung von Claude, die seit Januar 2026 als Research Preview verfügbar ist. Sie ermöglicht autonome Ausführung mehrstufiger Aufgaben auf dem **lokalen** Desktop, primär für nicht-technische Nutzer. Also statt z.B. stundenlang ihren lokalen Download Ordner aufzuräumen, geben Sie einer KI Zugriff und sie macht es selbstständig. Genau das ist die Idee für einen KI-Agenten auf dem Desktop.

Bedenken? Ja, ich auch auf den ersten Blick, aber dazu komme ich noch.

Ich habe mir Anthropic Cowork mal angeschaut und versucht, eine Bewertung der Architektur, Implementierung und Implikationen für Entscheider zu erstellen. Da auch Microsoft in der M365 E7 Lizenz mit dem Feature [Copilot Cowork](https://www.microsoft.com/en-us/microsoft-365/blog/2026/03/09/copilot-cowork-a-new-way-of-getting-work-done/) erscheinen und das im Wesentlichen auf Anthropic Cowork basieren wird, habe ich auch etwas dazu geschrieben.

## Funktionale Architektur

Kommen wir erstmal zur Architektur. Cowork basiert auf **Claude Opus 4.6** (Februar 2026), das Multi-Agent-Koordination unterstützt. Bedeutet, ein sogenannter Lead-Agent zerlegt die Aufgaben in kleine Häppchen, verteilt diese Häppchen dann an Worker-Agents und integriert die Ergebnisse dann für die Erledigung der Aufgabe. Das ganze kommt, Stand heute, als Claude Desktop App (macOS und Windows in einer Research Preview).

Im Wesentlichen geht es im ersten Schritt um den Dateizugriff und -manipulation, das Lesen, Schreiben, Umwandlung von Dateien (z. B. Screenshots zu Excel-Tabellen mit Formeln). Dabei funktioniert parallele Verarbeitung mehrerer Tasks gleichzeitig, auch mit Ausführung im Hintergrund. Erweitert wird das ganze durch Plugins z.B. für Web-Navigation sowie Konnektoren zu diversen Tools z.B. Asana/Notion.

Neu seit Februar sind OpenTelemetry-Integration für Kostentracking, Enterprise-Konnektoren (z. B. Intuit) und das Claude Agent SDK für Custom-Integrationen. Das signalisiert so langsam den Übergang von der Research Preview zu einem produktivem Einsatz

Cowork ist Teil der kostenpflichtigen Claude-Abonnements. Cowork ist in Claude Pro (ca. 20 EUR/Monat),Max( 100–200 EUR/Monat), Team und Enterprise enthalten. Für Unternehmen bietet Anthropic auch nutzungsbasierte Modelle mit OpenTelemetry-Tracking. Cowork ist natürlich nicht in der kostenlosen Claude-Version enthalten.

## Use Cases

Cowork eignet sich derzeit besonders für wiederkehrende Aufgaben mit vielen Dateien:

- Aus unstrukturierten Notizen z.B. einen strukturierten Bericht erstellen, Präsentationen aus Rohdaten generieren oder auch Zusammenfassungen schreiben.

- Belege aus Fotos in eine Tabelle übertragen, Informationen aus mehreren Dokumenten zusammenführen, Recherche-Ergebnisse aufbereiten.

- In Kombination mit der Claude in der Chrome-Erweiterung kann Cowork auch Web-Recherchen durchführen und entsprechende Formulare ausfüllen.

Kommen wir mal zu einem kleinen Beispiel. Eine Reisekostenabrechnung aus jeder Menge Beleg-Screenshots, Notizen und E-Mail-Anhängen.

- **Input**: Ein lokaler Ordner mit 50+ Dateien (Cowork benötigt die entsprechenden Berechtigungen dafür)
- **Tasks**:
  - OCR auf Bildern durchführen
  - Strukturierung der Daten in Excel (Formeln für Summen)
  - PDF-Bericht erstellen
  - Daten im Ordner neu strukturieren
- **Dauer**: 10–15 Minuten vs. ca. 2 Stunden manuell

## Anthropic Cowork vs. Microsoft Copilot Cowork

Anthropic Cowork ist ja das, worauf Microsoft Copilot Cowork aufbaut. Es gibt aber signifikante Unterschiede, deshalb möchte beides einmal gegenüberstellen.

| Kriterium          | Anthropic Cowork                                          | Microsoft Copilot Cowork                 |
| ------------------ | --------------------------------------------------------- | ---------------------------------------- |
| **Umgebung**       | Lokaler Desktop (macOS/Windows)                           | Cloud (Microsoft 365, E-Mail, Teams)     |
| **Agenten-Modell** | Multi-Agent (Lead/Worker/Merge)                           | Workflow-Agenten mit Work IQ             |
| **Datenzugriff**   | Dateisystem, Chrome                                       | Enterprise-Daten (Compliance-fokussiert) |
| **Stärken**        | Autonome Dateibearbeitung, Parallelität                   | Nahtlose App-Integration in M365         |
| **Reifegrad**      | Research Preview (Risiken: Dateilöschung)                 | Frontier-Preview (Enterprise-Sicherheit) |
| **Monitoring**     | Lokal (Logs), OpenTelemetry optional                      | Work IQ + Enterprise Audit Logs          |
| **Preis**          | Cowork ist Teil der kostenpflichtigen Claude-Abonnements. | M365-Abo (nicht spezifiziert)            |

Kurz gesagt, Anthropic Cowork eignet sich für lokale, unstrukturierte Daten, wogegen Copilot Cowork eher für cloudbasierte Prozesse in Microsoft 365 Umgebungen gedacht ist.

## Risiken und Vorteile für Entscheider

### Vorteile

- **Produktivitätsgewinne**: Automatisierung repetitiver Tasks (z. B. Datei-Organisation: Stunden vs. Minuten)
- **Skalierbarkeit**: Parallele Agenten reduzieren Latenz bei Multi-Tasking
- **Diversifikation**: Weniger Konzentration auf Standard Tasks, mehr Zeit für die individuellen Aufgaben

### Risiken

- **Sicherheit**: Prompt-Injection-Attacken sind das Hauptrisiko.
Zum Beispiel könnte eine manipulierte PDF Datei mit verstecktem Prompt Cowork anweisen, Dateien zu löschen oder zu exfiltrieren. Deshalb niemals Cowork auf Ordnern mit User-generierten Inhalten (Downloads, E-Mail-Anhänge) ohne Validierung laufen lassen.
- **Datenschutz**: Lokaler Zugriff ohne Enterprise-Controls (Backup ist hier Pflicht)
- **Zuverlässigkeit**: Preview-Status mit "noch" hoher Fehlerquote bei komplexen Tasks
- **Abhängigkeiten**: Plattform lock-in und zum Teil hohe Kosten für kleine Teams

**Empfehlung für eine Proof of Concept Installation**: Starten sie auf jeden Fall mit isolierten Ordnern (vorherige Backups obligatorisch) und messen Sie die Zeitersparnis vs. Fehlerquote der Tasks. Cowork sollte auf gar keinen Fall Zugriff auf sensible Daten haben. Es bietet sich auch an, Cowork in einer virtuellen Umgebung betreiben und entsprechende Ordner in die virtuelle Welt zu kopieren (nicht mounten). Wie bei allen KI-Tools gilt es, die Ergebnisse zu prüfen, besonders bei wichtigen Dokumenten.

## Fazit

Cowork und auch Copilot Cowork markieren den Übergang zu „[Execution AI](https://thomas-krampe.com/posts/openclaw-agentic-workplace/)" und einem [Agentic Workplace](https://thomas-krampe.com/posts/der-agentic-workplace/) - technisch überzeugend für Desktop-Agents, aber mit signifikanten Risiken in der Preview-Phase. Gegenüber Copilot Cowork gewinnt Anthropic Cowork an Flexibilität, verliert aber an Enterprise-Readiness. Beide Varianten haben großes Potenzial für Automatisierung, aber nur im Zusammenhang mit einer strengen Governance.
