---
title: Identitäten für KI-Agenten
date: 2026-06-16 11:43:00 +/-0000
categories: [Technologie, KI, Azure]
tags: [azure,entra,entraid,agentic,ki-agent,zero-trust] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/45a9dd5f-bee7-40da-9ae3-622abef1903d.png
---

# Warum Entra ID neu gedacht werden muss

Service Accounts für Automatisierungen, Dienste oder Anwendungen waren jahrelang die Standardlösung für alles, was kein Mensch ist. Jetzt kommen KI-Agenten hinzu und sie passen in dieses Konstrukt nicht mehr wirklich.

Um das Thema etwas greifbarer zu machen, fange ich mal mit einem echten Beispiel an. Ein Kunde, ein mittelständisches Unternehmen mit 300 Mitarbeitern, setzt bereits seit einiger Zeit Copilot Studio ein. Ein selbstgebauter Agent nimmt dabei Urlaubsanträge über Microsoft Teams entgegen, prüft den Resturlaub im HR-System, informiert den Vorgesetzten und trägt den genehmigten Urlaub dann in den Kalender ein. Dieser Agent läuft unter einem generischen Service Account, weil das die schnellste Lösung war.

Jetzt, drei Monate später, stellt der Datenschutzbeauftragte die Frage, welche Mitarbeiterdaten dieser Agent wann abgefragt hat und wo diese Zugriffe dokumentiert wurden.

Die Antwort können wir uns alle denken. Aber genau das ist das Problem, das ich mit diesem Artikel ansprechen möchte.

## Warum klassische Identitäten nicht reichen

Service Accounts sind statisch. Sie haben Credentials, eine feste Rollenzuweisung und tun immer dasselbe. KI-Agenten dagegen sind das genaue Gegenteil. Sie interpretieren Kontext, treffen eigenständige Entscheidungen, kombinieren Aktionen und agieren über mehrere Systeme hinweg. Alles in einem Ablauf, ohne menschliche Bestätigung für jeden Schritt.

Das schafft jetzt ein Governance-Problem, denn wir wissen nicht, wer dem Agenten Zugriff auf etwas gegeben hat. Wer überwacht, was er tut? Und wenn er etwas falsch macht, wer ist dafür verantwortlich?

Der klassische Service Account gibt darauf keine gute Antwort. Er hat keine Lifecycle-Verwaltung für das Verhalten eines Agenten, kein kontextbezogenes Risiko-Scoring und keine Integration in Conditional-Access-Policies, die für Benutzer längst Standard sind. Ein Agent unter einem geteilten Service Account ist aus Governance-Sicht unsichtbar, man sieht lediglich seine Aktionen, aber keinen wirklichen Akteur.

**Und Managed Identities?** Für Azure-Dienste, die innerhalb der Microsoft-Infrastruktur kommunizieren, funktionieren Managed Identities gut und sollten auch weiterhin genutzt werden. Für KI-Agenten, die eigenständig über Systemgrenzen hinweg agieren, Benutzerkontext interpretieren und protokollierungspflichtige Aktionen ausführen, reichen sie strukturell nicht aus. Ihnen fehlt ein eigener Lebenszyklus, granulare Governance und die Fähigkeit, risikobasiert bewertet zu werden.

## Was Entra Agent ID ist (und was noch Preview ist)

Microsoft hat im März 2026 mit **Entra Agent ID** eine eigene Identitätskategorie für KI-Agenten eingeführt. Das Kernprinzip lautet "Human-led, Agent-operated". Der Mensch gibt die Absicht vor, der Agent führt es aus. Und das unter einer nachvollziehbaren, verwaltbaren Identität mit eigenem Lebenszyklus.

![microsoft-entra-agent-identity-capabilities-expanded](https://picsur.kngstn.eu/i/d854c0d7-c215-4e0f-a145-ae35627619b4.png)

Was Entra Agent ID konkret heute bereits kann:

- **Eigene Identität pro Agent**: Jeder Agent erhält ein individuelles Identitätskonto in Entra ID, keine geteilten Accounts mehr.
- **Lifecycle-Verwaltung**: Erstellung, Berechtigungszuweisung, Rezertifizierung und Deaktivierung über Entra ID Governance.
- **Conditional Access für Agenten**: Richtlinien steuern direkt Agenten-Zugriffe, IP-Beschränkungen, Compliance-Prüfungen sowie Risikobewertungen.
- **Identitätsschutz**: Anomales Verhalten eines Agenten wird erkannt und kann automatisch geblockt werden.
- **Plattformoffenheit**: Unterstützung für Nicht-Microsoft-Umgebungen über Workload Identity Federation, sodass AWS Bedrock, n8n und andere externe Frameworks eingebunden werden können.

**Was noch nicht GA ist (aber kommt):** Die automatische Zuweisung von Agentidentitäten in Copilot Studio sowie Teile der Entra ID Governance für Agenten befinden sich Stand Juni 2026 noch in der Public Preview. Das bedeutet: Produktiveinsatz ist möglich, aber mit dem Vorbehalt, dass sich Funktionsumfang und Oberflächen noch ändern können. Wer heute damit startet, sollte das in seiner internen Dokumentation festhalten.

## Was kostet das?

Das ist die Frage, die in keiner IT-Leitungsrunde ausbleibt und die ich in diesem Artikel natürlich nicht unbeantwortet lassen kann.

| Lizenz              | Preis (ca.)        | Enthält                                  |
| ------------------- | ------------------ | ---------------------------------------- |
| Microsoft 365 E3    | ~36 USD/User/Monat | Grundpaket, kein Copilot                 |
| Microsoft 365 E5    | ~57 USD/User/Monat | E3 + Security, Compliance, Analytics     |
| M365 Copilot Add-on | ~30 USD/User/Monat | Copilot für E3/E5-Kunden                 |
| Agent 365           | 15 USD/User/Monat  | Governance-Ebene für Agenten, standalone |
| Microsoft 365 E7    | 99 USD/User/Monat  | E5 + Copilot + Entra Suite + Agent 365   |

Alle Preise sind Listenpreise in USD und können auf der Microsoft Website abgerufen werden, CSP-Konditionen können natürlich abweichen. Für den deutschen Markt gelten EUR-Preise, die je nach Wechselkurs und Partnervertrag variieren. Aber dieser Artikel soll ja auch keine Lizenzberatung sein.

Entscheidend dabei ist, dass nicht jeder Mitarbeiter dieselbe Lizenz benötigt. Das realistische Bild für ein Unternehmen mit 300 Mitarbeitern sieht eher so aus:

| Gruppe                                 | Lizenz                 | Kosten/Jahr (ca.) |
| -------------------------------------- | ---------------------- | ----------------- |
| 300 Mitarbeiter                        | E3 als Basis           | ~130.000 USD      |
| 40 Power User und Führungskräfte       | + Copilot Add-on       | ~14.400 USD       |
| 8 IT-Admins und Prozessverantwortliche | + Agent 365 standalone | ~1.440 USD        |
| **Gesamt**                             |                        | **~146.000 USD**  |

Der Mehraufwand für Agenten-Governance beläuft sich damit auf rund 16.000 USD pro Jahr, wenn wir alle Produkte einzeln kaufen (gem. Tabelle). E7 lohnt sich vor allem dann, wenn ohnehin ein Upgrade auf E5 ansteht und Copilot breit ausgerollt werden soll. Wer gezielt nur die Governance-Ebene nachrüsten will, kommt mit Agent 365 standalone für die relevanten Personen deutlich günstiger weg.

Entra Agent ID selbst ist in Entra ID P2 enthalten, das viele Unternehmen über E5 oder Add-ons bereits lizenziert haben. Wer noch auf E3 ist, sollte das vorher in einem Lizenzgespräch oder einem Audit prüfen.

## Zero Trust für KI-Identitäten

Zero Trust bedeutet, niemals vertrauen, immer prüfen. Das galt bisher für Benutzer und Geräte. Mit KI-Agenten kommt eine dritte Kategorie hinzu. Wer sich tiefer in das Thema Zero-Trust einlesen möchte, dem empfehle ich meine folgenden Artikel.

- [Zero Trust ist tot, lang lebe verified Trust](https://thomas-krampe.com/posts/zero-trust-ist-tot/)
- [Zero-Trust verstehen und einsetzen](https://thomas-krampe.com/posts/zero-trust-verstehen-und-einsetzen/)

**Conditional Access für Agenten** funktioniert analog zu Benutzerrichtlinien. Ein Agent, der intern auf SharePoint zugreift, braucht andere Richtlinien als einer, der externe APIs aufruft oder Daten aus Kundensystemen verarbeitet. Richtlinien lassen sich granular je Agent und Ressource definieren.

**Risikobasierte Auswertung**: Entra bewertet den Kontext eines Agenten-Zugriffs adaptiv. Weicht das Verhalten vom Erwartungsprofil ab, z.B. ungewöhnliche Zugriffszeiten, bisher nicht genutzte Systeme oder plötzlich breitere Datenzugriffe, greift die Risikobewertung, analog zum Identitätsschutz für Benutzer.

**Minimale Berechtigung**: Agenten sollten nur die Rechte haben, die sie für ihre konkrete Aufgabe benötigen. Das ist in der Praxis die häufigste Lücke, weil Agenten oft mit zu breiten Berechtigungen gestartet werden, um schnell einsatzbereit zu sein. Mit Entra Agent ID lässt sich das strukturiert durchsetzen, statt es der Disziplin einzelner Teams zu überlassen.

## Die Governance-Oberfläche für IT-Admins

Agent 365 ist seit 1. Mai 2026 die zentrale Steuerkonsole im Microsoft Admin Center für alle KI-Agenten der Organisation, unabhängig davon, womit sie gebaut wurden. IT-Teams sehen auf einen Blick, welche Agenten aktiv sind, welche Berechtigungen sie haben, welche Aktionen sie ausgeführt haben und wo Anomalien aufgetreten sind. Die Integration in Microsoft Defender, Purview und Entra macht Agent 365 zur übergreifenden Governance-Schicht.

Für IT-Admins ist das die entscheidende Neuerung. Kein Tool-Hopping mehr zwischen Copilot Studio, Power Platform Admin Center und Entra, um einen Überblick über aktive Agenten zu bekommen. Für Unternehmen, die mit Copilot Studio Agenten erstellen und verwenden, ist das keine Option, sondern ein absolutes Must-have.

## DSGVO: Die Fragen, die der Datenschutzbeauftragte stellen wird

Für Unternehmen in Deutschland ist dieser Punkt nicht optional. Wer KI-Agenten einsetzt, die Personaldaten, Kundendaten oder andere personenbezogene Informationen verarbeiten, muss mehrere Fragen beantworten, bevor der Agent in Produktion geht:

**Auftragsverarbeitung**: Wenn ein Agent Mitarbeiterdaten abruft, z.B. Urlaubskonten, Gehaltsabrechnungen, Krankmeldungen etc., ist Microsoft als Auftragsverarbeiter eingebunden. Der bestehende Auftragsverarbeitungsvertrag mit Microsoft muss auf den Agenten-Kontext geprüft werden.

**Protokollierung und Nachvollziehbarkeit**: Agent 365 und Entra Agent ID erzeugen Audit-Logs. Wo werden diese gespeichert? Microsoft betreibt für deutsche Unternehmen primär Rechenzentren in der EU, aber der genaue Speicherort der Agent-Logs sollte im Rahmen der Datenschutz-Folgenabschätzung dokumentiert werden.

**Betriebsrat**: In Unternehmen mit Betriebsrat ist der Einsatz von KI-Systemen, die das Verhalten oder die Leistung von Mitarbeitern protokollieren könnten, mitbestimmungspflichtig nach § 87 Abs. 1 Nr. 6 BetrVG. Das gilt auch für Agenten, die Mitarbeiterprozesse automatisieren. Wer bereits Artikel von mir gelesen hat, wird diesen Tip in anderen Zusammenhängen bereits gelesen haben. Die frühzeitige Einbindung der Arbeitnehmervertretung spart Zeit und verhindert spätere Konflikte.

**Datensparsamkeit**: Welche Daten greift der Agent tatsächlich ab, und welche davon sind für seine Aufgabe wirklich notwendig? Das Prinzip der Datensparsamkeit gilt auch für automatisierte Systeme.

Diese Punkte sind keine Blockade für den Einsatz von KI-Agenten, aber sie sind Hausaufgaben, die vor dem Go-live erledigt sein müssen. Keine Ausnahme, kein "das machen wir später", das kann im Rahmen von Verstößen gegen die DSGVO teuer werden.

## Was ist mit bestehenden Automatisierungen?

Nicht jede bestehende Automatisierung muss auf Entra Agent ID umgestellt werden. Eine pragmatische Einschätzung:

**PowerShell-Skripte und Scheduled Tasks**: Diese laufen unter Service Accounts oder Managed Identities und sollten dort bleiben, solange sie klar abgegrenzte, nicht-autonome Aufgaben erledigen. Migration nur, wenn diese Skripte zukünftig KI-Komponenten erhalten.

**Power Automate Flows ohne KI**: Bestehende Flows ohne AI Builder oder Copilot-Integration sind nicht betroffen. Wer allerdings AI Builder-Schritte hinzufügt oder Copilot-Aktionen einbindet, sollte die Identitätsfrage neu bewerten.

**RPA-Tools (UiPath, Power Automate Desktop)**: RPA steht für Robotic Process Automation. Das sind Software-Roboter, die regelbasierte, sich wiederholende Aufgaben automatisieren, indem sie Benutzeroberflächen steuern, Klicks simulieren und Daten zwischen Anwendungen übertragen, ganz ohne direkte API-Integration. Klassische RPA-Bots fallen nicht unter Entra Agent ID, diese haben in der Regel eigene Identitätskonzepte. Sobald jedoch ein RPA-Bot KI-gesteuerte Entscheidungen trifft und dabei z.B. auf Personaldaten zugreift, wird die Governance-Frage wieder relevant.

**Copilot Studio Agenten und Azure AI Foundry**: Hier ist Entra Agent ID der natürliche Ausgangspunkt. Wer neu baut, sollte von Anfang an damit starten.

Meine persönliche Empfehlung: Kein Big-Bang-Ansatz. Bestandssysteme inventarisieren, kategorisieren, und nur dort umstellen, wo ein tatsächlicher Governance-Mehrwert entsteht. An dieser Stelle empfehle ich auch meinen Artikel [KI Agenten für den Mittelstand erstellen](https://thomas-krampe.com/posts/ki-agenten-fuer-den-mittelstand-erstellen/).

## Welche Fragen heute beantwortet werden müssen

Wer Copilot Studio nutzt, Azure AI Foundry testet oder Agenten in Geschäftsprozesse einzubinden plant, sollte die folgenden Fragen klären, bevor der erste Agent produktiv geht:

1. **Inventar**: Welche Agenten laufen bereits, auch informell oder als Power Automate-Flows mit KI-Schritten?
2. **Identität**: Haben diese Agenten eigene Identitäten, oder teilen sie Credentials mit Benutzern oder generischen Service Accounts?
3. **Berechtigungen**: Welche Systeme und Daten darf welcher Agent erreichen und warum genau diese?
4. **Verantwortung**: Wer ist Owner für das Verhalten eines Agenten? IT-Abteilung, Prozessverantwortlicher, beides?
5. **Logging**: Werden Agenten-Aktionen protokolliert, auswertbar gespeichert und sind im Ernstfall nachvollziehbar?

Diese Fragen klingen nach Compliance-Overhead. Sind sie auch, aber zugleich sind sie auch die Grundlage dafür, dass KI-Agenten intern und extern Vertrauen aufbauen können. Und sie sind deutlich einfacher zu beantworten, solange der erste Agent noch im Piloten steckt, als nachträglich in einer produktiven Umgebung.

## Pragmatischer Einstiegspfad

Wer noch keine Agenten im Produktiveinsatz hat, kann strukturiert starten:

1. **Entra Agent ID aktivieren** und erste Testidentitäten für Pilotprojekte anlegen.
2. **Agent 365 im Admin Center** als Beobachtungsschicht einrichten, bevor Agenten aktiv werden.
3. **Conditional Access Policies** für den Agenten-Kontext definieren — analog zu bestehenden Benutzerrichtlinien, aber mit agenten-spezifischen Parametern.
4. **Einen Piloten mit engem Scope** starten: ein Copilot Studio Agent mit klar definierten Aktionen, eigener Entra-Identität, minimalen Berechtigungen und aktivem Logging.
5. **Ein Governance-Dokument anlegen**: Wer hat den Agenten freigegeben? Was darf er? Wer überwacht ihn?

## Was wirklich auf Sie zukommt

Der Einstieg ist kein Wochenendprojekt und wird auch nicht von einem Mitarbeiter "mal schnell mit gemacht". Agent 365 und Entra Agent ID einzurichten ist in ein bis zwei Tagen machbar. Sinnvolle Conditional Access Policies für einen ersten Agenten zu definieren, testen und dokumentieren braucht realistisch eine Woche. Wenn jemand da ist, der sich damit auskennt. In einer IT-Abteilung mit vier Leuten, die ohnehin im Tagesgeschäft stecken, muss dafür bewusst Kapazität freigeräumt oder auch eingekauft werden. Wer das nicht tut, macht den gleichen Fehler wie der Kunde aus meinem Einstiegsszenario. Schnell eingeführt, schwer zu erklären und weit weg von Compliance.

KI-Agenten sind keine Benutzer und keine klassischen Dienste. Die Werkzeuge, die für beides jahrelang gut funktioniert haben, reichen für autonome KI-Systeme strukturell nicht aus. Microsoft hat mit Entra Agent ID und Agent 365 die Infrastruktur bereitgestellt, aber die Entscheidung, diese zu nutzen, Governance frühzeitig zu etablieren und die DSGVO-Hausaufgaben zu machen, liegt bei Ihnen und Ihren IT-Teams.

Wer jetzt die richtigen Fragen stellt, vermeidet später die teuren Antworten.
