---
title: Eigene Agents bauen mit Agent 365
date: 2026-04-04 08:31:05 +/-0000
categories: [Technologie, KI, Lizenzen]
tags: [ki-agent,m365,microsoft,automation,agentic] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/06554d05-8641-4720-b1f3-3767cfc7c997.png
---

## Der praktische Leitfaden mit Microsoft Copilot und Agent 365

Jetzt im April 2026 steht der deutsche Mittelstand an einem Wendepunkt. Viele Unternehmen nutzen nämlich bereits die erste Generation von Microsoft Copilot in ihren E3 und E5 Plänen für Office-Automatisierung. Microsoft startet ab 1. Mai 2026 mit der neuen [Microsoft 365 E7 Suite][1] mit integriertem Copilot Studio und Agent 365, die es ermöglicht, eigene KI-Agenten zu bauen und zu verwalten und das ohne tiefe technische Programmierkenntnisse vorauszusetzen. An dieser Stelle müssen wir aber auch im Kopf behalten, dass Microsoft die Preise für alle Microsoft 365 Abonnoments weltweit ab dem 01. Juli 2026 zwischen 5 und 25% erhöht. [siehe hier][2]

Mit diesem Artikel zeige ich Ihnen Schritt für Schritt, wie Sie als Mittelständler (100 – 500 Benutzer) Ihre ersten produktiven [KI-Agenten][3] in 3–8 Wochen erstellen und in Betrieb nehmen, was es wirklich kostet, und welche weiteren Use-Cases sofort einen positiven ROI liefern. Das Besondere daran ist, das die meisten Unternehmen bereits die entsprechenden Basis-Lizenzen im Einsatz haben. Jetzt müssen sie nur noch wissen, wie sie diese auch richtig nutzen und wo die echten Effizienzgewinne und Einsparpotentiale liegen.

### Die Lizenz-Architektur von E3/E5 zu E7 und zu Agent 365

Um Agenten zu bauen zu können, brauchen Sie zunächst Klarheit über Ihre Microsoft 365 Lizenzlandschaft. Kleinere Mittelständler bewegen sich derzeit noch im Bereich der Business Premium, die etwas größeren nutzen bereits Microsoft 365 E3 oder E5 im Einsatz. Die gute Nachricht, Sie können bereits jetzt einfache Agenten erstellen und starten, für Enterprise-Features sollten Sie allerdings zur E7 wechseln oder müssen gezielt kostenpflichtige Add-ons hinzubuchen.

### Status Quo von E3 und E5 im April 2026

Microsoft 365 E3 kostet aktuell 34,90 Euro pro Benutzer pro Monat (im Jahresabo mit Teams) und enthält grundlegende Office-Tools, Exchange Online und OneDrive. Ab Juli 2026 bekommen E3-Pläne zusätzlich Microsoft Defender for Office 365 P1 und Copilot Chat integriert, ohne dass der Preis wesentlich steigt. Für Sie bedeutet das, dass Sie bereits Copilot Chat für einfache Aufgaben nutzen können, etwa zum Zusammenfassen von E-Mails oder zur Erstellung von Texten. Allerdings sind wirkliche "Custom Agents" mit einer E3 Lizenz nur sehr begrenzt möglich. Hier brauchen Sie zusätzlich Copilot Studio Lizenzen als Add-on.

Microsoft 365 E5 (mit derzeit 55,20 Euro pro Benutzer pro Monat mit Teams) bietet mehr Sicherheit, fortgeschrittene Compliance-Tools und Copilot integriert. E5 Kunden haben damit eine bessere Basis für ihre Custom Agents, da die enthaltene Sicherheitsinfrastruktur robuster ist. Ab Juli 2026 bekommen E5-Pläne zusätzlich Intune-Module und weitere Defender-Funktionen, was für das Agent-Deployment maßgeblich ist, denn um den Überblick zu behalten, müssen Agents zentral verwaltet werden.

[Aktuelle Preise][7] finden Sie hier.

### Microsoft 365 E7 ab dem 1. Mai 2026

Die neue E7-Suite (99 USD/Monat = ca. 92 Euro, ohne MwSt.) ist ein echter Wendepunkt für den Mittelstand und enthält:

- Komplettes Microsoft 365 E5 (Office, Advanced Security, Compliance)
- Microsoft 365 Copilot (für Office-Automatisierung, sonst nur als Add-on)
- Agent 365 (zentrale Agent-Verwaltung, Governance, Monitoring) – sonst als Add-on 15 USD zusätzlich pro User und Monat
- Microsoft Entra Suite für erweiterte Identitätsverwaltung
- Erweiterte Defender-, Intune- und Purview-Funktionen

![Microsoft 365 E7 - The Frontier Suite](https://picsur.kngstn.eu/i/32004a89-3dcf-4887-b314-2a33c3499aa1.png)

Für einen Mittelstand mit 200 Benutzern bedeutet das konkret: Statt E5 (55€) + Copilot (26€) zusammen ca. 81€ pro Benutzer/Monat zu zahlen, bekommen Sie die E7 Lizenz für 92€. Also lediglich 11€ Mehrkosten pro Benutzer im Monat. Das hört ich im ersten Moment zwar nach einem ganz klassischen Up-Selling an, Sie erhalten dafür aber die zentrale Agent-Governance inklusive (Agent 365 allein kostet bereits ca. 14€ pro User pro Monat). Das ist wirtschaftlich wesentlich attraktiver für Unternehmen, die produktiv mit KI werden wollen.

## Was ist das alles

### Copilot Studio - Das Werkzeug für Custom Agents

Copilot Studio ist die Low-Code-Plattform von Microsoft, mit der Ihr IT-Team und Ihre Business-Power-User eigene Agenten bauen können. Es gibt im Wesentlichen zwei Varianten:

- Die Standalone Copilot Studio Lizenz kostet ab 26€ pro Nutzer im Monat und kommt mit 25.000 Message Credits pro Monat (falls die per-User-Lizenz nicht genug ist, zahlen Sie extra Credits zu 0,01€ pro Credit). Sie brauchen diese Lizenz, wenn Sie mit einer M365 E3 Lizenz Custom Agents bauen wollen oder wenn einzelne Power-User Agenten entwickeln sollen. Das ist sinnvoll für Unternehmen mit einer dedizierten Automation-Abteilung (z.B. 3–5 Personen).

  - **Alternativ:** Die Copilot Studio (lite) Variante ist ab Juni 2026 direkt in Microsoft Teams und SharePoint verfügbar. Sie ist ein Low-Code-Interface im Browser und richtet sich an Business-User. Das ist praktisch kostenfrei, wenn Sie bereits M365 Copilot haben, und perfekt für erste Experimente.

- Die vollständige Copilot Studio (full) mit Visual Studio Code und Azure AI Foundry bietet Pro-Code-Entwicklung für komplexere Agenten (z.B. mit Custom API-Integrationen, Machine Learning Models). Diese Variante brauchen Sie, falls Ihre Use-Cases über klassische Workflow-Automatisierung hinausgehen, das ist aber eher ein Enterprise-Szenario.

### Agent 365 - Zentrale Verwaltung und Compliance

Ab 1. Mai 2026 können Sie Agent 365 für 15 USD/Monat pro Benutzer buchen (in E7 enthalten). Agent 365 ist kein Entwicklungstool, sondern vielmehr eine komplette Verwaltungsplattform. Sie sehen, welche Agenten in Ihrer Organisation existieren, wer Zugriff hat und welche sensiblen Daten die Agenten abrufen dürfen. Darüber hinaus können Sie einzelne Agenten blockieren, wenn sie nicht compliance-konform sind. Das ist essentiell für jedes Unternehmen, da Ihre HR-, Finance- und Sales-Teams unterschiedliche Agenten brauchen, aber trotzdem eine zentrale Kontrolle für DSGVO, IT-SiG 2.0 und interne Audit zwingend erforderlich ist.

## So bauen Sie Ihren ersten Agent mit Copilot Studio

Als Mittelständler starten Sie mit Copilot Studio (lite) im Browser, nutzen bestehende M365-Daten (SharePoint, Excel, Outlook) und nach 3–5 Tagen haben Sie einen funktionierenden Prototyp. Dann können Sie einfach entscheiden, ob Sie skalieren möchten (dann mit einer E7 Lizenz oder zusätzliche Standalone Lizenzen) oder ob dieser Agent vielleicht bereits ausreichend ist.

Lassen Sie uns einmal Schritt-für-Schritt durch den Prozess der "Agent Erstellung" gehen.

### Schritt 1 - Definieren Sie Ihren Usecase

Bevor Sie eine Zeile Code oder einen Flow bauen, brauchen Sie zuerst mal etwas Klarheit. Was soll der Agent eigentlich machen? Ein typischer, gut konfigurierbarer Agent hat einen sehr fokussierten Job, etwa Kundensupport-Tickets triagieren, HR-Anfragen beantworten oder Verkaufsangebote vorbereiten, eben einen existierenden Prozess, der bereits (manuell) gelebt wird. Ein Agent, der "alles machen soll" wird komplex, fehleranfällig und lässt sich nur sehr schwer kontrollieren.

Folgende Fragen können hier helfen:

- **Was ist das Problem, das der Agent lösen soll?** Zum Beispiel: *Unser Support-Team verbringt 40 % der Zeit damit, E-Mail-Anfragen zu klassifizieren und an die richtige Person weiterzuleiten. Das dauert und Kunden warten.*

- **Wie viele Nutzer brauchen den Agent täglich?** Wenn 50 Support-Mitarbeiter von Automationen profitieren, ist der ROI klar. Wenn es nur ein Pilot mit 3 Personen ist, bleiben die Erfolge klein.

- **Welche Daten braucht der Agent?** Liegen Ihre Kundendaten in Salesforce, Ihr Knowledge Management in SharePoint, oder in Excel-Listen? Der Agent kann auf all das zugreifen, aber die Datenqualität muss auch stimmen. Wenn Ihre Kundendatenbank chaotisch ist, wird auch der Agent nur schlechte Arbeit und Vorschläge machen können ([Trash in - trash out][4]).

- **Welche Aktionen soll der Agent ausführen?** Nur Fragen beantworten (einfach), oder auch z.B. Tickets erstellen, E-Mails schreiben, Datenbanken aktualisieren etc.?

Dieser Schritt dauert ca. 1-2 Tage und Sie sollten in dieser Phase auch einen 2–3-Stunden-Workshop mit Ihren Stakeholdern (Abteilungsleiter, IT, ggf. externe Berater) einplanen. Das erspart Ihnen später teure Umbauten oder Neustarts.

### Schritt 2 - Erstellen des eigentlichen Agents in Copilot Studio

Technisch gesehen brauchen wir für diesen Schritt nur ein entsprechendes Microsoft-Konto mit Zugriff auf Copilot Studio und Zugriff auf die in M365 gespeicherten Daten. Die Bedienung ist grafisch (Drag-and-Drop) und in natürlicher Sprache, hier ist kein Code oder Programmierkentnisse nötig. Allerdings ist etwas Erfahrung, wie ein Prompt für eine KI erstellt wird, durchaus von nutzen.

- **Grundkonfiguration:** Geben Sie dem Agent einen aussagekräftigen Namen (z.B. „Support Ticket Triage Agent"), eine Beschreibung und die Anweisungen, wie er sich verhalten soll. Diese Anweisungen sind das "Gehirn" des Agenten. Ein schlechter Prompt führt zu schlechten Ergebnissen.

  - **Beispiel-Prompt:** *"Du bist ein freundlicher Support-Agent. Deine Aufgabe ist es, eingehende Kundenmails zu analysieren und zu klassifizieren. Fragen zu Rechnungen gehen an 'billing@company.de', technische Probleme an 'tech-support@company.de'. Bei Lob oder Komplimenten antworte persönlich und dankbar. Du darfst keine Rückerstattungen zusagen, nur vorschlagen. Du findest Lösungen immer zuerst in unserer Knowledge Base, bevor du um Eskalation fragst."*

- **Wissen hinzufügen:** Der Agent braucht Zugang zu Ihren Daten. Das können SharePoint-Dokumente sein (Ihre Produkthandbücher, Policies, eine Knowledgebase), Excel-Listen (Kundenreferenzen, Preislisten) oder SQL-Datenbanken. Sie verbinden diese Quellen im Studio, und der Agent kann dann darauf zugreifen.
  - **Beispiel:** Sie laden ein FAQ-Dokument hoch und der Agent lernt daraus, die entsprechenden  Antworten zu geben.

- **Aktionen konfigurieren:** Was darf der Agent tun? Sie können Power Automate-Flows verlinken, damit der Agent automatisch Outlook-Mails sendet, Tickets in ServiceNow erstellt oder CRM-Daten aktualisiert. Das ist die Brücke zwischen einem einfachen ChatBot und echter Geschäftslogik.

- **Trigger definieren:** Soll der Agent nur bei Anfrage arbeiten (Manual Trigger) oder proaktiv? Beispiel: Jede Nacht um 22 Uhr prüft der Agent, ob offene Support-Tickets älter als 24h sind und sendet Eskalations-E-Mails

Dieser Schritt kann, je nach Art und Umfang des Agenten, auch so 1-3 Tage dauern.

### Schritt 3 - Testen und Optimierung

Ohne einen Test, ist der Agent lediglich eine Idee. Das richtige testen ist somit ein kritischer Prozess in der Entwicklung von Agents. Wer schon mal bei einer Hotline angerufen hat und in der Schleife eines Sprachcomputers der ersten Generation gefangen war, weiß was ich meine. Sie müssen den Agenten hier mit echten Szenarien testen am besten auch mit unbelasteten Testbenutzern. Typische Fehler können hier sein, dass der Agent nicht ausführlich genug oder aber auch viel zu ausführlich antwortet, die Kundenabsichten nicht richtig versteht oder einfach zu schnell eskaliert.

Copilot Studio bietet hierfür einen Bereich zum testen. Sie geben lediglich eine Test-Nachricht ein (z.B. „Mein Internet ist seit 2 Tagen weg") und sehen sofort, wie der Agent antwortet. Nun können Sie optimieren, bis die Antwortqualität des Agenten gut genug ist. Der ganze Vorgang dauert wieder ungefähr 3-5 Tage.

> **Wichtig für Deutschland:** Achten Sie auf DSGVO-Compliance. Der Agent darf nicht versehentlich Kundendaten in ungeschützten Logs speichern. Microsoft sichert die Agent-Daten standardmäßig in Azure-Rechenzentren in Deutschland/Europa, aber Sie sollten das mit Ihrer IT Security und Compliance klären.
{: .prompt-tip }

### Schritt 4 - Proof of Concept mit echten Benutzern

Lassen Sie 20–30 echte Benutzer (z.B. das Support-Team) auf den Agenten los und sammeln das Feedback. Das ist keine Entwicklung mehr, sondern Change Management. Geben Sie auch klare Anweisungen, wann der Agent zu nutzen ist und wann nicht. Sammeln Sie das Feedback via Umfrage oder wöchentliche Check-ins und lassen Sie die Benutzer selbst oder auch jemand anderen die konkreten Metriken messen, wie z.B. Zeitersparnis pro Ticket, Kundenzufriedenheit, Eskalationsquote. Der Agent wird von ganz allein besser, je mehr echten Kontext er sieht.

Nach etwa 2 Wochen Pilotphase sollte klar sein, ob der Agent funktioniert oder nicht? Wenn ja, rollen Sie das in der Breite aus. Wenn nein, begeben Sie sich zu Schritt 3 und verbessern den Agent.

### Schritt 5 - Rollout und Monitoring

Erfüllt der neue Agent alle Kriterien und Qualitätsansprüche veröffentlichen Sie den Agent unternehmensweit. Mit Agent 365 (ab E7 oder 15€ zusätzlich als Add-on) können Sie Ihren Agenten zentral verwalten und überprüfen, wer Zugriff hat und welche Agenten in welcher Abteilung bereitgestellt wurden. Agent 365 läuft fortlaufend und überwacht:

- Wie viele Anfragen pro Tag werden vom Agent bearbeitet?
- Wie viele davon werden korrekt gelöst vs. eskaliert?
- Welche Rückmeldungen geben Nutzer?

Auf Basis dieser Daten können Sie ihren Prompt fortlaufend optimieren und verbessern.

## Fazit

Agenten sind im Mittelstand nicht mehr optional. Ihre Konkurrenz baut und nutzt diese bereits. Der Mittelstand, der bis Ende 2026 nicht mindestens einen produktiven Agenten im Einsatz hat, verliert immer mehr Wettbewerbsfähigkeit. Starten Sie jetzt, die technologische und wirtschaftliche Grundlage ist da. Sie brauchen keine studierten Mathematiker und kein großes Budget. Sie brauchen einen klaren Use-Case, 3–4 Wochen Zeit, und ein kleines Budget für den ersten Agenten.

Wer jetzt an dieser Stelle denkt, das ist ja alles ganz nett, aber wie fange ich an?
Nun, ich habe für Sie einige fertige und sofort umsetzbare Use-Cases mit einer Kostenbetrachtung in einem Whitepaper zusammengefasst. Sprechen Sie mich [einfach an][5] oder vereinbaren gleich einen [Termin mit mir][6].

[1]: https://partner.microsoft.com/en-us/blog/article/agent-365-announcement
[2]: https://www.heise.de/news/Microsoft-erhoeht-Preise-fuer-Microsoft-365-ab-Juli-2026-deutlich-11103864.html
[3]: https://thomas-krampe.com/posts/der-agentic-workplace/
[4]: https://thomas-krampe.com/posts/trash_in_trash_out/
[5]: https://www.linkedin.com/in/thomaskrampe/
[6]: https://t13k.de/bookp
[7]: https://www.microsoft.com/de-de/microsoft-365/enterprise/microsoft-365-plans-and-pricing?market=de
