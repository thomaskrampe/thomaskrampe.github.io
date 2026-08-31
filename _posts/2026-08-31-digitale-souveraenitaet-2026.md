---
title: Digitale Souveränität 2026
date: 2026-08-31 11:48:00 +/-0000
categories: [Strategie, Compliance, Souveränität]
tags: [cloud,cade,compliance,souveränität] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/4b68489a-d416-4ca8-abd0-75e81a899c7c.png
---

## Was Data Act und CADA für Ihre Cloud-Strategie bedeuten

Ein bekannter IT-Leiter erzählte mir kürzlich sichtlich zufrieden von seinem neuen Rechenzentrum in Frankfurt. Das Thema digitale Souveränität ist für ihn damit erstmal erledigt. Auf meine Rückfrage, wer denn den Betrieb eigentlich verantwortet und wer im Ernstfall Zugriff hat, fiel die Antwort sehr zögerlich aus und zeigt deutlich, dass das Thema wohl doch noch nicht vom Tisch ist.

Diese Szene wiederholt sich in meinen Kundengesprächen ständig. Der Serverstandort ist die Frage, die am leichtesten zu beantworten und am leichtesten zu vermarkten ist. Ein "Hosted in Europe" Label auf dem Rechenzentrum löst noch kein Souveränitätsproblem. Wer betreibt die Infrastruktur tatsächlich, und wie abhängig bleibt man vom Software-Stack eines einzelnen Anbieters, sind die schwerer zu beantwortenden Fragen. Zwei EU-Regelwerke, die 2026 konkret werden, zeigen gerade sehr deutlich, warum diese Unterscheidung nicht akademisch ist, sondern in Verträge und Budgets gehört.

## Drei Ebenen statt einer Standortfrage

Mein persönlicher Fragen-Stack ist dabei denkbar einfach, ich nenne es meine drei Ebenen der Souveränität. Die drei Ebenen sind unabhängig voneinander zu beantworten, erst alle drei zusammen ergeben ein vollständiges Bild.

**Infrastruktur-Souveränität (Wo)** Hier geht es rein um den physischen Standort und den Rechtsraum. Das ist die Ebene, über die am meisten gesprochen wird, weil sie sich am leichtesten in einer Pressemitteilung oder einem Vertragsanhang unterbringen lässt, also die berühmten "Low hanging fruits".

**Betriebs-Souveränität (Wer)** Wer hat eigentlich die Admin-Schlüssel und schaltet sich im Notfall auf? Ein Rechenzentrum in Frankfurt hilft wenig, wenn sich Techniker aus einem Drittstaat für Wartung und Support verbinden können.

**Software-Souveränität (Wie)** Wie tief stecken Sie schon im bequemen goldenen Käfig des Anbieters? Kommen Sie im Zweifel ohne eine monatelanges und teures Projekt wieder heraus?

Am Beispiel des Microsoft-Mistral-Deals habe ich diese drei Ebenen [an anderer Stelle](https://thomas-krampe.com/posts/Microsoft-und-Mistral-Souveraenitaet-zu-Ende-gedacht/) bereits einmal konkret durchgespielt.

Die zwei folgenden Regelwerke setzen bei genau diesen Ebenen aus einer anderen Richtung an, nicht als freiwillige Selbstverpflichtung, sondern eben als echtes Gesetz.

## Der Data Act macht Betriebs-Souveränität ab Herbst 2026 zur Pflicht

Der EU Data Act bringt uns zwei getrennte Verpflichtungen mit unterschiedlichem Datum, die in der öffentlichen Debatte gern durcheinander gebracht werden.

Ab dem 12. September 2026 gilt "Access by Design". Hersteller vernetzter Produkte müssen Nutzern standardmäßig, einfach, sicher und unentgeltlich Zugriff auf die Daten geben, die ihre Geräte im Betrieb erzeugen. Die Pflicht betrifft neu hergestellte Produkte und Modellwechsel nach diesem Stichtag, bereits im Umlauf befindliche Geräte müssen nicht nachgerüstet werden. Für den Enterprise-IT-Alltag ist das eher ein Randthema, relevant vor allem für Hersteller vernetzter Hardware.

Die zweite Verpflichtung betrifft dagegen fast jedes Unternehmen direkt. Ab dem 12. Januar 2027, vier Monate später, verbietet der Data Act Wechselgebühren beim Anbieterwechsel für Cloud-Dienste. Genau der Kostenfaktor, der Multi-Cloud- und Exit-Strategien bisher regelmäßig ausbremst, weil ein Wechsel neben dem technischen Aufwand auch finanziell unattraktiv gemacht wurde, fällt damit gesetzlich weg. Details zu SaaS-Konstellationen und konkreten Wechselszenarien sind laut Berichterstattung noch nicht vollständig geklärt, in Deutschland liegt die Durchsetzung bei der Bundesnetzagentur. Wer jetzt Cloud-Verträge verhandelt oder verlängert, sollte Kündigungs- und Wechselklauseln explizit gegen diese Frist prüfen, nicht erst im Januar 2027.

Genau diesen Knackpunkt habe ich bereits auf LinkedIn mit meinem Beitrag [Cloud-Abhängigkeit als Risiko](https://www.linkedin.com/pulse/cloud-abhängigkeit-als-risiko-thomas-krampe-agthf/?trackingId=sC83FAfv87OJvOc28oPmBw%3D%3D) beschrieben. Dass eine saubere Exit-Option der Kern von Betriebs-Souveränität ist, halte ich nicht erst seit gestern für zentral, zumal es der Data Act jetzt auch wieder finanziell planbarer macht.

## CADA soll Souveränität messbar machen, und genau daran entzündet sich der Streit

Der Cloud and AI Development Act (CADA) verfolgt ein anderes Ziel. Erstmals ist verbindlich festzulegen, wann ein Cloud- oder KI-Dienst überhaupt als souverän gilt. Vier Stufen sind vorgesehen. Level 1 verlangt Datenverarbeitung und -speicherung in der EU. Level 2 verlangt zusätzlich nachgewiesene Unabhängigkeit von Drittstaaten und eine transparente Software-Lieferkette. Level 3 verlangt EU-Eigentümerschaft und EU-Kontrolle des Anbieters. Level 4 verlangt volle Kontrolle über den gesamten Software-Stack und den Nachweis, dass kein Drittstaat Einfluss nehmen kann.

Über die Kriterien sind sich die betroffenen Verbände alles andere als einig. Der europäische Cloud-Infrastrukturverband CISPE hält gerade die unteren beiden Stufen für zu lax, fast jeder Anbieter könne sie erfüllen, kritisch sei vor allem Anhang II, der europäischen Aggregatoren erlaubt, Dienste als CADA-konform anzubieten, selbst wenn außereuropäische Subunternehmer die Infrastruktur dahinter liefern. Die Digital SME Alliance fordert eine explizite europäische Beschaffungspräferenz statt einer bloßen Prüfpflicht und verweist darauf, dass europäische Cloud-Anbieter derzeit nur auf 13 Prozent Marktanteil kommen. Eco und der Branchenverband CCIA warnen umgekehrt vor politisch motivierten Whitelists und protektionistischen Hürden, Kriterien sollten sich an nachprüfbaren Sicherheitsergebnissen orientieren statt an der Herkunft eines Anbieters. Gaia-X wiederum begrüßt CADA als praktikablen Rahmen und verweist auf das eigene Zertifizierungssystem, über das perspektivisch rund 3.000 europäische Cloud-Dienste zertifiziert werden sollen.

Die Zahl, die den Streit einordnet, kommt von der Digital SME Alliance selbst. Drei US-Unternehmen halten zusammen rund 70 Prozent des Cloud-Markts, die daraus entstehende Abhängigkeit soll Europa jährlich etwa 264 Milliarden Euro kosten. Ob CADA daran etwas ändert oder nur ein neues Etikett schafft, das sich mit Level 1 und 2 relativ leicht erreichen lässt, ist genau die offene Frage. Einen verbindlichen Zeitplan für Verabschiedung oder Inkrafttreten gibt es bisher nicht.

## Was IT-Leiter jetzt konkret tun sollten

Daraus folgen zwei klare Aufgaben für Ihren Kalender. Der Data Act bringt ein festes Datum und eine klare Pflicht, hier lohnt sich der Kalendereintrag, Cloud-Verträge bis Januar 2027 auf Wechselgebühren und Kündigungsklauseln zu prüfen, unabhängig davon, ob ein Wechsel aktuell überhaupt geplant ist. Eine Exit-Option, die nichts mehr kostet, ist ein strukturell anderer Ausgangspunkt für die eigene Multi-Cloud-Diskussion als eine, die bisher an Wechselkosten scheiterte.

CADA dagegen ist noch in der Diskussion und weit weg. Wer jetzt schon auf ein fertiges Zertifikat wartet, wartet vermutlich lange. Selbst wenn CADA tatsächlich mal zum Tragen kommt, entscheidet der Ausgang des aktuellen Streits, ob Level 1 und 2 tatsächlich etwas taugen oder nur auf dem Papier gut aussehen. Sinnvoller ist es, die eigenen kritischen Workloads schon jetzt anhand der drei Souveränitätsebenen einer Einschätzung zuzuordnen, unabhängig von einer späteren offiziellen Einstufung. Wer jetzt bereits weiß, wo die eigene Infrastruktur-, Betriebs- und Software-Souveränität steht, kann eine spätere CADA-Stufe einfach einordnen, statt von ihr überrascht zu werden.

Beide Regelwerke ändern aber grundsätzlich nichts an der Kernaussage, die sich für mich seit Monaten in Kundengesprächen bestätigt. Der Serverstandort beantwortet nur die erste von drei Fragen. Der Data Act macht dabei Betriebs-Souveränität günstiger, CADA versucht, alle drei Ebenen in ein Punktesystem zu pressen, mit sehr ungewissem Ausgang. Wer beide Ebenen schon vor der gesetzlichen Pflicht kennt, muss im Januar 2027 nicht erst anfangen, seine Verträge zu lesen.

Wenn Sie Ihre Cloud-Verträge oder Ihre KI-Strategie gegen diese Entwicklungen prüfen möchten, melden Sie sich gerne über [LinkedIn](https://www.linkedin.com/in/thomaskrampe) oder per [E-Mail](mailto:t.krampe@previder.de).

## Quellen

- [heise online: Data Act: Access by Design wird ab Herbst 2026 zur Pflicht](https://www.heise.de/hintergrund/Data-Act-Access-by-Design-wird-ab-Herbst-2026-zur-Pflicht-11414659.html)
- [IT-BUSINESS: EU Cloud and AI Development Act – Streit um Souveränität](https://www.it-business.de/eu-cada-cloud-ki-souveraenitaet-streit-a-af3bd8db55c3aa5f7b799cce6e94c9c0/)
- [Cloud and AI Development Act, Shaping Europe's digital future (EU-Kommission)](https://digital-strategy.ec.europa.eu/en/policies/cloud-and-ai-development-act)
- Eigener Artikel: [Microsoft und Mistral - Souveränität zu Ende gedacht](https://thomas-krampe.com/posts/Microsoft-und-Mistral-Souveraenitaet-zu-Ende-gedacht/)
- Eigener Artikel: [Cloud-Abhängigkeit als Risiko.md](https://www.linkedin.com/pulse/cloud-abhängigkeit-als-risiko-thomas-krampe-agthf/?trackingId=sC83FAfv87OJvOc28oPmBw%3D%3D)
