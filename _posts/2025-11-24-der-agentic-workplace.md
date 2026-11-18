---
title: Der Agentic Workplace und die Neubewertung des Arbeitsplatzes
date: 2025-11-24 09:00:00 +/-0000
categories: [Strategie, Arbeitsplatz, KI]
tags: [workplace,llm,ki,processes,agentic] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/f6a4d5b2-4698-4124-ac98-d24d4f3daac3.jpg
---

## Wie Multi-Agenten-Systeme die Arbeitswelt neu ordnen

Der Digital Workplace hat uns mit Tools zur Kollaboration versorgt, der Smart Workplace nutzt Generative KI zur Assistenz. Doch die nächste, tiefgreifendste Evolution ist der Agentic Workplace. Hier übernehmen Multi-Agenten-Systeme (MAS) die Automatisierung komplexer, domänenübergreifender Geschäftsprozesse, indem sie KI-Agenten in die Lage versetzen, autonom zu planen, zu kooperieren und Aufgaben zu orchestrieren.

Dieser technologische Sprung verändert die Rolle von Software grundlegend. Anwendungen werden von starren Werkzeugen zu flexiblen Mitarbeitern, die selbstständig handeln können und sich gegenseitig ergänzen.

## Von Co-Piloten zu autonomen Agenten

Der Unterschied zwischen einem generativen KI (GenAI) Copiloten und einem autonomen Agenten liegt in der Autonomie und Zielorientierung.

Ein Copilot assistiert einem menschlichen Benutzer bei der Erledigung einer einzelnen Aufgabe (z. B. einen E-Mail-Entwurf erstellen). Ein autonomer Agent hingegen ist ein Softwaremodul, das folgende Eigenschaften besitzt:

* **Zieldefinition:** Er erhält ein übergeordnetes Ziel (z. B. "Buche eine Geschäftsreise nach Berlin inklusive Terminkoordinierung").

* **Planung und Zerlegung:** Der Agent zerlegt das Ziel selbstständig in Sub-Tasks (Flug suchen, Hotel buchen, Kalender prüfen, E-Mail an Meeting-Teilnehmer senden).

* **Tool-Nutzung:** Er verwendet Tools (in der Regel APIs) der bestehenden IT-Landschaft, um Aktionen auszuführen (z. B. die API eines Flugbuchungsportals oder des internen Kalenders).

* **Speicher:** Er behält den Überblick über bisherige Schritte und Ergebnisse (Kurzzeit- und Langzeitspeicher).

* **Reflexion:** Er ist in der Lage, Fehler im Plan zu erkennen, Rückschläge zu verarbeiten und den Plan anzupassen und zu verbessern.

## Multi-Agenten-Systeme (MAS)

Ein Multi-Agenten-System entsteht, wenn mehrere autonome Agenten mit unterschiedlichen Rollen und Kompetenzen zusammenarbeiten, um ein gemeinsames komplexes Ziel zu erreichen.

* **Rollenverteilung:** Man kann sich ein MAS wie ein virtuelles Team vorstellen: Der "Planungs-Agent" übernimmt die Strategie, der "Datenbank-Agent" extrahiert relevante Informationen, und der "Kommunikations-Agent" interagiert mit externen Systemen oder Menschen.

* **Koordination:** Die Agenten kommunizieren über definierte Protokolle und Mechanismen, oft unter der Leitung eines Orchestrierungs-Frameworks, um Ressourcenkonflikte zu lösen und die Reihenfolge der Aktionen festzulegen.

## Orchestrierung und Integration in die IT-Landschaft

Der Erfolg von MAS hängt von der reibungslosen Integration in die bestehende IT-Infrastruktur ab. Die Agenten müssen Zugriff auf die richtigen APIs der Enterprise-Systeme (CRM, ERP, Kalender, Datenbanken) erhalten. Auch hier muss im Vorfeld auf Least Privilege sowie eine rollenbasierte Zugriffskontrolle /RBAC) geachtet werden, damit die Agenten nur auf die Daten kommen, auf die sie auch Zugriff benötigen.

Der Orchestrator (oder besser der Chief Operating Officer (COO) des MAS) ist das Herzstück des Systems. Er hat dafür zu sorgen, dass:

1\. Die gesamte Aufgabe effizient auf die spezialisierten Agenten verteilt wird.

2\. Die Ergebnisse der einzelnen Agenten konsolidiert und in den nächsten Arbeitsschritt überführt werden.

3\. Der Prozess überwacht und protokolliert wird, um Audits und Korrekturen zu ermöglichen.

Diese Orchestrierungsebene überbrückt die Lücke zwischen dem Ziel des Menschen (in natürlicher Sprache) und der technischen Ausführung durch die Backend-Systeme. Die Agenten führen dabei keine Aktionen direkt in den Hauptsystemen durch.

Die KI-Agenten arbeiten dabei nicht direkt im System. Stattdessen geben wir den Agenten einen digitalen Werkzeugkasten mit. Jedes Werkzeug ist eine Schnittstelle (API) einer Unternehmensanwendung (z.B. "Kundeninformation abrufen"). Der Agent wählt dann autonom das passende Werkzeug und wendet es mit den richtigen Daten an, um die Aufgabe zu lösen.

Auf diese Weise kann der Agent handeln, ohne die Sicherheitsmechanismen und Zugriffsrechte der Originalanwendung zu umgehen.. Dies gewährleistet, dass die Sicherheit und die Zugriffsrechte der ursprünglichen Anwendung erhalten bleiben.

## Beispiele aus der Praxis

Agentic Workplaces sind besonders dort entscheident, wo komplexe Geschäftsprozesse systemübergreifende Logik erfordern und wo bisher manuelle Übergaben nötig waren.

**Automatisierte Onboarding-Prozesse (HR):**

Ein HR-Agent triggert nach Vertragsunterzeichnung einen IT-Agenten zur Einrichtung von Benutzerkonten und Lizenzen, während ein Facilities-Agent die Bestellung des Arbeitsplatz-Equipments veranlasst. Der Fortschritt wird zentral vom Orchestrator überwacht.

**Intelligentes Supply Chain Management:**

Ein Sicherheits-Agent überwacht Log-Files und erkennt eine ungewöhnliche Zugriffsrate. Er leitet die Information an einen Ticket-Agenten weiter, der automatisch ein Incident-Ticket mit allen relevanten Log-Ausschnitten im ITSM System anlegt. Gleichzeitig informiert ein Kommunikations-Agent die verantwortlichen Mitarbeiter im internen Chat.

**Dynamische Datenanalyse und Reporting:**

Ein Analyse-Agent erhält die Anweisung: "Ermittle die Kunden mit den wahrscheinlich  höchsten Umsätzen im kommenden Quartal und erstelle daraus eine Präsentation." Der Agent ruft Daten aus dem CRM ab, führt Vorhersagemodelle aus, lässt einen Visualisierungs-Agenten Grafiken erstellen und übergibt die fertige Gliederung an den Präsentations-Agenten der dann eine entsprechende Präsentation dafür erstellt.

Durch Multi-Agenten-Systeme wird die digitale Arbeitswelt flexibler und widerstandsfähiger, da Prozesse nicht mehr nur skriptgesteuert ablaufen, sondern sich dynamisch an die Gegebenheiten anpassen können. Der menschliche Mitarbeiter verschiebt seinen Fokus vom Abarbeiten von Sub-Tasks zur Definition des strategischen Ziels und zur Überwachung der Agenten.

## Der Arbeitsplatz des Benutzers

Vergessen Sie die nächste Software-Suite. Der Agentic Workplace ist die unsichtbare Betriebssystem-Schicht (quasi ein *agenticOS*), die Ihre bestehende Infrastruktur orchestriert und bestehenden Anwendungen im Auftrag des Benutzers steuert. Gegebenenfalls können auch die bestehenden Anwendungen oder Frontendsysteme gleich ganz entfallen, wenn die Agenten Dateiformate oder Datenbanken ohne die entsprechende Anwendung lesen und schreiben können.

Die Anwendung auf dem Agentic Workplace des Benutzers geschieht hauptsächlich in vier Bereichen:

1\. **Zielorientierte Prozess-Orchestrierung (statt manueller Klicks)**
Der größte Mehrwert für den einzelnen Benutzer liegt in der Möglichkeit, komplexe, kettenartige Aufgaben mit einer einfachen Anweisung in natürlicher Sprache zu starten.

2\. **Intelligentes und vernetztes Wissensmanagement**
Agenten transformieren die Art und Weise, wie Benutzer Unternehmenswissen finden, nutzen  und in Zusammenhang bringen.

3\. **Proaktive und aufmerksame Überwachung**
Agenten können proaktiv agieren, erkennen Muster und melden Probleme, bevor sie kritisch werden.

4\. **Kontinuierliche Selbstoptimierung und Lernfähigkeit**
Der Agentic Workplace kann aus dem Verhalten und den Präferenzen des individuellen Benutzers lernen und seine Arbeitsweise entsprechend anpassen.

**Fazit:** Die Bereitstellung eines Agentic Workplace ist weniger eine einzelne Installation als vielmehr die Etablierung einer neuen, intelligenten Betriebsebene über Ihrer bestehenden IT-Infrastruktur. Sie beginnt mit der Strategie und endet mit der Befähigung der Mitarbeiter, diese neuen, autonomen Assistenten optimal zu nutzen.
