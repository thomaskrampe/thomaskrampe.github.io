---
title: Vom Modern Workplace zum Intelligent Workplace
date: 2026-09-14 11:47:00 +/-0000
categories: [Strategie, Compliance, KI]
tags: [ki,souveränität,agenticai,security] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/f4770463-fce8-44b8-baca-9b56adca9f84.png
---

Wenn wir in den letzten Jahren vom digitalen Arbeitsplatz gesprochen haben, meinten wir meist den Modern Workplace. Eine einheitliche Definition gab es selten, in der Praxis hing das Ergebnis davon ab, wer die Umgebung aufgesetzt und betrieben hat. Meist war es ein Mix aus klassischen VDI-Strukturen, Terminal-Servern oder Desktop-as-a-Service-Umgebungen von Microsoft, Citrix oder Parallels. 

Mit dem Aufkommen des „Intelligent Workplace” verschiebt sich dieser Fokus. Dabei geht es um deutlich mehr, als einen bestehenden Arbeitsplatz nur mit KI-Funktionen aufzubohren. Die praxistaugliche Architektur dafür folgt einem zweigleisigen Modell. Die Basis bildet weiterhin das Fundament aus Identitäten, Endgeräten, E-Mail, Collaboration und Security, also der klassische Modern Workplace. Darauf setzt ein zweiter Track auf, ein System aus Agenten, MCP-Schnittstellen (Model Context Protocol) und API-Integrationen, das eigenständig Workflows steuert und sich tief mit den bestehenden Fachanwendungen verbindet.

Am Ende laufen beide Schienen im Intelligent Workplace zusammen. Für die Geschäftsführung und die IT-Leitung ist das keine reine IT-Frage, sondern eine Entscheidung über Reihenfolge, Governance und Risikokontrolle.

![Titel-Vom-Modern-Workplace-zum-Intelligent-Workplace](https://picsur.kngstn.eu/i/2bc59df2-cb02-494f-a599-59953d757660.png)

## Das Fundament mit dem Modern Workplace

Bevor ein Agent autonome Aktionen im Unternehmen ausführen kann, müssen wir unsere Hausaufgaben in der Basis-Infrastruktur erledigt haben. Dazu gehören die saubere Identitäts- und Zugriffssteuerung über Entra ID (MFA, Conditional Access), die Geräteverwaltung mit Intune und Autopilot, gehärtete Mail-Streams, die Collaboration-Landschaft in Microsoft 365 sowie der geordnete Rückbau lokaler Fileserver. Ergänzt wird das durch durchgängige Security-Mechanismen via Defender, ein strukturiertes Tenant-Management und erste Data-Governance-Leitplanken für Copilot.

Diese Reihenfolge ist zwingend. Wer hier Lücken hat, sollte das Thema KI-Agenten vorerst parken und sich zuerst um die Basis kümmern. Einem autonomen System Zugriff auf Core-Business-Daten zu geben, während die Berechtigungsstruktur und der Datenzugriff unklar ist, hebelt jede IT-Sicherheit aus.

Diese Basis ist aber kein Zustand, den man einmal herstellt und dann abhakt. Tenants verändern sich ständig. Microsoft aktiviert fast täglich neue Features, häufig standardmäßig aktiviert, Admins ändern Berechtigungen im Tagesgeschäft, neue Mitarbeiter bekommen Zugriffe, alte werden nicht konsequent entzogen. Ich glaube ich muss hier nicht weiter ausführen. Jede dieser Änderungen kann eine vorher sauber konfigurierte Umgebung stillschweigend aufweichen, unabhängig davon, wie sorgfältig das initiale Setup war.

Ohne regelmäßige Kontrolle fällt diese Drift meist erst auf, wenn bereits etwas passiert ist. Ein wiederkehrender Tenant Check, der die zentralen Bereiche wie Identitäten, Geräte, Mail, Collaboration und Security automatisiert und in festen Abständen prüft, macht solche Abweichungen sichtbar, bevor sie zum Risiko werden, gerade wenn ein Agent auf Basis dieser Berechtigungen agiert. Bei uns inzwischen ebenso fester Bestandteil der Beratung wie das initiale Assessment.

## Was den Intelligent Workplace ausmacht

Erst auf dieser intakten Basis setzt die zweite Schiene an. Den Einstieg bildet im Microsoft-Ökosystem Microsoft Agent 365 als zentrale Steuerungsebene (Control Plane) für Inventarisierung, Monitoring und Lifecycle-Management aller eingesetzten Agenten. Wichtig ist die Abgrenzung zu Copilot Studio: Dort werden Agenten gebaut, Agent 365 verwaltet und kontrolliert sie danach, unabhängig davon, wo sie erstellt wurden. Erstellung und Governance sind zwei getrennte Bausteine, nicht ein Produkt mit zwei Namen. Darauf aufbauend kommen autonome Agenten zum Einsatz, die Arbeitsabläufe nicht mehr nur begleiten, sondern selbstständig ausführen. Über standardisierte MCP-Schnittstellen verbinden sich diese Agenten direkt mit den führenden Systemen wie ERP, CRM oder Ticketing.

Der Unterschied zum bisherigen Copilot-Einsatz ist grundlegend. Copilot agiert als Assistent, der dem Anwender zuarbeitet. Ein autonomer Agent mit MCP-Anbindung z.B. ans ERP führt Aktionen eigenständig aus, er liest, verarbeitet und schreibt Daten, ohne dass bei jedem Zwischenschritt ein Mensch auf „Bestätigen“ klicken muss.

## CLOUD Act, DSGVO und EU AI Act

Mit der autonomen Datenverarbeitung wächst auch die rechtliche Angriffsfläche. Jede MCP-Schnittstelle ist ein weiterer Kanal, über den Daten zwischen Fachanwendungen und den KI-Modellen der Hyperscaler fließen, und MCP ist als junges Protokoll noch nicht frei von Schwachstellen: Präparierte Tool-Beschreibungen oder ein kompromittierter MCP-Server können einen Agenten zu nicht freigegebenen Aktionen verleiten. Jede neue MCP-Anbindung braucht deshalb dieselbe Prüfung wie eine externe API, nicht das Vertrauen für ein internes Tool.

Hinzu kommt der US CLOUD Act, der US-Anbieter zur Herausgabe von Daten verpflichtet, unabhängig vom Serverstandort. Microsoft hat das für Frankreich öffentlich eingeräumt. Bei punktueller Copilot-Nutzung war dieses Risiko oft kalkulierbar, bei automatisierten Agentenzugriffen auf Finanzsystem, CRM und Support wird daraus ein handfestes Compliance-Problem. Agenten brauchen dafür eigene, verwaltete Identitäten. Microsoft bietet dafür inzwischen Entra Agent ID, aber ohne eigene Freigabeschwellen, lückenlose Protokollierung und einen Kill-Switch pro Agent bleibt der Einsatz ein Blindflug.

Rechtlich kommt der EU AI Act hinzu: Ein Agent, der in Bewerberauswahl oder Leistungsbeurteilung eingreift, fällt schnell in eine Hochrisiko-Kategorie mit Pflichten zu Risikomanagement und Dokumentation. Die vollen Pflichten greifen ab Dezember 2027, verschoben von ursprünglich August 2026, die Vorbereitungszeit dafür ist aber kürzer, als sie wirkt. Wer Agenten Richtung HR- oder Finanzprozesse ausrollt, sollte die Einstufung vorab klären, nicht erst wenn die Aufsichtsbehörde fragt.

## Was das für Geschäftsführung und IT-Leitung bedeutet

Für die Geschäftsführung steckt hinter der Governance-Frage auch eine Wettbewerbsfrage. Wer die Reihenfolge aus Fundament, Kontrolle und erst dann Autonomie sauber durchzieht, skaliert Agenten am Ende schneller und mit weniger Nacharbeit als ein Wettbewerber, der zuerst experimentiert und die Governance hinterherzieht.

Die Konsequenz daraus ist nicht das Blockieren von Agenten-Technologien, sondern eine risikobasierte Roadmap: Vor der Anbindung an produktive Fachsysteme steht die Datenklassifizierung, welche Daten fließen und welche Folgen eine Verarbeitung über US-Infrastrukturen hätte. Für hochsensible Inhalte wie Finanzdaten oder Personaldaten ist ein Setup auf europäisch kontrollierter Infrastruktur oft die verlässlichere Architektur, für Standardprozesse reichen meist ein durchdachtes Rechtemodell und lückenloses Logging.

Eine strategische Dimension kommt hinzu, die in der Risikobetrachtung oft fehlt: die Bindung an einen einzigen Anbieter. Wer Agent 365, Copilot Studio und MCP-Anbindungen tief in die eigene Prozesslandschaft einwebt, vertieft damit die Abhängigkeit von Microsofts Plattform spürbar stärker als durch die reine Nutzung von Microsoft 365, und ein späterer Wechsel oder eine Parallelstrategie wird von Jahr zu Jahr aufwendiger. Das ist kein Grund, auf Agenten zu verzichten, aber ein Punkt, den eine IT-Strategie von Anfang an mitdenken sollte.

Es gilt dieselbe Regel wie beim Modern Workplace. Identitätsmanagement, Zugriffskontrolle und Security sind keine optionalen Module, sondern die Fundament-Voraussetzung für jeden weiteren Schritt, ganz gleich, ob der Anwender am Ende Mensch oder Agent heißt.

## Der erste Schritt

Bevor die Agenten-Frage überhaupt sinnvoll zu beantworten ist, steht die Bestandsaufnahme. Wie stabil ist das Fundament in Ihrem eigenen Tenant tatsächlich? Genau das prüfen wir bei Previder mit dem M365 Tenant Check, zuletzt bei den [Hitzler Ingenieuren](https://previder.de/referenzen/hitzleringenieure): Der automatisierte Scan deckte dort eine Lücke bei den Conditional-Access-Regeln auf, die im laufenden Betrieb vermutlich nicht aufgefallen wäre. Der Kunde danach: „Wir dachten, wir sind sicher. Jetzt wissen wir es.“

Interesse an einer solchen Bestandsaufnahme statt Bauchgefühl für Ihren Tenant? Sprechen Sie mich einfach darauf an, Sie erreichen mich über [LinkedIn](https://www.linkedin.com/in/thomaskrampe) oder per [E-Mail](mailto:t.krampe@previder.de).
