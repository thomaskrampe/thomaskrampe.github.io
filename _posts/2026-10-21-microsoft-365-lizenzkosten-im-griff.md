---
title: Microsoft 365 Lizenzkosten im Griff
date: 2026-10-21 12:00:00 +/-0000
categories: [Strategie, FinOps]
tags: [microsoft, m365, finance] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/fb2d0e45-e36c-490f-a80a-b3fffa25bf08.jpg
---

Seit dem 1. Juli 2026 gelten für Microsoft 365 höhere Listenpreise. Bestandskunden haben das vielleicht noch nicht auf der Agenda, denn diese zahlen laut Microsoft bis zur nächsten Verlängerung den bisherigen Preis, erst mit der kommenden Verlängerung greift der neue Preis. Je nach Verlängerungstermin sehen Unternehmen die Erhöhung schon auf der Rechnung, andere erst in den kommenden Monaten. In beiden Fällen wird wohl die Frage beim IT-Leiter landen, was sich dagegen tun lässt.

Am Preis selbst können Sie leider nichts ändern, es sei denn, Sie wechseln den Anbieter. Das ist beim ersten Ärger über die Rechnung oft ein naheliegender Gedanke. Ich erlebe aber meistens, dass er sich bei genauerem Hinsehen nicht oder nur mit sehr großem Aufwand umsetzen lässt. Wer bei Microsoft bleibt, hat trotzdem etwas Spielraum, denn die Gesamtkosten richten sich nach Anzahl und Art der Lizenzen, die Sie tatsächlich benötigen.

## Was Microsoft geändert hat

Microsoft hat die neuen Listenpreise auf der [Lizenzseite zu den Preis- und Paketänderungen 2026](https://www.microsoft.com/en-us/licensing/news/2026-m365-packaging-pricing-updates) veröffentlicht (Stand 16. Februar 2026). Sie gelten ab dem 1. Juli 2026. Bei den Microsoft-365-Suiten mit Teams liegt der Aufschlag je nach Plan zwischen 5 und 33 Prozent, nur Business Premium bleibt unverändert.

| Plan                            | bisher    | ab Juli 2026 | Aufschlag   |
| ------------------------------- | --------- | ------------ | ----------- |
| Microsoft 365 Business Basic    | 6,00 USD  | 7,00 USD     | 16 %        |
| Microsoft 365 Business Standard | 12,50 USD | 14,00 USD    | 12 %        |
| Microsoft 365 Business Premium  | 22,00 USD | 22,00 USD    | unverändert |
| Microsoft 365 E3                | 36,00 USD | 39,00 USD    | 8 %         |
| Microsoft 365 E5                | 57,00 USD | 60,00 USD    | 5 %         |
| Microsoft 365 F3                | 8,00 USD  | 10,00 USD    | 25 %        |
| Microsoft 365 F1                | 2,25 USD  | 3,00 USD     | 33 %        |

Alle Angaben sind Listenpreise in US-Dollar pro Nutzer und Monat, jeweils für die Variante mit Teams. Laut Microsoft können sich die Preise je nach Land und Währung unterscheiden und ändern. Prüfen Sie in Ihrem Vertrag, wann Ihre nächste Verlängerung ansteht, denn ab dann gelten für Sie die neuen Preise.

Parallel verändert Microsoft die Zusammensetzung der Pakete. Laut Lizenzseite beginnt das im Juni 2026, zu den Erweiterungen zählen Copilot-Chat-Funktionen mit Zugriff auf Postfach und Kalender sowie auf Agenten in Word, Excel und PowerPoint. Ob das den Aufschlag rechtfertigt, hängt davon ab, was Sie davon tatsächlich nutzen. Bezahlen müssen ihn auch Unternehmen, die diese Funktionen gar nicht einsetzen. Das wäre dann aber ein anderer Artikel, wahrscheinlich mit dem Titel "Was Sie schon bezahlen, aber nicht einsetzen".

## Der Aufschlag ist selten das größte Problem

Kommen wir zur Verdeutlichung mal zu einem Rechenbeispiel auf Basis der Listenpreise: 250 Nutzer mit Business Standard zahlen künftig 1,50 USD mehr pro Nutzer und Monat, das sind 4.500 USD im Jahr. Bei 1.000 Nutzern mit Microsoft 365 E3 oder E5 sind es jeweils 3 USD mehr, also 36.000 USD im Jahr. Das ist zwar deutlich spürbar, aber gut planbar.

Schwerer wiegen aber die Lizenzen, die überhaupt nicht genutzt werden. Nehmen wir an, 10 Prozent der E5-Lizenzen hängen an ehemaligen Mitarbeitern, Testkonten oder Personen, die nur Mail und Teams verwenden. Bei 1.000 Nutzern wären das 100 Lizenzen zu je 60 USD im Monat, also 72.000 USD im Jahr. Das ist doppelt so viel wie der gesamte Preisaufschlag für alle 1.000 Nutzer. Die 10 Prozent sind eine Annahme aus meiner Erfahrung, keine belegbare Marktstatistik. Wie hoch der Wert bei Ihnen tatsächlich ist, zeigt ein Blick in Ihren Tenant.

## Wo Lizenzen typischerweise liegen bleiben

| Befund                             | Woran Sie ihn erkennen                                                                                                    | Maßnahme                                                                                          |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Konten ohne Anmeldung              | Letzte Anmeldung in Entra ID liegt mehr als 90 Tage zurück                                                                | Lizenz entziehen, Offboarding-Prozess prüfen                                                      |
| Ein Plan für alle                  | Alle Nutzer haben denselben Plan, obwohl die Aufgaben sehr verschieden sind                                               | Pläne nach Rolle zuschneiden, für Mitarbeiter ohne eigenen PC-Arbeitsplatz Frontline-Pläne prüfen |
| Doppelte Funktionen                | Einzellizenzen für Dienste, die im Plan bereits enthalten sind, etwa Entra ID P2 oder Defender for Endpoint Plan 2 bei E5 | Überschneidungen abgleichen, Zusatzlizenz kündigen                                                |
| Lizenzierte Postfächer ohne Person | Shared Mailboxes oder Raumpostfächer mit zugewiesener Lizenz                                                              | Lizenz entziehen, Shared Mailboxes bis 50 GB brauchen keine eigene                                |
| Copilot ohne Nutzung               | Der Copilot-Nutzungsbericht im Admin Center zeigt kaum aktive Nutzer                                                      | Lizenz neu zuweisen oder entziehen                                                                |

Ein weiterer Punkt betrifft die Business-Pläne von Microsoft. Diese sind auf 300 Nutzer begrenzt. Wer wächst, muss irgendwann zwangsläufig in die Enterprise-Pläne wechseln. Der Sprung von Business Premium (22 USD) auf Microsoft 365 E3 (39 USD) kostet 17 USD mehr pro Nutzer und Monat. Das gehört rechtzeitig in die Kostenplanung.

## Beim Einsatz von Copilot erst messen, dann ausrollen

Copilot wird pro Nutzer lizenziert. Ob sich die Lizenz lohnt, hängt stark vom Tätigkeitsprofil des Mitarbeiters ab. Wer den ganzen Tag Texte entwirft, Protokolle schreibt oder große Dokumente auswertet, profitiert eher als jemand, der überwiegend in einer Fachanwendung arbeitet.

Ich würde hier mit einem Pilotprojekt mit 20 bis 50 Nutzern aus unterschiedlichen Rollen über acht bis zwölf Wochen starten. Legen Sie vorher fest, woran Sie den Nutzen messen, etwa am Nutzungsbericht und an einer kurzen Befragung zur Zeitersparnis. Entscheiden Sie danach rollenbezogen, statt Lizenzen an alle zu verteilen.

Vorher sollten Sie allerdings die Berechtigungen prüfen. Copilot zeigt alles, was ein Nutzer auch sehen darf. Dateien, die jemand versehentlich zu breit freigegeben hat landen da schnell im Ergebnis. Warum die Basis zuerst stimmen muss, habe ich in meinem Artikel [Vom Modern Workplace zum Intelligent Workplace](https://thomas-krampe.com/posts/vom-modern-workplace-zum-intelligent-workplace/) beschrieben.

## Das weitere Vorgehen in vier einfachen Schritten

1. Bestand erfassen: Exportieren Sie im Microsoft 365 Admin Center eine Liste, um zu sehen, welcher Nutzer welche Lizenz zugewiesen hat.
2. Nutzung gegenprüfen: Legen Sie die Nutzungsberichte und die letzte Anmeldung aus Entra ID neben die Dokumentation des Lizenzbestands.
3. Pläne zuschneiden: Weisen Sie Lizenzen über Gruppen nach den jeweiligen Rollen zu, damit diese bei Ein- und Austritten automatisch mitwandern können. Nicht jeder Mitarbeiter braucht E7 (99 USD) oder E5 (60 USD), für viele Rollen reicht durchaus auch E3 (39 USD).
4. Termine festhalten: Überwachen Sie Laufzeiten und Verlängerungstermine in einem Kalender, denn mit einer Verlängerung greifen oft auch neue Preise.

> Tipp aus der Praxis: Wiederholen Sie die Prüfung mindestens zweimal im Jahr. Ein Lizenzbestand, der einmal bereinigt wurde, ist nach einem Jahr durch Eintritte, Austritte, Projekt- und Rollenwechsel schnell wieder unscharf.
{: .prompt-tip }

Die Listenpreise legt Microsoft fest, die Zahl der bezahlten, aber ungenutzten Lizenzen bestimmen Sie selbst. Wer seinen Bestand kennt, kann dem nächsten Verlängerungstermin gelassen entgegensehen. Der Previder M365 Tenant Check enthält dafür übrigens ein eigenes Modul zu Lizenzen und Kosten. Wenn Sie Ihren Bestand gegenprüfen lassen möchten, schreiben Sie mir über [LinkedIn](https://www.linkedin.com/in/thomaskrampe) oder per [E-Mail](mailto:t.krampe@previder.de).
