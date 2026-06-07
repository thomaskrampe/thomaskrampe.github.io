---
title: SSH Config meistern
date: 2026-06-05 07:28:00 +/-0000
categories: [Tools, Linux]
tags: [ssh,linux,security,compliance] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/183fd0d2-b2b6-4ddc-8427-51dfd16b04b6.png
---

## Mehrere Kunden, ein Terminal

Ich habe gerade diverse Linux Systeme verwaltet und dabei mal nachgezählt. Aktive Kundenzugänge, auf die ich regelmäßig per SSH zugreife, sind aktuell 11 zuzüglich so 3-4 Testsysteme. Jeder Kunde hat eigene Konventionen, ein anderer Standardport, ein anderer Benutzername, ein anderer Key, und mindestens zwei davon haben Bastion Hosts vor der eigentlichen Infrastruktur.

Wer das noch manuell tippt, kann sich ausrechnen, was dabei passiert. Falsche Umgebung, falscher Key, im schlimmsten Fall der falsche Host. Nicht weil man unachtsam ist, sondern weil das Terminal per default keinen Unterschied zwischen Kunde A und Kunde B kennt, wenn man selbst keinen Prozess dafür hat. In der Regel verwenden viele dafür ein Tool und ja, es gibt sicherlich auch ein paar gute Tools dafür. Aber wenn diese Tools nicht richtig eingesetzt und konfiguriert werden, haben wir immer noch das gleiche Problem. Aber alles was wir benötigen, kann das Terminal bereits.

Die ganze Magie steckt in einer einzigen Datei, der SSH Config. Und diese Datei kann mehr, als die meisten nutzen.

## Was die Config-Datei eigentlich ist

Unter `~/.ssh/config` liegt eine Textdatei, die SSH vor jeder Verbindung liest. Sie ordnet Hostnamen Konfigurationsblöcken zu und übernimmt alles, was man sonst auf der Kommandozeile mitgeben müsste. Das Ergebnis ist `ssh kunde-a-prod` statt `ssh thomas@bastion.kunde-a.example.com -p 2222 -i ~/.ssh/keys/kunde-a/id_ed25519`. Das ist doch schon ein guter Anfang, oder?

Bevor wir anfangen, müssen wir dafür sorgen, dass die Datei die richtigen Berechtigungen hat, sonst ignoriert SSH sie kommentarlos.

```bash
chmod 600 ~/.ssh/config
chmod 700 ~/.ssh
```

Ein einzelner Host-Block in der Datei sieht so aus:

```bash
Host kunde-a-prod
    HostName 10.0.1.10
    User thomas
    Port 22
    IdentityFile ~/.ssh/keys/kunde-a/id_ed25519
    IdentitiesOnly yes
```

Prinzipiell selbst erklärend, aber `IdentitiesOnly yes` verdient eine genauere Erklärung, weil das Verhalten ohne diese Option kontraproduktiv ist.

Die Angabe von `IdentityFile` *ergänzt* die Liste der zu testenden Keys, es *ersetzt* sie nicht. SSH sammelt vor dem Verbindungsaufbau die Keys aus drei unterschiedlichen Quellen gleichzeitig. Alle im SSH-Agent geladenen Keys, die mit `IdentityFile` angegebenen Keys, und die Default-Dateien wie `~/.ssh/id_rsa` oder `~/.ssh/id_ed25519`. Diese gesamte Liste wird von SSH der Reihe nach durch probiert.

Das Problem dabei ist, dass SSH-Server in der Regel die Authentifizierungsversuche pro Verbindung begrenzen. OpenSSH setzt `MaxAuthTries` standardmäßig auf sechs. Wer fünf Kunden mit je zwei bis drei Keys im SSH Agent hat, kommt sehr schnell an diese Grenze, bevor der richtige Key überhaupt dran ist. Der Server bricht die Verbindung ab, und der Fehler sieht aus, als wäre der Key falsch, obwohl er eigentlich korrekt wäre und nur noch nicht "dran" kam.

`IdentitiesOnly yes` schaltet dieses Verhalten ab und SSH ignoriert den Agent und alle Default-Dateien und verwendet ausschließlich den in `IdentityFile` angegebenen Key.

## Struktur für mehrere Kunden

Wer mehrere Kundenzugänge und Hosts verwaltet, stellt schnell fest, dass eine einzelne Config-Datei schnell unübersichtlich wird. Die saubere Lösung ist die `Include`-Direktive, mit der sich pro Kunde eine eigene Datei anlegen lässt:

```bash
# ~/.ssh/config

Include ~/.ssh/config.d/*

Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    AddKeysToAgent yes
```

Der `Host *`-Block am Ende gilt als Fallback für alle Verbindungen, die keinem spezifischen Block zugeordnet sind. SSH liest die Blöcke von oben nach unten und nimmt den ersten Treffer, der spezifische Kundenblock gewinnt also immer gegenüber dem Fallback.

> Hinweis zu `AddKeysToAgent yes`: Diese Option lädt den privaten Key automatisch in den SSH-Agent, sobald er zum ersten Mal verwendet wird. Das ist praktisch, hat aber eine Wechselwirkung mit `IdentitiesOnly yes`: Keys, die im Agent landen, werden bei Verbindungen ohne explizites `IdentitiesOnly yes` wieder als Kandidaten angeboten. Wer mehrere Kunden verwaltet, sollte den Agent-Inhalt gelegentlich mit `ssh-add -l` prüfen und nicht mehr benötigte Keys z.B. mit `ssh-add -d ~/.ssh/keys/kunde-a/id_ed25519` entfernen. `IdentitiesOnly yes` in den Kundenblöcken sorgt dafür, dass der Agent dort ignoriert wird, aber ein voller Agent ist grundsätzlich ein Angriffsziel, falls der Agent-Socket kompromittiert wird.
{: .prompt-tip }

Pro Kunde liegt dann eine eigene Datei im Verzeichnis `~/.ssh/config.d/`:

```bash
mkdir -p ~/.ssh/config.d
chmod 700 ~/.ssh/config.d
```

Hier mal exemplarisch für einen Kunden.

```bash
# ~/.ssh/config.d/kunde-a

Host kunde-a-bastion
    HostName bastion.kunde-a.example.com
    User thomas
    Port 2222
    IdentityFile ~/.ssh/keys/kunde-a/id_ed25519
    IdentitiesOnly yes

Host kunde-a-*
    User thomas
    IdentityFile ~/.ssh/keys/kunde-a/id_ed25519
    IdentitiesOnly yes
    ProxyJump kunde-a-bastion

Host kunde-a-prod
    HostName 10.0.1.10

Host kunde-a-staging
    HostName 10.0.1.20

Host kunde-a-dev
    HostName 10.0.1.30
```

Der Wildcard-Block `kunde-a-*` enthält den gemeinsamen Benutzernamen und Key, damit er nicht für jeden einzelnen folgenden Host wiederholt werden muss. Die einzelnen `Host`-Blöcke darunter definieren nur noch die IP-Adresse. Das macht die Datei wartbar, wenn sich Passwörter oder Keys ändern.

Keys für jeden Kunden separat zu halten ist kein Aufwand, sondern Pflicht. Wenn ein Kunde einen Key widerrufen lässt oder eine Umgebung kompromittiert wird, betrifft das nur diesen Key. Alle anderen Kunden sind nicht betroffen.

```bash
# Key für einen neuen Kunden anlegen — immer Ed25519 (ECC), nie RSA
ssh-keygen -t ed25519 -f ~/.ssh/keys/kunde-a/id_ed25519 -C "thomas@kunde-a"
```

`ssh-keygen` erzeugt immer ein Schlüsselpaar, den privaten Key (id_ed25519) und den öffentlichen Key (id_ed25519.pub). Nur der öffentliche Key wird auf den Server übertragen und in `~/.ssh/authorized_keys` eingetragen. Der private Key verlässt niemals das lokale System.

Ed25519 ist ein ECC-Algorithmus (Elliptic Curve Cryptography) und die richtige Wahl für neue SSH-Keys. Kleiner, schneller, sicherer als RSA, und deutlich resistenter gegenüber zukünftigen Angriffen. Wer wissen will, warum ECC der aktuelle Standard sein sollte und welche Algorithmen auch in einer Welt mit Quantencomputern noch Bestand haben, dem empfehle ich meinen Artikel [Quanten-ready und performance-stark](https://thomas-krampe.com/posts/quanten-ready-und-performance-stark/).

## Bastion Hosts und ProxyJump

Manche Kundenumgebungen haben keinen direkten SSH-Zugang zu internen Servern. Der Weg führt über einen Bastion Host. Dafür gibt es zwei Optionen in der SSH Config, die sich konzeptionell unterscheiden.

`ProxyCommand` leitet stdin und stdout durch einen beliebigen externen Prozess. SSH sieht dahinter nur einen Bytestrom und weiß nicht, dass es über einen anderen SSH-Server läuft. Das erklärt auch das `-W %h:%p` in der klassischen Variante. `%h` und `%p` sind Platzhalter, die SSH durch Zielhost und Zielport ersetzt, bevor der Befehl ausgeführt wird.

```bash
# ProxyCommand — SSH behandelt die Verbindung als rohen Bytestrom
Host kunde-b-prod
    HostName 192.168.10.5
    ProxyCommand ssh -W %h:%p kunde-b-bastion
```

`ProxyJump` funktioniert etwas anders. SSH baut zuerst eine vollständige SSH-Verbindung zum Sprunghost auf und öffnet dann darin einen TCP-Kanal zum Zielhost. SSH weiß dabei, dass es über einen SSH-Server kommuniziert und zieht die Konfiguration des Springhosts direkt aus `~/.ssh/config`. `IdentityFile`, `User` und `Port` für den Bastion-Host müssen nicht inline mitgegeben werden, sie stehen bereits in der Config. Agent-Forwarding funktioniert sauber durch die Kette, ohne manuelle Eingriffe.

```bash
# ProxyJump — SSH verbindet sich zuerst vollständig mit dem Sprunghost
Host kunde-b-prod
    HostName 192.168.10.5
    ProxyJump kunde-b-bastion
```

`ProxyCommand` ist trotzdem nicht überflüssig: Es ist z.B. die einzige Option, wenn der Sprungserver nicht über SSH läuft, zum Beispiel über AWS Systems Manager Session Manager oder einen HTTP-Proxy. Für reine SSH-zu-SSH-Szenarien ist `ProxyJump` die bessere Wahl.

`ProxyJump` unterstützt auch Ketten, falls eine Umgebung mehrere Sprungpunkte hat:

```bash
Host kunde-c-db
    HostName 10.10.0.100
    ProxyJump kunde-c-bastion,kunde-c-management
```

SSH baut die Verbindung von links nach rechts auf, jeder Sprunghost zieht seine Konfiguration aus der lokalen `~/.ssh/config`. Kein separates Terminal-Fenster, kein manuelles Tunneling.

## ControlMaster für Ansible und Terraform

Wer mit Ansible, Terraform oder einem anderen Werkzeug arbeitet, das viele SSH-Verbindungen in kurzer Zeit aufbaut, kennt das Problem. Jede Verbindung geht durch den vollständigen Handshake. Bei zwanzig Hosts in einem Ansible-Playbook sind das zwanzig Handshakes, die Zeit kosten.

ControlMaster löst das durch Session-Multiplexing: Die erste SSH-Verbindung zu einem Host legt einen Unix-Socket an. Alle weiteren Verbindungen zum gleichen Host nutzen diesen Socket, ohne einen neuen Handshake.

```bash
Host *
    ControlMaster auto
    ControlPath ~/.ssh/sockets/%r@%h:%p
    ControlPersist 10m
```

```bash
mkdir -p ~/.ssh/sockets
chmod 700 ~/.ssh/sockets
```

`ControlPersist 10m` hält den Master-Socket zehn Minuten offen, auch nachdem die erste SSH-Session bereits geschlossen wurde. Ansible oder Terraform kann danach weiterhin die bestehende Verbindung nutzen. Nach zehn Minuten Inaktivität wird der Socket automatisch geschlossen.

>Hinweis zu ControlMaster und ProxyJump: Der ControlMaster-Socket liegt immer lokal auf dem eigenen Rechner und ist daher unabhängig von ProxyJump-Ketten erreichbar. ControlMaster funktioniert problemlos in Kombination mit ProxyJump für die Verbindung zum Zielhost. Was nicht funktioniert, ist einen ControlMaster-Socket zu verwenden, der auf einem Sprunghost selbst liegt, das ist aber kein üblicher Anwendungsfall. Bei komplexen Netzwerktopologien kann es vorkommen, dass unterschiedliche Netzwerkpfade zu demselben Zielhost führen, was zu Konflikten im Socket-Pfad führen kann. In solchen Fällen hilft ein eindeutigerer ControlPath, z.B. mit %C als Hash über alle Verbindungsparameter: `ControlPath ~/.ssh/sockets/%C`.
{: .prompt-tip }

## Port Forwarding in der Config

Port Forwarding ist ein weiteres Feature, das sich direkt in die Config-Struktur integrieren lässt und im Kundenbetrieb täglich nützlich ist. Die SSH Config kennt drei Varianten: `LocalForward`, `RemoteForward` und `DynamicForward`.

LocalForward macht einen Dienst im Kundennetz lokal erreichbar. Ein typischer Anwendungsfall ist der Datenbankzugriff: Die Datenbank läuft intern und ist von außen nicht direkt erreichbar, aber über die bestehende SSH-Verbindung kann ein lokaler Port dorthin weitergeleitet werden.

```bash
# ~/.ssh/config.d/kunde-a

Host kunde-a-db-tunnel
    HostName 10.0.1.10
    User thomas
    IdentityFile ~/.ssh/keys/kunde-a/id_ed25519
    IdentitiesOnly yes
    ProxyJump kunde-a-bastion
    LocalForward 15432 10.0.2.10:5432
    RequestTTY no
    ExitOnForwardFailure yes
```

Nach `ssh kunde-a-db-tunnel` ist die Datenbank des Kunden unter z.B. `localhost:15432` erreichbar, als wäre sie lokal. `RequestTTY no` verhindert, dass SSH eine interaktive Shell öffnet. `ExitOnForwardFailure yes` sorgt dafür, dass SSH sich beendet, wenn das Forwarding fehlschlägt, statt eine Shell ohne funktionierenden Tunnel zu öffnen.

Mehrere Ports lassen sich in einem Block kombinieren:

```bash
Host kunde-a-tunnel
    HostName 10.0.1.10
    User thomas
    IdentityFile ~/.ssh/keys/kunde-a/id_ed25519
    IdentitiesOnly yes
    ProxyJump kunde-a-bastion
    LocalForward 15432 10.0.2.10:5432
    LocalForward 19200 10.0.2.20:9200
    RequestTTY no
    ExitOnForwardFailure yes
```

RemoteForward funktioniert in die Gegenrichtung, ein Port auf dem Server wird auf einen lokalen Dienst weitergeleitet, nützlich z.B. für Webhooks in abgeschotteten Umgebungen, betrachte ich aber hier nicht weiter.

DynamicForward richtet einen lokalen SOCKS-Proxy ein, der gesamten Traffic durch die SSH-Verbindung leitet. Nützlich, wenn man kurzzeitig auf mehrere Dienste im Kundennetz zugreifen muss, ohne für jeden einen eigenen `LocalForward` zu definieren:

```bash
Host kunde-a-socks
    HostName bastion.kunde-a.example.com
    User thomas
    Port 2222
    IdentityFile ~/.ssh/keys/kunde-a/id_ed25519
    IdentitiesOnly yes
    DynamicForward 1080
    RequestTTY no
```

Nach `ssh kunde-a-socks` kann ein Browser oder ein anderes Tool so konfiguriert werden, dass es `localhost:1080` als SOCKS5-Proxy verwendet. Der gesamte Traffic läuft dann verschlüsselt durch die SSH-Verbindung ins Kundennetz.

## Konditionelle Konfiguration mit Match

Host-Blöcke matchen immer statisch auf den Namen, den man auf der Kommandozeile eingibt. Die `Match`-Direktive geht einen Schritt weiter und wertet Bedingungen zur Laufzeit aus. Das ermöglicht Konfigurationen, die sich je nach Kontext automatisch anpassen.

`Match exec` führt einen Shell-Befehl aus und wendet den Block nur an, wenn der Befehl mit Exit-Code 0 zurückkehrt. Ein praktischer Anwendungsfall ist die automatische Erkennung, ob man sich in einem VPN befindet:

```bash
# ~/.ssh/config

# Wenn das VPN aktiv ist (Route ins interne Netz vorhanden), direkt verbinden
Match host kunde-a-prod exec "ip route show | grep -q 10.0.0.0/8"
    ProxyJump none

# Andernfalls gilt der Kundenblock mit Bastion
```

`Match`-Blöcke werden in der Reihenfolge ausgewertet, in der sie in der Datei stehen, genau wie `Host`-Blöcke. Der Unterschied ist, dass Match-Bedingungen zur Laufzeit geprüft werden, nicht beim Parsen. Ein Match-Block, der vor einem Host-Block steht, kann dessen spätere Einstellungen nicht überschreiben, weil SSH nach dem Prinzip "first match wins" arbeitet. Das macht sie ideal für Ausnahmen, die nicht in die normale Kundenstruktur passen.

## Das vollständige Setup

Zusammengefasst sieht die vollständige Konfiguration so aus:

```bash
# ~/.ssh/config

Include ~/.ssh/config.d/*

Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    AddKeysToAgent yes
    ControlMaster auto
    ControlPath ~/.ssh/sockets/%r@%h:%p
    ControlPersist 10m
```

```bash
# ~/.ssh/config.d/kunde-a

Host kunde-a-bastion
    HostName bastion.kunde-a.example.com
    User thomas
    Port 2222
    IdentityFile ~/.ssh/keys/kunde-a/id_ed25519
    IdentitiesOnly yes

Host kunde-a-*
    User thomas
    IdentityFile ~/.ssh/keys/kunde-a/id_ed25519
    IdentitiesOnly yes
    ProxyJump kunde-a-bastion

Host kunde-a-prod
    HostName 10.0.1.10

Host kunde-a-staging
    HostName 10.0.1.20

Host kunde-a-dev
    HostName 10.0.1.30
```

Die Verzeichnisstruktur auf der Festplatte:

```bash
~/.ssh/
├── config                   # Haupt-Config mit Include
├── config.d/
│   ├── kunde-a              # Je Kunde eine Datei
│   ├── kunde-b
│   └── kunde-c
├── keys/
│   ├── kunde-a/
│   │   ├── id_ed25519       # Privater Key — verlässt diesen Rechner nicht
│   │   └── id_ed25519.pub   # Öffentlicher Key — wird auf Server übertragen
│   ├── kunde-b/
│   │   ├── id_ed25519
│   │   └── id_ed25519.pub
│   └── kunde-c/
│       ├── id_ed25519
│       └── id_ed25519.pub
└── sockets/                 # ControlMaster Sockets (leer im Normalfall)
```

## Verbindung prüfen

`ssh -v` gibt den Verbindungsaufbau aus und zeigt, welcher Konfigurationsblock und welcher Key tatsächlich verwendet werden. Nützlich beim ersten Einrichten und beim Fehlersuchen:

```bash
ssh -v kunde-a-prod
```

In der Ausgabe sieht man, welche Config-Datei geladen wurde, über welchen ProxyJump die Verbindung läuft und welcher Key akzeptiert wurde. Wenn etwas nicht funktioniert, ist `-v` der erste Schritt, kein Raten.

## Konfiguration prüfen mit ssh-audit

`ssh -v` zeigt, was SSH macht. `ssh-audit` zeigt, ob das, was SSH macht, sicher ist. Das Tool prüft Algorithmen, Cipher-Suites und MACs einer SSH-Verbindung und meldet veraltete oder unsichere Einstellungen.

`ssh-audit` ist über pip, Snap oder die meisten Paketmanager verfügbar.

```bash
pip install ssh-audit
# oder
sudo apt install ssh-audit
```

Die eigene Client-Konfiguration lässt sich ebenfalls auditieren. Dafür startet `ssh-audit` einen lokalen Listener, gegen den man sich mit dem eigenen SSH-Client verbindet.

```bash
# Terminal 1: Listener starten
ssh-audit --client-audit

# Terminal 2: Eigenen Client dagegen verbinden
ssh -p 2222 anything@localhost
```

Die Ausgabe zeigt, welche Algorithmen der eigene Client anbietet und welche davon als schwach oder veraltet eingestuft werden. Fertige Hardening-Guides für gängige Plattformen sind verfügbar und lassen sich direkt auf den globalen `Host *`-Block in der Config anwenden (siehe weiterführende Links).

## Was das im Alltag ändert

Einmal sauber aufgesetzt, braucht ein neuer Kunde eine neue Datei in `config.d/` und ein neues Key-Paar. Alles andere ist Vorlage. Bestehende Kunden profitieren sofort, wenn sich ein Benutzername oder ein Key ändert, weil die Änderung nur an einer Stelle gemacht werden muss.

Ansible und Terraform laufen schneller, weil der ControlMaster die Verbindung hält. Und das Terminal kennt jetzt den Unterschied zwischen Kunde A und Kunde B.

In der Praxis ist es der Unterschied zwischen einem Terminal, in dem man konzentriert arbeiten kann, und einem, in dem man zehn Prozent der Zeit damit verbringt, die richtige SSH-Kommandozeile zusammenzusuchen.

## Weiterführende Links

- [OpenSSH Manual — ssh_config(5)](https://man.openbsd.org/ssh_config)
- [OpenSSH Manual — ssh-keygen(1)](https://man.openbsd.org/ssh-keygen)
- [Quanten-ready und performance-stark — thomas-krampe.com](https://thomas-krampe.com/posts/quanten-ready-und-performance-stark/) — Hintergründe zu ECC und quantenresistenter Kryptografie
- [ssh-audit — SSH Server & Client Auditing Tool](https://github.com/jtesta/ssh-audit)
- [ssh-audit Hardening Guides](https://www.ssh-audit.com/hardening_guides.html)
