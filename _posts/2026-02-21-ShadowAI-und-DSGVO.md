---
title: Shadow AI und DSGVO
date: 2026-02-21 08:12:45 +/-0000
categories: [Strategie, KI, Sicherheit, Governance]
tags: [shadow ai, compliance, dsgvo, datenschutz] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/1d567446-731c-47e5-87db-fcf3355f8341.png
---

## Wenn Mitarbeiter KI-Tools nutzen, die die IT-Abteilung nicht kennt

Stellen Sie sich folgendes vor, Ihr Vertriebsmitarbeiter tippt gerade die komplette Kundenhistorie der letzten drei Jahre in ein kostenloses KI-Tool ein, um eine bessere Verkaufspräsentation zu erstellen. Gut gemeint, aber  DSGVO-technisch ein absoluter Albtraum. Und es passiert gerade jetzt, genau in diesem Moment und in tausenden deutschen Unternehmen. Nachdem ich zuletzt Tools wie [OpenClaw][1] und [Mammouth][2] vorgestellt habe, ist es nun höchste Zeit, auch mal den unkontrollierten KI-Einsatz im Unternehmenskontext erneut zu beleuchten. Mein letzter [Beitrag zu dem Thema][3] ist übrigens jetzt fast schon ein Jahr alt und das Thema ist immer noch akut.

## Das stille Datenproblem in deutschen Unternehmen

ChatGPT, Claude, Perplexity, Notion AI - die Liste verfügbarer KI-Tools ist endlos, die Einstiegshürde ist niedrig und der Produktivitätsgewinn sofort spürbar. Das Problem, die IT-Abteilung weiß von nichts.

Dieses Phänomen hat einen Namen: **Shadow AI**. Und es entwickelt sich 2026 zu einer der gefährlichsten Compliance-Bedrohungen für deutsche Unternehmen und das nicht erst nach OpenClaw (aka. Clawbot / Moltbot).

Nach Angaben von **[Netwrix][4]** mussten bereits 37% der Organisationen ihre Sicherheitsstrategie aufgrund von KI-getriebenen Bedrohungen anpassen. Unternehmen mit hoher Shadow-AI-Nutzung erleiden Datenpannen, die im Durchschnitt **4,63 Millionen US-Dollar** kosten, deutlich mehr als bei Unternehmen mit kontrollierter KI-Nutzung.

## Shadow AI ist mehr als die klassische Shadow IT

Ein Dokument, das zum Beispiel in einer Dropbox landet, bleibt erstmal dort. Solange dieses Dokument keiner aktiv teilt oder die Dropbox kompromitiert wird, ist das vorerst noch kein Problem. Ein Kundendatensatz, der allerdings in ein öffentliches KI-Modell eingespeist wird, kann theoretisch (und wird das praktisch sicher auch) als Trainingsdaten in das nächste Modell-Update einfließen. Das gefährliche dabei ist, das Shadow AI die Daten unkontrolliert *repliziert*, Shadow IT im Gegensatz *speichert diese lediglich* in einem nicht von Unternehmen kontrollierten Bereich. Beides nicht schön, aber die Folgen von Shadow AI können durchaus unangenehmer werden.

Laut **Netskope** hat sich die Anzahl der Vorfälle, bei denen Mitarbeiter sensible Daten an KI-Anwendungen senden, innerhalb eines Jahres **verdoppelt**, auf durchschnittlich **223 Vorfälle pro Monat** und Unternehmen.

**Besonders tückisch:** Shadow AI versteckt sich oft nicht in neuen Tools, die von der IT-Security kontrolliert werden könnten, sondern in:

- **Browser-Erweiterungen** mit eingebautem KI-Assistenten
- **AI-Features in bereits genehmigten SaaS-Tools**, die still per Update aktiviert werden
- **Coding-Assistenten** wie GitHub Copilot, die Entwickler eigenständig installieren
- **No-Code-Agenten**, die Mitarbeiter selbst bauen – ohne Governance, ohne Audit-Trail

Typisches Ergebnis aus der Praxis: Die IT-Abteilung meldet 30 genutzte Cloud-Apps. Ein scharfgeschaltetes CASB meldet dann nach einer Woche 847 gefundene Cloud-Apps.

Das ist übrigens kein Witz. Das sind reale Zahlen aus echten Unternehmensaudits.

## Die DSGVO-Dimension: Vier klassische Verstöße auf einmal

Für deutsche Unternehmen ist die rechtliche Dimension besonders ernst. Shadow AI schafft gleich mehrere DSGVO-Verstöße gleichzeitig:

### **Keine Rechtsgrundlage (Art. 6 DSGVO)**
  
Wer Kundendaten in ein nicht genehmigtes KI-Tool eingibt, hat weder Einwilligung eingeholt noch einen Auftragsverarbeitungsvertrag (AVV) geschlossen.

### **Drittlandübermittlung ohne Schutzmaßnahmen (Art. 44–49 DSGVO)**  

Die meisten öffentlichen KI-Dienste betreiben ihre Server in den USA – ohne AVV ist jede Übermittlung personenbezogener Daten ein Verstoß. In einem einzigen Chat können so gleich dutzende Verstöße vorkommen!

### **Verletzung der Informationspflichten**  

Betroffene haben das Recht zu wissen, dass ihre Daten verarbeitet werden. Bei Shadow AI ist das schlicht unmöglich.

### **Lückenhaftes Verzeichnis der Verarbeitungstätigkeiten (Art. 30 DSGVO)**  

Shadow-AI-Nutzung taucht dort definitionsgemäß bei den wenigsten Unternehmen auf.

Die deutschen Datenschutzaufsichtsbehörden werden aktiver. Es ist nur eine Frage der Zeit, bis der erste große Shadow-AI-Fall öffentlich wird.

## Warum ein einfaches Verbot nicht funktioniert

Die erste Reaktion vieler IT-Abteilungen ist of ein wenig planlos und hat wenig Voraussicht. Die Lösung ist oft, einfach alles blockieren. Diesen, meiner Meinung nach falschen Weg, geht übrigens gerade das [EU Parlament][5] mit dem deaktivieren der KI-Funktionen auf den Endgeräten der Abgeordneten. Der Hintergrund is verständlich, löst aber wahrscheinlich das Problem nicht nachhaltig.

Der Mitarbeiter, dem ChatGPT oder Copilot gesperrt wird, nutzt es auf dem Privathandy und schickt die Ergebnisse per privater E-Mail zurück. Das Datenleck ist größer als zuvor. Verbote ohne Alternativen schaffen nicht weniger Shadow AI, sondern leider mehr und das auch nicht mehr durch entsprechende CASB Systeme kontrollierbar.

Die richtige Antwort ist nicht Restriktion, sondern kontrollierte und überwachte Bereitstellung. Nebenbei, auch aus Kostensicht ein guter Ansatz.

## Der Weg raus, ein pragmatischer Governance-Rahmen

### Schritt 1: Bestandsaufnahme

Bevor Sie regulieren, müssen Sie wissen, was tatsächlich im Einsatz ist. Tools wie Netskope, Microsoft Defender for Cloud Apps oder Zscaler schaffen vollständige Transparenz, auch über versteckte KI-Features in bestehenden SaaS-Tools.

### Schritt 2: Genehmigten KI-Stack aufbauen

Geben Sie Ihren Mitarbeitern sichere Alternativen, bevor Sie anfangen zu sperren:

- **Microsoft 365 Copilot** mit aktiviertem EU Data Boundary
- **Azure OpenAI Service** – Daten verlassen nicht Ihren Tenant
- **Aleph Alpha** – deutsches LLM, DSGVO-nativ, BSI-geprüft
- **On-Premises-Lösungen** (z.B. Ollama) für hochsensible Bereiche
- **Nenna.ai** deutsches Tool für DSGVO-konformes Prompt-Management und automatische Datenanonymisierung vor der KI-Übermittlung
  
### Schritt 3: AI Acceptable Use Policy

Regeln Sie schriftlich, welche Tools genehmigt sind und welche Daten an eine KI gesendet werden dürfen. Klären Sie auch, was bei Verstößen passiert. Und, wer mich kennt, weiß das ich immer wieder empfehle, den Betriebsrat frühzeitig mit einzubinden, denn KI am Arbeitsplatz berührt die Mitbestimmungsrechte nach **§ 87 BetrVG**.

### Schritt 4: Technische Schutzmaßnahmen

- **DLP-Systeme** konfigurieren, um das Hochladen sensibler Daten zu erkennen.
- **CASB-Lösungen** für Sichtbarkeit und Kontrolle über alle Cloud-Dienste.
- **Regelmäßige Shadow-AI-Audits** mindestens quartalsweise.

Das wären auch Punkte, bei denen über einen Managed Service nachgedacht werden sollte.

## Quick Wins vs. langfristige Strategie

| Zeitraum    | Maßnahme                                                                           |
| ----------- | ---------------------------------------------------------------------------------- |
| 0–4 Wochen  | Mitarbeiterbefragung starten, AI Use Policy veröffentlichen, DLP-Regeln einrichten |
| 1–6 Monate  | Genehmigten KI-Stack ausrollen, Schulungen, Betriebsvereinbarung, AVV-Prüfung      |
| 6–18 Monate | AI Governance Framework, Agenten-Register, Integration ins bestehende ISMS         |

Zur Budgetorientierung für ein mittelständiges Unternehmen mit etwa 200–1.000 Mitarbeiter:

- Ein CASB/DLP-Toolstack verursacht Kosten zwischen 15 bis 40 € pro User/Monat,
- ein externes AI-Governance-Framework zwischen ca. 15.000 bis 50.000 €.

Gegenrechnung: Ein einziger DSGVO-Verstoß kann Bußgelder von bis zu **4% des weltweiten Jahresumsatzes** bedeuten!

> **Kleiner Tip:** Viele deutsche Mittelständler haben bereits einen CASB in ihrer Microsoft-Lizenz und wissen es nicht. Das wäre mein erster Check bevor jemand etwas neues einkauft und implementieren möchte.
{: .prompt-tip }

## Shadow AI ist kein IT-Problem – es ist ein Führungsthema

Shadow AI entsteht nicht, weil Mitarbeiter böswillig sind. Sie entsteht, weil Menschen ihre Arbeit gut machen wollen und KI-Tools dabei wirklich helfen. Die Frage ist nicht *ob* KI im Unternehmen genutzt wird, sondern *wie* und in welchem Rahmen.

Wer jetzt einen klaren Governance-Rahmen schafft, positioniert KI-Compliance als Wettbewerbsvorteil, denn Kunden, Partner und Behörden werden künftig immer mehr Transparenz darüber verlangen, wie Unternehmen mit KI und Daten umgehen.

## Fazit und Ausblick

Shadow AI ist also schon mit der DSGVO allein ein Compliance-Albtraum. Aber keine Sorge, der Gesetzgeber hat noch nachgelegt. Den **EU AI Act** habe ich in diesem Artikel bewusst ausgeklammert, sonst wäre das hier kein Blogartikel geworden, sondern eher ein psychiatrisches Gutachten über den Zustand der deutschen IT-Governance. Das kommt allerdings noch in einem nächsten Artikel. Sie dürfen schon mal wieder Kaffee kochen, viel Kaffee.

*[AVV]: Auftragsverarbeitungsvertrag
*[CASB]: Cloud Access Security Broker
*[DLP]: Data Loss Prevention
*[ISMS]: Informationssicherheitsmanagementsystem

[1]: https://thomas-krampe.com/posts/openclaw-agentic-workplace/
[2]: https://thomas-krampe.com/posts/schluss-mit-dem-abo-chaos/
[3]: https://thomas-krampe.com/posts/KI_rechtskonform_einsetzen/
[4]: https://netwrix.com/en/resources/blog/shadow-ai-security-risks/
[5]: https://www.politico.eu/article/eu-parliament-blocks-ai-features-over-cyber-privacy-fears/
