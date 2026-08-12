---
title: Mein persönliches KI-Betriebssystem
date: 2026-08-12 08:13:00 +/-0000
categories: [Technologie, KI, Compliance]
tags: [second-brain, obsidian, claude-code, automatisierung, wissensmanagement] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/b773d97d-f665-4bf3-8242-e8d4148ca492.png
---

## Warum ich mir ein eigenes KI-Betriebssystem gebaut habe

Die meisten Menschen, die ich kenne, nutzen KI-Chatbots wie eine bessere Google Suche. Frage stellen, Antwort bekommen, Tab schließen. Jede Anfrage startet bei null, ohne Erinnerung an gestern, ohne Wissen über das eigene Unternehmen, die eigenen Kunden oder die eigenen Vorlieben. Genau das hat mich lange gestört, weil der eigentliche Wert einer KI nicht in der einzelnen Antwort liegt, sondern darin, dass sie mit der Zeit mitwächst. Wichtig war mir dabei der zentrale Zugang, damit alle Endgeräte, die ich verwende, den gleichen Status haben und das Gelernte wieder verwenden sowie aus Fehlern lernen können. Dieser Artikel ist somit eher eine persönliche Idee oder ein Konzept als ein Anleitung zum nachbauen.

Ich habe mir etwas gebaut, das ich für mich selbst iBrain nenne. Kein Produkt, kein fertiges Framework, sondern eine Kombination aus einem strukturierten Notizsystem, einem KI-Agenten mit dauerhaftem Zugriff darauf und einer Synchronisation, die dafür sorgt, dass dieses Wissen auf jedem Gerät verfügbar ist, das ich gerade in der Hand habe. Ein persönliches KI-Betriebssystem, wenn man so will. Der Agent ist nicht mehr nur ein Werkzeug, das ich gelegentlich befrage, sondern eine feste Instanz, die meinen Arbeitsalltag kennt und aktiv selbstständig pflegt. Das ist übrigens nichts wirklich neues oder sogar eine Idee von mir. Wenn ihr im Netz nach Second Brain sucht, findet ihr da mehrere Anleitungen zu dem Thema.

In diesem Artikel zeige ich, wie ich persönlich das technisch aufgebaut habe, ohne dabei auf ein bestimmtes Gerät oder einen bestimmten Anbieter festgelegt zu sein. Das Prinzip funktioniert mit einem Business-Notebook und einem privaten Rechner genauso wie mit zwei beliebigen anderen Geräten, mit GitHub genauso wie mit einer selbstgehosteten Gitea- oder GitLab-Instanz oder auch mit einem geteilten Ordner, wichtig ist dabei nur, dass alle Systeme auf sichere Art und Weise an die Dateien kommen.

## Ein Ort, ein Agent, volles Kontextfenster

Der Kern von meinem iBrain ist ein Obsidian-Vault, also lediglich ein Ordner voller Markdown-Dateien mit Wikilinks zwischen den Notizen den ich mit Obsidian verwalte. Obsidian deshalb, weil es bereits jede Menge nützliche Funktionen enthält und ich es über Plugins erweitern kann. Das ist für sich genommen nichts Neues, das Konzept des Zettelkastens oder Wiki gibt es schon lange. Neu ist, dass ein KI-Agent, in meinem Fall Claude Code, direkten Lese- und Schreibzugriff auf diesen gesamten Vault hat und ihn bei jeder Session aktiv "selbst" pflegt.

Es muss auch nicht zwingend Claude sein (hier macht natürlich Claude Cowork und Claude Code Sinn), im Prinzip geht alles, was auch auf das Filesystem zugreifen kann. 

Statt bei jeder Anfrage aus Rohdaten neu zu generieren, was das klassische Muster hinter den meisten RAG-Systemen ist, pflegt der Agent ein persistentes, inkrementell wachsendes Artefakt. Neue Informationen landen in einer Inbox, werden dort sortiert und in die passende Projekt- oder Bereichsnotiz übernommen. Ein zentrales Regelwerk, bei mir eine einzige Markdown-Datei im Vault-Root, legt fest, wie Notizen benannt werden, welche Ordnerstruktur gilt und wie sich der Agent verhalten soll. Der Agent liest diese Datei bei jeder Session und hält sich daran, ohne dass ich sie jedes Mal neu erklären muss.

Das Ergebnis ist ein Agent, der beim nächsten Gespräch weiß, an welchem Kundenprojekt ich letzte Woche gearbeitet habe, welche Aufgabe noch offen ist und wie ich normalerweise Texte und E-Mails formuliere. Kein Prompt-Engineering für den immer gleichen Kontext, der Kontext liegt einfach schon da.

## Zwei Geräte, ein Vault

Ich arbeite in der Regel an einem leistungsfähigen Business-Notebook für die Kundenarbeit und unterwegs schon mal an einem zweiten, privaten Gerät. Beide sollen jederzeit denselben Wissensstand haben, ohne dass ich manuell etwas kopieren oder abgleichen muss. Die Lösung dafür ist banal und genau deshalb robust, Git.

Der komplette Vault liegt in einem privaten Git-Repository, bei mir auf GitHub. Genauso gut funktioniert das auch mit einer selbstgehosteten Gitea- oder GitLab-Instanz (zum Beispiel als Docker Container auf einem VPS, zu dem ich später komme), wenn einem die eigene Kontrolle über die Daten wichtiger ist als der Komfort eines Cloud-Anbieters. Die Regel, die ich verwende, ist denkbar einfach und steht ebenfalls im zentralen Regelwerk, das der Agent bei jeder Session liest:

- Session-Start: `git pull --rebase --autostash`
- Session-Ende: `git commit` und `git push`

Der Agent führt das selbstständig aus, ohne dass ich daran denken muss. Für die Sicherheit nicht ganz unwichtig, läuft das bei mir über einen GitHub-PAT (Personal Access Token), den der Agent kennt. Das ist kein Design-Fehler, sondern eine unvermeidbare Eigenschaft, wenn ein Agent unbeaufsichtigt auf einem Server läuft statt Keychain-gestützt auf meinem Mac. Wichtig dabei ist nur eine Grunddisziplin, nicht an zwei Geräten gleichzeitig aktiv am Vault arbeiten. Ein `--rebase --autostash` fängt zwar kleine Unachtsamkeiten ab, löst aber keine echten inhaltlichen Konflikte auf, wenn zwei Sessions parallel schreiben. Aber das was ich hier gebaut habe, ist ja auch kein Multi-User System und sollte mit der nötigen Selbstdisziplin kein Problem darstellen.

> Binärdateien wie Bilder oder PDFs gehören bewusst nicht in dieses Git-Repository. Große, sich häufig ändernde Binärdaten sorgen für aufgeblähte Repositories und nutzlose Diffs. Dafür eignet sich ein dezentrales Sync-Tool wie z.B. Syncthing deutlich besser, das die Anhänge direkt zwischen den Geräten austauscht, ganz ohne zentralen Server und ohne Git-Historie. Wenn ihr der Cloud vertraut, geht natürlich auch Dropbox, Google Drive, iCloud Drive, OneDrive oder was ihr sonst so nutzt.
{: .prompt-tip }

Wie ein Vault zum Beispiel aussehen könnte:

```bash
iBrain
├── 00_Kontext           # Hier liegen alle Informationen über mich
│   ├── Angebot.md
│   ├── Arbeitsstil.md
│   ├── ICP.md
│   ├── Portfolio.md
│   ├── Über mich.md
│   └── index.md
├── 01_Inbox             # Hier schmeiße ich alles rein, Claude sortiert das
│   ├── Brain Dump.md
│   └── index.md
├── 02_Projekte
│   ├── Internal
│   └── index.md
├── 03_Bereiche
│   ├── Meetings
│   ├── Vertrieb
│   ├── Weiterbildung
│   ├── Privat
│   └── index.md
├── 04_Ressourcen        # Hier findet Claude zum Beispiel Skills oder Routinen
│   ├── Claude
│   ├── Compliance & Security
│   │   ├── AI-Agenten-Setup Härtungsplan.md
│   │   ├── NIS-2
│   ├── DevOps & Automation
│   │   ├── 12 MCP servers for your stack.md
│   │   ├── Ansible
│   │   ├── CI-CD
│   │   ├── Docker
│   │   └── Terraform
│   └── index.md
├── 05_Daily Notes
│   └── index.md
├── 06_Archiv
│   └── index.md
├── 07_Anhänge          # Das synchronisiert Syncthing für mich
│   └── index.md
├── 08_Skripte
│   └── index.md
├── 09_Vorlagen
│   ├── Daily Note.md
│   ├── Dokumentation Vorlage.md
│   ├── Meeting Notizen.md
│   ├── ThomasKrampe.potx
│   └── index.md
├── CLAUDE.md
└── Index.md
```

Im Prinzip könnt ihr das einrichten, wie ihr gern möchtet. Ich habe mich hier weitestgehend an die Best Practices im OKF (Open Knowledge Format) und vor allem an das LLM Wiki von Andrej Karpathy gehalten (dazu später mehr).

## Der Agent läuft dauerhaft auf einem eigenen Server

Der eigentliche Sprung von "KI-Notiz-Tool" zu "KI-Betriebssystem" kam für mich erst, als der Agent nicht mehr an ein einzelnes, gerade geöffnetes Gerät gebunden war. Dafür läuft bei mir eine Instanz von Claude Code dauerhaft auf einem eigenen, kleinen Server - in meinem Fall ein Core VPS (Virtual Private Server) bei einem Hoster mit 6 CPUs und 12 GB RAM und derzeit Ubuntu 24.04 (mein VPS kostet mich so ca. EUR 7,50 im Monat, also überschaubar). Wer dafür eine Empfehlung braucht, gerne als Kommentar oder schreibt mich direkt an. Voraussetzung für den Betrieb ist neben dem OS lediglich Node.js und die Claude Code CLI (und Syncthing als Daemon, falls ihr auch die Dateien braucht), das Repository wird dort ganz normal per Git geklont.

Damit der Agent auf diesem VPS auch eine getrennte SSH-Verbindung überlebt, läuft er nicht direkt im Terminal sondern in einer gekapselten tmux-Session. Wer tmux noch nicht kennt, kann sich das wie eine Terminal-App vorstellen, in der Claude weiterläuft, auch wenn die eigentliche SSH-Session getrennt wird.

Ein weiterer Baustein ist Remote Control. Claude Code lässt sich so einrichten, dass dieselbe Session, die auf dem Server läuft, auch über die mobile App gesteuert werden kann. In der Praxis heißt das, ich kann von unterwegs, ohne Laptop, über mein iPhone und die Claude App mit dem Agenten sprechen, der genau denselben Kontext hat wie zu Hause oder im Büro am Schreibtisch, weil es eben dieselbe, dauerhaft laufende Session ist und kein neuer, kontextloser Chat. Das ist der große Unterschied zwischen einer KI, die man benutzt, und einer KI, die man dabei hat. Durch den Sync über Git kann ich dann auch auf den anderen Devices mit den Ergebnissen weiterarbeiten, ich muss lediglich das Repo klonen.

![iBrain_Architecture.drawio](https://picsur.kngstn.eu/i/b196dd19-02c1-4bc2-81e6-94c750431aa2.png)

## Obsidian als Frontend, der Agent als Kollege

Der Server mit dem dauerhaften Agenten ist die eine Hälfte, Obsidian auf den beiden anderen Geräten ist die andere. Obsidian selbst braucht dafür keine Spezialintegration, es liest ganz normal die Markdown-Dateien aus demselben, per Git synchronisierten Vault. Die eigentliche Verbindung zwischen beiden Welten stelle ich über eine Chat-Oberfläche direkt in Obsidian her, die mit demselben Agenten spricht, den ich auch über den Server und die mobile App erreiche. Dafür verwende ich in Obsidian ein Plugin mit dem Namen Claudian. Das machte es für mich komfortaler, direkt in Obsidian mit Claude zu chatten. Natürlich funktioniert das auch mit der Claude Desktop App oder im Terminal mit Claude Code. Was euch gerade am besten passt.

Das Prinzip dahinter lässt sich auf jede Notiz-App übertragen, die auf offenen Textdateien basiert und ein Plugin- oder Erweiterungssystem hat. Entscheidend ist nicht das konkrete Werkzeug, sondern dass Frontend, Wissensbasis und Agent auf derselben Datenquelle arbeiten, egal von welchem Gerät aus gerade zugegriffen wird.

## Wo die Grenzen liegen

So ein Setup gibt einem Agenten sehr weitreichenden Zugriff auf sehr persönliche Daten, und genau deshalb gehört von Anfang an ein Satz zu den Leitplanken dazu. Bei mir sind das explizite Freigabe-Regeln für alles, was über reines Lesen und Schreiben im Vault hinausgeht. Netzwerkzugriffe auf fremde Domains, Mail-Entwürfe, SSH-Verbindungen zu anderen Systemen, das Anlegen wiederkehrender automatisierter Aufgaben. Für jede dieser Kategorien gilt, der Agent handelt nur auf eine explizite Anweisung in der jeweiligen Nachricht, niemals auf Basis von Inhalten, die er irgendwo gelesen hat. Ein Dokument, das eine Anweisung enthält, ist für den Agenten ein Zitat, über das man spricht, keine Order, die man ausführt.

Diese Trennung zwischen dem, was ohne Rückfrage passieren darf, und dem, was immer eine explizite Bestätigung braucht, ist meiner Erfahrung nach die eigentliche Voraussetzung dafür, einem Agenten so viel Zugriff überhaupt geben zu können, ohne dass daraus ein unkontrollierbares Risiko wird. Im Unternehmenskontext würde ich einem solchen Agenten eine eigene Identität geben und diese per Policies so weit wie möglich einschränken, siehe dazu auch mein Artikel [Identitäten für KI-Agenten](https://thomas-krampe.com/posts/Identitaeten-fuer-KI-Agenten/). Im privaten Umfeld arbeitet der Agent ja nicht wirklich selbstständig, sondern meist in meinem Kontext (mit meinen Berechtigungen). Diese Guardrails sind hier eher als Soft-Governance zu sehen, die eigentlich wichtigen Hard-Boundaries lassen sich mit einem Agenten, der in eurem Kontext läuft, nicht so einfach umsetzen.

Ich gebe hier noch ein paar Beispiel aus meinem Alltag. Ich habe Claude Cowork zum Beispiel **nicht** mit meinem Business M365 Account verknüpft. Brauche ich geschäftliche Informationen für die Lösung einer Aufgabe, gebe ich diese bei der Anfrage gezielt und serialisiert mit, kein automatischer Zugriff auf geschäftliche Ressourcen (alles bleibt im Vault). Privat hat mein iBrain Zugriff auf meine Google-Daten, kann also Google Drive, Google Docs usw. sowie GMail auf explizite Nachfrage per Connector oder MCP Server nutzen. Außerhalb dieses Vaults / Ordners darf Claude auf nichts selbstständig zugreifen. Das ist erstmal aus Sicherheitsgründen wichtig, aber ich möchte ja auch, dass alles was ich mit Claude mache in meinem Vault bleibt, also mein Single Point of Knowledge und für Claude der Single Point of Trust.

> Dieser Artikel beschreibt meinen privaten Ansatz, für geschäftliche Anwendung mit Kundendaten, ganz besonders im Hinblick auf die DSGVO und dem EU AI Act, ist ein anderes Thema. In meinem Blog findet ihr dazu reichlich Informationen und mein Team kann euch so ein System auch gern Multi-User fähig in eurem Unternehmen "Enterprise Ready" implementieren. Sprecht mich einfach dazu an.
{: .prompt-warning }

Wie so ein Wissensspeicher aufgebaut wird (früher haben wir dazu Wiki gesagt) beschreibt Andrej Karpathy in seinem Gist [LLM Wiki (Gist)](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) und bei Google finden wir die Regeln, in Form vom [Open Knowledge Format (OKF) SPEC.md](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md). Wenn ihr euer Vault erstellt, lasst das doch Claude mit Referenz auf diese beiden Dateien für euch machen. Was darüberhinaus noch wichtig ist, ist nicht nur eine gute Claude.md im Root des Vaults (wenn ihr Claude nutzt) sondern auch Hilfen wie Über mich.md, Arbeitsstil.md oder auch ICP.md und natürlich entsprechende Guard Rails.

Ihr wisst nicht wie Ihr das umsetzen sollt, kein Problem, bittet Claude das für euch zu analysieren, zu planen und letztendlich auch umzusetzen.

Der Witz dabei ist, ich muss (und will) euch hier gar nicht Schritt-für-Schritt erklären, wie ihr so etwas aufbaut, mal davon abgesehen, dass es unzählige Videos bei YouTube zu dem Thema gibt. Aber in diesen Videos baut "jemand anderes" etwas, was ihr lediglich "nachbaut". Bittet doch einfach Claude das **mit** und **für** euch aufzubauen. Ordnerstruktur, Frontmatter, regelmäßige Tasks, Guard Rails alles könnt ihr gemeinsam mit Claude aufbauen. Wenn euch etwas nicht gefällt, fragt Claude warum er das so gemacht hat und lasst es umbauen wenn es euch nicht gefällt. Nur so wächst euer Know-How und auch das Know-How eures KI-OS. Ganz nebenbei noch erwähnt, Claude selbst kann über Erweiterung, Konnektoren, MCP-Server und Plugins sowie Skills enorm erweitert werden und diese Power landet letztendlich auch in eurem iBrain.

## Fazit

Ein KI-Betriebssystem im hier beschriebenen Sinn ist kein einzelnes Produkt, das man kauft, sondern die Kombination aus drei eigentlich schon lange bekannten Bausteinen: einem offenen, textbasierten Wissensspeicher (auch wenn es _nur_ ein Ordner in eurem Filesystem ist), einem Sync-Mechanismus, der diesen Speicher über alle Geräte hinweg konsistent hält und einem Agenten mit dauerhaftem, aber sauber begrenztem Zugriff darauf. Keiner dieser Bausteine ist für sich genommen aufregend. **Git** gibt es seit mehr als zwei Jahrzehnten, Markdown-Notizen noch viel länger, tmux ist ein Werkzeug aus einer Zeit, in der Cloud-Computing noch kein Begriff war und Obsidian lediglich ein Texteditor um komfortabel mit Textdateien zu arbeiten. Achtung bei Syncthing, das ist eine reine P2P-Anwendung zum Synchronisieren von Dateien und kein Backup. Bei falscher Einstellung synchronisiert sich ein gelöschter Ordner schonmal in einen vollen, dann habt ihr zwei leere. Da sich eure Binärdateien nicht im Git-Repo befinden, wenn ihr meiner Beschreibung folgt, solltet Ihr diese Binaries in eine echte Offsite-Sicherung aufnehmen.

Der eigentliche Unterschied zu einem gewöhnlichen Chatbot ist, dass hier nichts verloren geht. Jedes Gespräch trägt zu einem wachsenden, durchsuchbaren und versionierten Wissensstand bei, der auf jedem Gerät verfügbar ist und den der Agent selbst pflegt. Wer viel mit einer KI arbeitet und regelmäßig das Gefühl hat, dieselben Zusammenhänge immer wieder neu erklären zu müssen, für den lohnt sich genau dieser Schritt von der Einzelanfrage zum dauerhaften, mitwachsenden System.

## Nützliche Links

- [Obsidian (gibt es für Windows, Linux, MacOS)](https://obsidian.md/download)
- [Claudian Plugin für Obsidian](https://github.com/YishenTu/claudian)
- [Syncthing](https://syncthing.net/)
- [tmux](https://github.com/tmux/tmux/wiki)
  