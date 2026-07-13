---
title: EU Chatkontrolle 2026 und die Rückkehr der Verschlüsselungsfrage
date: 2026-07-13 11:43:00 +/-0000
categories: [Tools, Sicherheit, DSGVO]
tags: [security, verschluesselung, chatkontrolle, dsgvo, recht, compliance] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/649d6dd4-5892-4ac7-bfe6-ded92f3be4af.png
---

## Was IT-Verantwortliche nach der Eilentscheidung wirklich wissen müssen

Am 9. Juli hat das EU-Parlament über die sogenannte Chatkontrolle abgestimmt, zwei Tage nachdem es mit 331 zu 304 Stimmen ein Eilverfahren dafür durchgewunken hatte. Seitdem laufen die üblichen Schlagzeilen durch die Feeds, von "Ende der Verschlüsselung" bis "Orwellsches Gesetz". Wer sich beruflich mit IT-Sicherheit beschäftigt, bekommt aktuell also wieder Fragen von der Geschäftsführung, vom Betriebsrat oder von Kunden. Und die meisten dieser Fragen lassen sich nicht mit einer Schlagzeile beantworten.

Ich ordne in diesem Artikel ein, was am 9. Juli tatsächlich beschlossen wurde, was das für Unternehmen bedeutet und was nicht. Der eigentliche Kern des Artikels ist aber ein anderer. Denn die Chatkontrolle-Debatte stellt eine Frage, die weit über Messenger-Dienste hinausgeht: Wer kontrolliert eigentlich die Schlüssel zu Ihren Daten? Und was können Sie technisch tun, um diese Kontrolle selbst zu behalten, unabhängig davon, wie eine Verordnung am Ende aussieht?

## Was am 9. Juli tatsächlich beschlossen wurde

Die folgende Tabelle zeigt den Ablauf der letzten Monate.

| Datum             | Was passiert ist                                                                                                                                   |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| 26. November 2025 | Rat einigt sich auf eine Position ohne verpflichtende Aufdeckungsanordnungen, verpflichtendes Client-Side-Scanning wird aus dem Entwurf gestrichen |
| 26. März 2026     | EU-Parlament lehnt die Verlängerung der freiwilligen CSAM-Scan-Ausnahme mit 228 zu 311 Stimmen ab                                                  |
| 3. April 2026     | Bisherige Rechtsgrundlage für freiwilliges Scannen läuft aus                                                                                       |
| 7. Juli 2026      | Parlament stimmt mit 331 zu 304 Stimmen für ein Eilverfahren                                                                                       |
| 9. Juli 2026      | Chat Control 1.0 wird angenommen                                                                                                                   |

Was am 9. Juli beschlossen wurde, ist im Kern eine Verlängerung der bestehenden, freiwilligen Ausnahme von der ePrivacy-Richtlinie. Diese Ausnahme erlaubt es Anbietern von Messenger-Diensten, freiwillig nach bekanntem Missbrauchsmaterial zu scannen, ohne dafür gegen den grundsätzlichen Schutz der Kommunikationsvertraulichkeit zu verstoßen. Sie gilt jetzt bis zum 3. April 2028. Das ist Chat Control 1.0.

Bemerkenswert dabei ist der Mechanismus der Annahme. Der Antrag, die Ratsposition abzulehnen, bekam mit 314 zu 276 Stimmen zwar eine Mehrheit, verfehlte aber die für die zweite Lesung nötige absolute Mehrheit von 361 Stimmen. Dadurch gilt die Ratsposition automatisch als angenommen, obwohl mehr Abgeordnete dagegen als dafür gestimmt haben.

Parallel dazu verhandeln Parlament und Rat weiter im Trilog an einer dauerhaften Verordnung, intern meist Chat Control 2.0 genannt. Genau die Frage, die am meisten Aufmerksamkeit bekommt, nämlich ob Anbieter zu einem verpflichtenden Scannen auf dem Endgerät gezwungen werden können, ist dabei aktuell nicht Teil des Entwurfs. Der Rat hat diese Passage im November 2025 aus seiner Verhandlungsposition gestrichen. Das kann sich im weiteren Trilog noch ändern, aber Stand heute schüren Headlines wie "Chatkontrolle killt Ende-zu-Ende-Verschlüsselung" unnötig Angst und geben den aktuellen Verhandlungsstand nicht korrekt wieder.

## Was das für Unternehmen bedeutet, und was nicht

Rechtlich betrifft die Chatkontrolle-Debatte in erster Linie Anbieter von Consumer-Messaging-Diensten wie WhatsApp, Signal oder Threema, die unter die ePrivacy-Richtlinie fallen. Microsoft Teams, Google Meet oder Slack in ihrer Funktion als Business-Collaboration-Tools laufen über eine andere Rechtsgrundlage, nämlich Verträge zur Auftragsverarbeitung nach DSGVO Artikel 28. Eine unmittelbare rechtliche Verpflichtung aus der Chatkontrolle-Verordnung für diese Tools sehe ich derzeit noch nicht.

Trotzdem würde ich Ihnen nicht empfehlen, das Thema für Ihr Unternehmen abzuhaken.

Die aktuelle Debatte zeigt eine politische Richtung. Der Wunsch nach mehr Zugriff auf verschlüsselte Kommunikation, sei es zur Missbrauchsbekämpfung, zur Terrorismusbekämpfung oder aus anderen Gründen, taucht in der EU seit Jahren in unterschiedlicher Form immer wieder auf. Wer heute eine Sicherheitsarchitektur plant, sollte nicht davon ausgehen, dass sich diese Fragen nicht irgendwann auch auf Business-Kommunikation ausweiten.

Wichtiger ist aber ein anderer Punkt: Die Frage "wer kontrolliert meine Verschlüsselung" ist für Business-Tools längst relevant, unabhängig von der Chatkontrolle. Wenn Microsoft, Google oder ein anderer Cloud-Anbieter die Schlüssel zu Ihren Daten verwaltet, ist es für Ihre Datensouveränität am Ende relativ egal, ob der Grund für einen möglichen Zugriff eine EU-Verordnung, eine Herausgabeanordnung auf Basis des US CLOUD Act oder eine interne Richtlinienänderung des Anbieters ist. In allen drei Fällen verlassen Sie sich auf das Versprechen eines Dritten, statt auf die eigene Kontrolle.

## Warum "Ende-zu-Ende-verschlüsselt" allein nicht die ganze Antwort ist

Viele Cloud-Dienste werben mit Verschlüsselung bei der Übertragung und im Ruhezustand (in transit / at rest). Das ist grundsätzlich gut und deutlich besser als überhaupt keine Verschlüsselung. Es beantwortet aber nicht die entscheidende Frage: Wer hält den Schlüssel?

Bei den meisten großen Collaboration- und Storage-Diensten liegt die Antwort beim Anbieter selbst. Microsoft verschlüsselt OneDrive-Daten, verwaltet aber in der Standardkonfiguration auch die Schlüssel dafür. Google verschlüsselt Meet-Aufzeichnungen in Drive, mit derselben Grundkonstellation. Das ist praktisch, weil Sie sich um nichts kümmern müssen. Es bedeutet aber auch, dass der Anbieter technisch in der Lage ist, auf Ihre Daten zuzugreifen, wenn er dazu verpflichtet wird, sei es durch ein Gericht, eine Behörde oder eine neue Regulierung.

Der einzige Weg, diese Abhängigkeit für wirklich sensible Daten zu vermeiden, ist clientseitige Verschlüsselung. Das heißt: Die Datei wird verschlüsselt, bevor sie überhaupt beim Cloud-Anbieter ankommt. Der Anbieter sieht dann nur noch einen verschlüsselten Blob, unabhängig davon, was er selbst an Verschlüsselung im Hintergrund anbietet. Genau dafür eignet sich ein Werkzeug wie zum Beispiel age.

## age: Verschlüsselung ohne den GPG-Ballast

age (gesprochen wie das englische Wort "age", offiziell auch als Abkürzung für "Actually Good Encryption" bekannt) ist ein von Filippo Valsorda entwickeltes, modernes Verschlüsselungstool für Dateien. Es verfolgt bewusst einen anderen Ansatz als GPG. Es hat keine Konfigurationsoptionen, kleine und einfach zu handhabende Schlüssel und eine Unix-typische Kombinierbarkeit mit anderen Tools über Pipes.

Installation unter macOS, Windows und den gängigen Linux-Distributionen:

```bash
# macOS
brew install age

# Windows (winget)
winget install --id FiloSottile.age

# Windows (Chocolatey)
choco install age.portable

# Debian/Ubuntu
sudo apt install age
```

Der Befehlssatz von `age` ist auf allen drei Plattformen identisch, nur bei mehrzeiligen Befehlen mit Zeilenumbruch unterscheidet sich die Syntax der Windows-PowerShell leicht vom Backslash-Stil unter macOS und Linux, dazu weiter unten mehr.

Ein Schlüsselpaar erzeugen Sie mit `age-keygen`:

```bash
age-keygen -o schluessel.txt
```

Die Ausgabe enthält den privaten Schlüssel in der Datei und den öffentlichen Schlüssel als Kommentar direkt im Terminal, in der Form `age1ql3z7hjy54pw3hyww5ayyfg7zqgvc7w3j2elw8zmrj2kg5sfn9aqmcac8p`. Nur der private Schlüssel muss geschützt werden, der öffentliche Schlüssel kann problemlos weitergegeben werden, genau wie bei asymmetrischer Verschlüsselung üblich.

Eine Datei verschlüsseln Sie gegen den öffentlichen Schlüssel:

```bash
age -r age1ql3z7hjy54pw3hyww5ayyfg7zqgvc7w3j2elw8zmrj2kg5sfn9aqmcac8p \
    -o vertragsentwurf.pdf.age vertragsentwurf.pdf
```

In der PowerShell wird der Zeilenumbruch statt mit `\` mit dem Backtick `` ` `` fortgesetzt, ansonsten ist der Befehl identisch:

```powershell
age -r age1ql3z7hjy54pw3hyww5ayyfg7zqgvc7w3j2elw8zmrj2kg5sfn9aqmcac8p `
    -o vertragsentwurf.pdf.age vertragsentwurf.pdf
```

Entschlüsselt wird mit dem passenden privaten Schlüssel:

```bash
age -d -i schluessel.txt vertragsentwurf.pdf.age > vertragsentwurf.pdf
```

Wer kein Schlüsselpaar verwalten möchte, kann age auch rein passphrasenbasiert nutzen, praktisch für den schnellen Einzelfall:

```bash
age -p -o geheim.zip.age geheim.zip
```

Seit Version 1.3.0 unterstützt age zusätzlich postquantensichere Schlüssel über das Flag `-pq` bei `age-keygen`. Für Daten, die auch in zehn oder zwanzig Jahren noch vertraulich bleiben müssen, zum Beispiel Verträge oder Personalunterlagen mit langer Aufbewahrungspflicht, ist das ein Punkt, den Sie jetzt schon mitdenken sollten, nicht erst wenn Quantencomputer eine reale Bedrohung für aktuelle Verschlüsselung darstellen. Mehr Hintergrund dazu auch in meinem Artikel [Quanten-ready und performance-stark](https://thomas-krampe.com/posts/quanten-ready-und-performance-stark/).

## Der praktische Anwendungsfall: Verschlüsseln vor dem Upload

Der eigentliche Wert von age für Unternehmen liegt nicht im Ersatz von OneDrive oder Google Drive, sondern in der Ergänzung. Für die große Mehrheit Ihrer Dateien ist die anbieterseitige Verschlüsselung ausreichend. Für eine kleine, klar abgrenzbare Kategorie sensibler Daten, zum Beispiel M&A-Unterlagen, bestimmte Personaldaten oder Zugangsdaten, lohnt sich der zusätzliche Schritt.

Ein einfacher Workflow dafür:

1\. Ein Team-Schlüsselpaar für einen bestimmten Datenkreis anlegen, zum Beispiel für die Geschäftsführung oder eine Projektgruppe.
2\. Sensible Dateien vor dem Hochladen in Teams, OneDrive oder Google Drive lokal mit `age` gegen den öffentlichen Schlüssel aller Berechtigten verschlüsseln. `age` unterstützt dabei mehrere Empfänger in einem Befehl:

```bash
age -r age1empfaenger1... -r age1empfaenger2... \
    -o quartalsplanung.xlsx.age quartalsplanung.xlsx
```

Unter Windows in der PowerShell wieder mit Backtick statt Backslash:

```powershell
age -r age1empfaenger1... -r age1empfaenger2... `
    -o quartalsplanung.xlsx.age quartalsplanung.xlsx
```

3\. Nur die `.age`-Datei landet in der Cloud. Der Cloud-Anbieter sieht ausschließlich einen verschlüsselten Blob, unabhängig von seiner eigenen Verschlüsselungskonfiguration.

4\. Jeder Berechtigte entschlüsselt lokal mit seinem privaten Schlüssel.

Für Teams, die Konfigurationsdateien, API-Schlüssel oder andere Secrets versionieren müssen, bietet sich zusätzlich SOPS an, ursprünglich von Mozilla gestartet und heute als CNCF-Projekt community-getragen, das age direkt als Backend unterstützt. Damit lassen sich einzelne Werte in YAML-, JSON- oder .env-Dateien verschlüsseln, während die Struktur der Datei lesbar bleibt und sich problemlos in Git versionieren lässt, ohne Klartext-Secrets im Repository zu haben. Wer zusätzlich eine Hardware-Bindung möchte, kann private Schlüssel über `age-plugin-yubikey` an einen YubiKey binden, statt sie als Datei auf der Festplatte zu halten.

## Was das konkret ändert

Anders als bei reinem Vertrauen in die Anbieter-Compliance bleibt es für Ihre clientseitig verschlüsselten Dateien größtenteils irrelevant, ob Microsoft oder Google die Vorgaben der DSGVO korrekt umsetzen, ob das EU-US Data Privacy Framework vor dem EuGH Bestand hat oder ob eine künftige Chatkontrolle-2.0-Verordnung doch noch weiter reichende Zugriffsmöglichkeiten vorsieht. Der Anbieter kann technisch nur herausgeben, was er auch entschlüsseln kann. Ohne Ihren privaten Schlüssel bleibt eine mit age verschlüsselte Datei für ihn ein Blob aus Zufallsdaten.

Das ersetzt nicht die grundlegende DSGVO-Compliance-Arbeit, also AVV, Datenregionen und Transfer-Impact-Assessments, die für den regulären Betrieb weiterhin notwendig sind. Es ist eine zusätzliche technische Schicht für die Daten, bei denen Sie sich nicht auf das Versprechen eines Dritten verlassen wollen.

## Fazit

Die Chatkontrolle-Entscheidung vom 9. Juli betrifft Ihr Unternehmen rechtlich vermutlich nicht direkt, zumindest nicht in der aktuellen Fassung als Chat Control 1.0. Aber sie ist ein guter Anlass, eine Frage zu stellen, die unabhängig von jeder einzelnen Verordnung Bestand hat: Wo in Ihrem Unternehmen verlassen Sie sich auf das Verschlüsselungsversprechen eines Cloud-Anbieters, wo Sie stattdessen die Kontrolle selbst behalten könnten?

Für die meisten Daten reicht die Antwort des Anbieters aus. Bei den übrigen ist ein Tool wie age kein großer Aufwand. Ein Nachmittag reicht, um einen Workflow für die sensibelste Datenkategorie in Ihrem Unternehmen aufzusetzen. Der beste Zeitpunkt dafür ist nicht, wenn die nächste Verordnung oder der nächste Vorfall Schlagzeilen macht, sondern jetzt.

Wenn Sie eine Einschätzung brauchen, welche Daten in Ihrem Unternehmen diese zusätzliche Schicht wirklich benötigen, oder Unterstützung beim Aufsetzen eines solchen Workflows, sprechen Sie mich gerne an, über [LinkedIn](https://www.linkedin.com/in/thomaskrampe) oder per [E-Mail](mailto:t.krampe@previder.de).

## Quellen und nützliche Links

- [Why Chat Control 1.0 is the EU's most Orwellian law yet, Euronews](https://www.euronews.com/next/2026/07/10/chat-control-10-passed-the-european-parliament-through-the-back-door)
- [age — A simple, modern and secure encryption tool, GitHub FiloSottile](https://github.com/FiloSottile/age)
- [age vs GPG: Modern File Encryption That Doesn't Make You Cry](https://sumguy.com/age-vs-gpg-modern-encryption/)
- [Using SOPS + age to Encrypt Files, Hey! Linux](https://blog.heylinux.com/en/2026/02/using-sops-age-to-encrypt-files/)
