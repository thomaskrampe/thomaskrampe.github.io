---
title: Jenseits der Automatisierung: Die strategische Rolle von KI-Workflows und autonomen Agenten für CIOs
date: 2025-10-11 09:00:00 +/-0000
categories: [Strategie, KI, Automatisierung]
tags: [processes, llm, ki-agents] # TAG names should always be lowercase
image:
  path: https://picsur.myctx.net/i/8a8d20b2-58b4-4ca6-97a0-8bdd28cc6e68.png
---

In meinen vorangegangenen Artikeln habe ich immer wieder die KI als eine mögliche Lösung für den Fachkräftemangel hingewiesen. Als Feedback auf diese Artikel habe ich mitgenommen, dass die Unternehmensautomatisierung eine tiefgreifende Transformation durchläuft. In vielen Organisationen besteht Grundsätzlich eine große Unklarheit über die feinen, aber entscheidenden Unterschiede zwischen einer Automatisierung, einem KI-Workflow und einem KI-Agenten. Für CIOs ist das Verständnis dieser unterschiedlichen Konzepte nicht nur wichtig, sondern bildet die kritische Grundlage für strategische Investitionen und eine zukunftssichere IT-Architektur.

Im Kern lassen sich diese drei Stufen der Prozessoptimierung anhand von drei Schlüsselkriterien unterscheiden:

* der Nutzung von **Large Language Models (LLMs)**,
* der **Entscheidungsfindung** und
* der **Ausführungslogik**.

Die Evolution geht dabei vom starren Regelwerk (Automatisierung) über die assistierte Intelligenz (KI-Workflow) bis hin zur echten Autonomie (KI-Agent).

![Stufen der Prozessautomatisierung](https://picsur.myctx.net/i/42c68cf0-6e74-4ccd-83a8-d696049016d7.png)

## 1\. Automatisierungen (Regelbasiert und Statisch)

Automatisierungen sind die klassische Form der Prozessoptimierung und werden bei vielen Unternehmen bereits seit längerem eingesetzt. Automatisierung führt vordefinierte, regelbasierte Aufgaben aus, die entweder manuell, zeitgesteuert oder durch einen Auslöser gestartet werden.

### Kernmerkmale

* **LLM-Anrufe:** Es finden keine LLM-Aufrufe statt, die Logik ist frei von generativer Intelligenz.
* **Entscheidungsfindung:** Die Entscheidungslogik wird von Menschen vorher definiert (Wenn-Dann-Logik).
* **Ausführungslogik:** Der Prozess folgt einem vorgegebenen, statischen Pfad.
* **Vorhersagbarkeit:** Hoch, da die Ergebnisse wiederholbar sind.

### Vorteile & Nachteile

* **Vorteile:** Hohe Transparenz und Kontrolle, einfache und schnelle Implementierung, ideal für klar definierte, repetitive Aufgaben.
* **Nachteile:** Völlig unflexibel, kann sich nicht an dynamische oder unstrukturierte Eingaben anpassen.

### Verwendungszwecke & Use Cases

Automatisierungen sind der Go-to-Ansatz, wenn Prozesse klar definiert, stark reguliert und auf absolute Wiederholgenauigkeit angewiesen sind.

### Beispiele

* **ERP/HR:** Onboarding-E-Mail-Workflows, die bei einer Neueinstellung ausgelöst werden.
* **Finanzen:** Automatisches Preis-Update im System, wenn der Lagerbestand unter einen definierten Schwellenwert fällt.
* **IT:** Senden eines Alarms, wenn die CPU-Auslastung eines Servers 90 % überschreitet.

## 2\. KI-Workflows (Regelbasiert mit LLM-Assistenz)

KI-Workflows sind als eine Evolution der Automatisierung zu sehen. Sie folgen ebenfalls einem vorgegebenen Pfad, integrieren jedoch LLMs für einen oder mehrere spezifische, intelligente Schritte. Ein LLM wird mindestens einmal aufgerufen, um eine vorher definierte Aufgabe zu erledigen.

### Kernmerkmale

* **LLM-Anrufe:** Mindestens ein LLM-Aufruf pro Workflow.
* **Entscheidungsfindung:** Menschen definieren den Gesamtpfad, das LLM liefert Intelligenz für spezifische Teilschritte (z. B. Zusammenfassung, Klassifizierung etc.).
* **Ausführungslogik:** Der Prozess folgt einem vorgegebenen Pfad, der an bestimmten Stellen durch LLM-Ergebnisse beeinflusst wird.
* **Vorhersagbarkeit:** Hoch, die Verzweigungslogik ist definiert und weitestgehend transparent.

### Vorteile & Nachteile

* **Vorteile:** Ermöglicht die Automatisierung komplexerer Aufgaben durch intelligente Schritte (z. B. Zusammenfassen, Klassifizieren), behält hohe Transparenz durch vordefinierte Pfade.
* **Nachteile:** Bietet begrenzte Anpassungsmöglichkeiten für sehr komplexe oder stark variierende Abläufe, erfordert Basiswissen in Prompt Engineering.

### Verwendungszwecke & Use Cases

KI-Workflows sind die ideale Wahl, wenn ein grundlegender Ablauf stabil ist, aber innerhalb dieses Ablaufs intelligente Entscheidungen oder Textverarbeitungsaufgaben erforderlich sind.

### Beispiele

* **Kundendienst:** Ein eingehendes Support-Ticket wird automatisch von einem LLM analysiert und klassifiziert (Schritt 1 - KI Entscheidung) und dann anhand der Klassifizierung an die richtige Abteilung weitergeleitet (Schritt 2 - Automatisierung).
* **Dokumentenverarbeitung:** Ein Workflow parst eine unstrukturierte Schadensmeldung, und ein LLM fasst die wichtigsten Details zusammen und strukturiert sie vor (z. B. Schadenart, Datum), bevor ein menschlicher Sachbearbeiter übernimmt.

## 3\. KI-Agenten (Zielorientiert und Autonom)

KI-Agenten repräsentieren die höchste Evolutionsstufe. Sie führen nicht-statische Aufgaben autonom aus und sind zielorientiert. Anstatt einem festen Pfad zu folgen, entscheidet das LLM in jedem Schritt selbst, welche Aktion als Nächstes zur Zielerreichung notwendig ist.

### Kernmerkmale

* **LLM-Anrufe:** Mehrmalige LLM-Aufrufe bei jedem Entscheidungsschritt (Reasoning).
* **Entscheidungsfindung:** Das LLM entscheidet autonom die nächsten Schritte zur Zielerreichung.
* **Ausführungslogik:** Der Agent findet selbstständig den Weg durch eine Kette von LLM-Anfragen (Reasoning) sowie ggf. weiterer verbundener Werkzeuge.
* **Vorhersagbarkeit:** Geringer als bei Workflows, da die Schritte dynamisch sind, aber mit extremer Flexibilität.

### Vorteile & Nachteile

* **Vorteile:** Hohe Flexibilität und Anpassungsfähigkeit an dynamische, unklare Probleme, entlastet Teams von komplexen operativen Aufgaben, arbeitet rund um die Uhr.
* **Nachteile:** Höhere Kosten und Latenzen aufgrund mehrfacher LLM-Aufrufe, geringere Kontrolle/Transparenz, komplexerer Einrichtungsaufwand (Prompt-Design, Monitoring, Richtlinien).

### Verwendungszwecke & Use Cases

KI-Agenten sind immer dann notwendig, wenn der Entscheidungsbaum exponentiell anwachsen würde, die Umgebung dynamisch ist und der Weg zum Ziel nicht vorab definiert werden kann.

### Beispiele

* **Vertrieb:** Ein Sales-KI-Agent erhält das Ziel, einen Lead zu qualifizieren. Der Agent entscheidet autonom: Prüfe CRM -> Recherchiere Unternehmensnews -> Verfasse personalisierte E-Mail -> Sende E-Mail -> Erstelle CRM-Folgeaufgabe.
* **Support/DevOps:** Ein DevOps-KI-Agent erhält den Alert "CPU > 90%". Er entscheidet: Erhöhe Container-Cluster -> Teste Health-Checks -> Installiere Hotfix -> Dokumentiere die Änderung im Jira-System

**Mein Tip an dieser Stelle:** Wenn ein Prozess mit einer Automatisierung oder einem KI-Workflow lösbar ist, sollte kein Agent erstellt werden. Keep it simple.
{:.note title="Tip"}

## Strategische Handlungsempfehlung für CIOs: Der Weg zu mehr Autonomie

Die Integration dieser Technologien erfordert eine strategische Vorgehensweise, die nicht nur auf Technologie, sondern auch auf Governance und Unternehmenskultur abzielt. Wie ich das Thema angehen würde:

1. **Starten Sie mit dem Fundament (Automatisierung):** Bevor Sie in komplexe Agenten-Architekturen investieren, stellen Sie sicher, dass Ihre klar definierten Standardprozesse optimal durch klassische Automatisierungen unterstützt werden. Dies schafft schnell messbare Erfolge, senkt die Betriebskosten und legt die Basis für eine strukturierte Datenlage.

2. **Pilotieren Sie den LLM-Einsatz (KI-Workflows):** Führen Sie Pilotprojekte durch, um das Potenzial von LLMs in stabilen, aber datenintensiven Prozessen zu testen (z. B. der intelligente Schritt der Dokumentenklassifizierung oder -zusammenfassung). Dies baut internes Wissen im Prompt-Design und in der LLM-Integration auf.

3. **Governance vor Autonomie:** Echte KI-Agenten sind mächtige, aber weniger kontrollierbare Systeme. Implementieren Sie robuste KI-Testprogramme und klare Richtlinien, bevor Sie Agenten auf kritische End-to-End-Prozesse loslassen. Transparenz, ethische Grundsätze und Compliance müssen von Anfang an in der Architektur verankert sein.

4. **Skalierbarkeit und Interoperabilität sicherstellen:** Wählen Sie flexible und interoperable KI-Plattformen, die sich nahtlos in Ihre bestehenden Unternehmenssysteme (ERP, CRM) integrieren lassen. Agenten entfalten ihr volles Potenzial nur, wenn sie auf alle relevanten Tools zugreifen können.

5. **Fokus auf Geschäftswert:** Priorisieren Sie Anwendungsfälle, die einen klaren, messbaren Wertbeitrag liefern, sei es durch Kostensenkung, verbesserte Kundenzufriedenheit oder die Befreiung von Mitarbeitern für strategische Aufgaben. Der Fokus muss auf der Entlastung der Wissensarbeiter liegen, um die unternehmerische Wertschöpfung zu steigern.

Wie weit ist Ihr Unternehmen auf dem Weg zur Autonomie?

*[LLM]: Large Language Model
*[CRM]: Customer Relationship Management
*[ERP]: Enterprise Resource Planning
