---
title: Goldsuche für die KI - Machen Sie Ihre Daten sofort nutzbar
date: 2025-11-10 09:00:00 +/-0000
categories: [Strategie, KI, Datenklassifizierung]
tags: [ki, daten, how-to, processes] # TAG names should always be lowercase
image:
  path: https://picsur.myctx.net/i/3a18cc5e-1382-491e-a1be-b778b72a1f8f.png
---

## Warum die KI Ihre Daten nicht versteht und wie Sie das ändern

Gerade diese Woche auch wieder auf der Microsoft AI Tour in Frankfurt in vielen Vorträgen gehört: KI kann nur valide Informationen liefern, wenn auch die Datenbasis stimmt. Oder kurz gesagt - trash in, trash out. Ihre Unternehmensdaten, egal ob in Word-Dokumenten, E-Mails, Datenbanken oder auf der alten Webseite sind für eine Künstliche Intelligenz (KI) wie ein unübersichtlicher Haufen Gold. Das Potenzial ist riesig, aber die KI kann es nicht direkt nutzen, weil die Daten nicht strukturiert sind und für z.B. einen KI-Agenten nicht oder nur mit großem Zeitaufwand verwertbar sind.

KI-Systeme wandeln die sauberen, strukturierten Daten dann in sogenannte Vektor-Embeddings um und speichern diese in einer Vektordatenbank. Diese Technologie nennt sich Retrieval Augmented Generation (RAG) und ist der Schlüssel zur Nutzung der Daten für KI-Systeme.

Damit KI-Agenten und -Systeme wie Copilot oder andere schnell, zuverlässig und vor allem richtig arbeiten, müssen die Daten in eine saubere, einheitliche und vor allem strukturierte Form gebracht werden. Diesen Prozess bezeichnen wir als "Data Cleansing" oder ganz Old-School Daten-Normalisierung (Formatierung). Die Qualität der KI-Ergebnisse ist zu 80 % abhängig von der Qualität der Input-Daten.

## Klassische Dokumente (Office, PDF) – Aufräumen ist das neue Training

Dokumente aus Suiten wie **Microsoft 365 (Word, Excel, PowerPoint)** oder vergleichbaren Office-Paketen sind ohne Frage das Herzstück des Unternehmenswissens.

* **Das Problem:** Diese Dokumente sind visuell ansprechend gestaltet und für den Konsum durch Menschen gemacht, inhaltlich sind sie aber eher chaotisch. Die KI stolpert über Header, Fußnoten und in Bilder eingebettete Texte.
  
* **Die neue Praxis:**
    Wir kennen sie alle, aber nur wenige nutzen sie wirklich, die Formatvorlagen. Gewöhnen Sie sich und Ihren Teams an, konsequent **Formatvorlagen** für Überschriften (`H1`, `H2`) und Absätze zu verwenden. Wenn die KI eine klare Struktur (`H1 = Thema`, `H2 = Unterpunkt`) erkennt, kann sie den Inhalt **zehnmal schneller** erfassen und zusammenfassen. Vermeiden Sie außerdem Textfelder und SmartArts, wenn der Inhalt rein informativ ist. Um Dokumente in Papierform zu nutzen und weiterzugeben, macht es sicher Sinn, auch auf das optische Erscheinungsbild zu achten. Um aber reine Informationen digital zu verwenden, ist das native Markdown ein besseres Format und eignet sich besser für Dokumente für KI-Systeme.

    Mindestens genauso wichtig wie das Dokument selbst sind die Metadaten. Verwenden Sie immer auch die Dokumenteigenschaften (z.B. Autor, Stichwörter, Abteilung). Diese Metadaten sind für die KI wie ein Inhaltsverzeichnis und helfen, relevante Dokumente ohne langes Durchsuchen zu finden und zu verarbeiten. Gut strukturierte Dokumente sind der schnellste Weg zu einem funktionierenden KI-Assistenten.

## Datenklassifizierung, der Schlüssel zur DSGVO-konformen KI

Bevor Sie mit der Datenbereinigung oder Anonymisierung beginnen, ist ein elementarer Schritt die Klassifizierung Ihrer Unternehmensdaten. Nur wenn Sie wissen, welche Daten Sie wo haben und wie sensibel diese sind, können Sie die richtigen Schutz- und Verarbeitungsmaßnahmen ergreifen, insbesondere im Hinblick auf die DSGVO.

* **Das Problem:** Viele Unternehmen haben keine klare, unternehmensweite Richtlinie, die definiert, welche Dokumente als **"öffentlich"**, **"intern"**, **"vertraulich"** oder **"streng geheim"** gelten. Diese fehlende Klassifizierung führt dazu, dass KI-Systeme möglicherweise Zugriff auf hochsensible Daten erhalten, die für ihre Aufgabe nicht notwendig sind. Dies ist ein direktes Compliance-Risiko.

* **Die neue Praxis:** Führen Sie ein klares und verbindliches Schema zur Datenklassifizierung ein und automatisieren Sie diesen Prozess, wo immer möglich (z.B. über die Vertraulichkeits-Labels in Microsoft 365 oder ähnlichen Systemen). Jedes Dokument, jede E-Mail und jede Datenbank muss mit einem eindeutigen Label versehen werden, das seinen Schutzbedarf kennzeichnet.

Legen Sie Regeln fest, welche Klassifizierungsstufe überhaupt in einen KI-Agenten oder ein Trainingsmodell eingespeist werden darf. Zum Beispiel:

* Öffentliche und interne Daten dürfen für allgemeine Wissens-KI verwendet werden.
* Vertrauliche Daten müssen vor der Verarbeitung pseudonymisiert oder anonymisiert werden.
* Streng geheime Daten (z.B. Finanzprognosen) sollten in den meisten Fällen komplett vom KI-System ausgeschlossen bleiben.

Die Datenklassifizierung ist der vorgelagerte Filter, der sicherstellt, dass die nachfolgende Zugriffskontrolle (RBAC) für Ihre KI-Agenten überhaupt erst funktioniert. Sie ist die erste und wichtigste Verteidigungslinie gegen den unbeabsichtigten Export kritischer Informationen durch Ihre neuen KI-Agenten.

## Webseiten und Intranet – Die Struktur, die KI liebt

Webseiten sind oft eine der reichhaltigsten, aber strukturell kompliziertesten Datenquellen. Die KI muss wissen, was Inhalt und was nur "Verpackung" ist.

* **Das Problem:** Viele Webseiten nutzen dynamische Inhalte, Pop-ups oder irrelevante Navigationselemente, die die KI als Rauschen interpretiert, was die Verwertung erschwert. Ein Blick in den Quelltext einer Website macht das sofort klar, der wirkliche Inhalt ist zwischen den ganzen Tags kaum zu finden. So geht es aber auch der KI, die dieses Rauschen erst herausfiltern muss.
* **Die neue Praxis:**
    Ungabhängig ob Sie einen Internetauftritt Ihres Unternehmens oder ein Intranetprojekt pflegen, achten Sie darauf, dass der Hauptinhalt klar von Menüs, Werbung oder Kopf- und Fußzeilen getrennt ist. Die KI profitiert enorm, wenn der wichtigste Text in klaren semantischen Tags wie z.B. `<article>`, `<main>` oder klar strukturierten Listen vorliegt. Vermeiden Sie verschachtelte Tabellen oder Tags für den reinen Textinhalt.

    Führen Sie auch regelmäßig einen Link-Check Ihrer wichtigsten Wissensseiten durch. Verwaiste oder fehlerhafte Links (`404 Fehler`) führen dazu, dass der KI-Agent seine Suche abbricht und aufgrund von fehlenden Daten gegebenenfalls falsche Antworten liefert. Ein kaputter Link ist für einen KI-Agenten wie ein Schlagloch auf dem Weg zur Antwort.

## Datensicherheit, der Schutz kritischer und persönlicher Daten

Die strengen europäischen Vorschriften (DSGVO) erfordern einen kontrollierten Zugang zu sensiblen Daten, die in die KI-Pipeline einfließen.

* **Das Problem:** Kritische Daten (wie Finanzdaten oder das sogenannte Intellectual Property) sowie personenbezogene Daten (PBD) werden zum Teil ungefiltert in Trainings- oder Abfrageprozesse geleitet, was zu massiven Compliance-Risiken führen kann.
* **Die neue Praxis:**
    Vor jeder Verarbeitung von Daten für KI-Modelle müssen Sie prüfen, ob evetuell verwendete personenbezogenen Daten (PBD) (Namen, E-Mail-Adressen, Kontaktdetails) zwingend für das Training benötigt werden. Testen Sie darüberhinaus auch, ob Ihre Mitarbeiter diese PBD in Prompts verwenden. Wenn ja, müssen Sie sicherstellend, dass diese **anonymisiert oder pseudonymisiert** werden, bevor sie in das KI-System gelangen.

    Stellen Sie auch sicher, dass die Zugriffsrechte, die in Ihren Quellsystemen (SharePoint, Datenbanken) gelten, unbedingt auf das KI-System übertragen werden. Ein KI-Agent darf nur die Dokumente sehen und verwerten, die auch der anfragende Benutzer sehen darf. Im Idealfall richten Sie für die verwendeten KI-Agenten entsprechende eigene Personas in Ihrem Identity Management System (IMS) ein. Das macht einen rollenbasierten Zugriff (RBAC) dieser Agents möglich und erlaubt eine granulare Kontrolle, in der zum Beispiel ein verwendeter Agent weniger Berechtigungen hat als der Benutzer, der ihn ausführt. Das wird dann besonders wichtig, wenn keine Menschen mehr involviert sind, sondern eine **KI-Agent-zu-KI-Agent** Kommunikation über vielleicht unklar definierte Schnittstellen stattfindet.

    Wichtig: Nur die Anonymisierung entfernt den Personenbezug endgültig und sollte das Ziel für Trainingsdaten sein, während Pseudonymisierung für RAG-Systeme oft ausreichend ist, aber zusätzliche Kontrollen erfordert.

    Saubere Daten sind in Europa immer anonymisierte Daten. Beginnen Sie KI-Projekte mit PBD-freien Daten, um juristische Fallstricke zu umgehen und sich nicht in eine ungewollte Situation zu begeben.

## Data Governanc - Datenaufbereitung ist der neue Business-Prozess

Datenaufbereitung ist keine einmalige IT-Aufgabe, sondern auch ein kontinuierlicher Business-Prozess. Indem Sie und Ihre Teams kleine, disziplinierte Schritte zur Strukturierung und Bereinigung Ihrer Daten gehen und dabei die europäischen Sicherheitsstandards einhalten, schaffen Sie die notwendige Basis, damit Ihre KI-Agenten vom "netten Gimmick" zum produktiven, vertrauenswürdigen Mitarbeiter werden. Die Zeit, die Sie heute in saubere Daten investieren, sparen Sie morgen in der Überprüfung fehlerhafter KI-Ergebnisse.

## Die entscheidende Zukunftsfrage

**Ist das Internet wie wir es kennen am Ende?**

Wenn KI-Agenten in Zukunft alle Daten besser, schneller und strukturierter verwerten können als der Mensch, brauchen wir dann überhaupt noch klassische Websites, Intranets und grafische Oberflächen oder werden alle menschlichen Interaktionen nur noch über Konversation (Chat/Sprache) mit der KI laufen, die ihrerseits dann nur noch die Daten im Hintergrund abfragt und für uns aufbereitet bereitstellt?

Ich freue mich auf die Diskussion mit euch.
