---
title: 10 CLI-Tools die auf keinem UX-System fehlen sollten
date: 2026-03-10 11:02:00 +/-0000
categories: [macOS, Tools, Linux]
tags: [homebrew, ubuntu, cli, how-to ] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/fb97649c-0c6f-4312-b100-a93cb5619201.png
---

Mal ehrlich, jeder der sich mit DevOps auseinandersetzen möchte, kommt an einer Linux Konsole / Terminal nicht vorbei. Tatsächlich wird Linux know-how als eine der grundlegend benötigten Kernkompetenzen aufgeführt. Wer jetzt aber auch noch mit den klassischen Kommandos wie `top`, `cat` und `find` arbeiten soll, sollte jetzt weiter lesen und sich meine smarteren Alternativen anschauen.

Das Terminal ist nach wie vor das mächtigste Werkzeug in der Toolbox eines jeden IT-Profis aber nur, wenn man es auch vernünftig ausstattet. Die gute Nachricht dabei ist, das die Open-Source-Community in den letzten Jahren eine beeindruckende Anzahl moderner CLI-Tools hervorgebracht hat, die alte Unix-Klassiker nicht nur ersetzen, sondern regelrecht alt aussehen lassen.

In diesem Artikel stelle ich euch meine persönlichen Top 10 vor. Tools, die ich auf jedem System installiere, bevor ich ernsthaft anfange zu arbeiten. Ob Entwickler, DevOps-Engineer oder Systemadministrator, diese Liste ist für alle relevant, die das Terminal (auch unter macOS) täglich nutzen und dabei effizienter sein wollen.

## Die Qual der Wahl - Warum gerade diese 10

Kriterien für diese Liste waren einfach. Das Tool muss einen echten Mehrwert gegenüber dem Standard-Pendant bieten, stabil und aktiv gepflegt sein, auf Linux (in diesem Artikel Ubuntu) **und** macOS funktionieren und sich in realen Produktionsumgebungen bewährt haben. Kein akademisches Gedöns, sondern echte Tools die das Leben erleichtern.

Los geht's.

## 1. htop – Der Klassiker

### Was macht das Tool?

`htop` ist ein interaktiver Prozess-Viewer und die modernere Alternative zum uralten `top`. Es zeigt CPU-, RAM- und Swap-Auslastung in Echtzeit, erlaubt das direkte Beenden von Prozessen per Tastendruck und bietet eine farbkodierte, übersichtliche Darstellung,  alles direkt im Terminal. Wer mag, kann aber auch die etwas modernere Variante `btop` verwenden.

![htop][1]

### Warum unverzichtbar?

Weil `top` aus einer Zeit stammt, in der Bildschirme noch in Zeichen gemessen wurden und UX ein Fremdwort war. `htop` ist das, was `top` immer hätte sein sollen - sofort verständlich, navigierbar und tatsächlich nützlich. Wer zum ersten Mal auf einem neuen Server landet, öffnet `htop` – Punkt.

### Installation

**Ubuntu:**

```bash
sudo apt update && sudo apt install htop -y
```

**macOS (Homebrew):**

```bash
brew install htop
# oder
brew install btop
```

### Pro-Tipp

Mit `F2` öffnest du das Setup-Menü und kannst die Anzeige individuell anpassen, zum Beispiel CPU-Kerne einzeln anzeigen lassen oder die Farbgebung ändern. Besonders praktisch, mit `F4` kannst du Prozesse live filtern.

### Website des Entwicklers

- [htop.dev](https://htop.dev)
- [htop auf GitHub](https://github.com/htop-dev/htop)

## 2. Glances – Das System-Monitoring-Schweizer-Taschenmesser

### Was macht das Tool?

Glances ist ein systemübergreifendes Monitoring-Tool, das auf einen Blick alles zeigt, was auf einem System passiert: CPU, RAM, Netzwerk, Festplatten, Prozesse, Docker-Container und vieles mehr. Es läuft im Terminal, kann aber auch als Webserver oder REST-API betrieben werden.

![glances][2]

### Warum unverzichtbar?

Weil du mit einem einzigen Befehl einen vollständigen Systemüberblick bekommst – inklusive Docker. Besonders stark: Glances lässt sich im Client-Server-Modus betreiben, sodass du mehrere Systeme remote überwachen kannst, ohne ein vollständiges Monitoring-System aufsetzen zu müssen. Für schnelle Diagnosen auf Produktionssystemen Gold wert.

### Installation

**Ubuntu:**

```bash
pip3 install glances
# Oder mit Docker-Support:
pip3 install glances[docker]
```

**macOS (Homebrew):**

```bash
brew install glances
```

### Pro-Tipp

```bash
glances -w
```

Startet Glances als Webserver auf Port 61208 – perfekt für Remote-Monitoring über den Browser. Mit glances --export influxdb lässt es sich sogar direkt in eine InfluxDB + Grafana-Pipeline integrieren.

### Website des Entwicklers

- [nicolargo.github.io/glances](https://nicolargo.github.io/glances/)
- [Glances auf GitHub](https://github.com/nicolargo/glances)

## 3. eza – ls war gestern

### Was macht das Tool?

`eza` ist ein moderner Ersatz für den allbekannten ls-Befehl. Es bietet Farbhervorhebungen, Git-Integration (zeigt direkt den Status von Dateien im Repository), Icons, Baumansicht und eine deutlich lesbarere Ausgabe, alles out of the box.

![eza][3]

### Warum unverzichtbar?

Weil `ls -la` funktioniert, aber nicht schön ist. eza zeigt auf einen Blick, welche Dateien neu, geändert oder nicht getrackt sind – direkt im Verzeichnislisting. Das spart täglich mehrere mentale Kontextwechsel.

### Installation

**Ubuntu:**

```bash
sudo apt install eza
# Falls nicht im Repo:
cargo install eza
```

**macOS (Homebrew):**

```bash
brew install eza
```

### Pro-Tipp

Aliases in die ~/.bashrc oder ~/.zshrc eintragen, damit `ls` jetzt in schön funktioniert:

```bash
alias ls='eza --icons'
alias ll='eza -la --icons --git'
alias lt='eza --tree --icons --level=2'
```

Nach einem `source ~/.zshrc` habt ihr `ls` dauerhaft aufgewertet, ohne euch umgewöhnen zu müssen.

### Website des Entwicklers

- [eza.rocks](https://eza.rocks/) 
- [eza auf GitHub](https://github.com/eza-community/eza)

## 4. ncdu – Festplattenanalyse ohne Schmerzen

### Was macht das Tool?

`ncdu` (NCurses Disk Usage) ist eine interaktive Alternative zu du. Es analysiert den Speicherverbrauch eines Verzeichnisses und zeigt das Ergebnis in einer navigierbaren Listenansicht an – sortiert nach Größe, mit der Möglichkeit, Verzeichnisse direkt zu löschen.

![ncdu][4]

### Warum unverzichtbar?

Weil `du -sh /*` zwar gut funktioniert, aber dann kommt der Output und man weiß trotzdem nicht sofort, wo der Speicherplatz geblieben ist. ncdu zeigt es auf Anhieb, lässt sich durch die Verzeichnisstruktur navigieren und spart bei der Fehlersuche ("Wer hat /var/log vollgeschrieben?") enorm Zeit.

### Installation

**Ubuntu:**

```bash
sudo apt install ncdu -y
```

**macOS (Homebrew):**

```bash
brew install ncdu
```

### Pro-Tipp

`ncdu /`
Analysiert das gesamte Dateisystem. Mit d kann man Verzeichnisse oder Dateien direkt aus ncdu heraus löschen, natürlich mit Bestätigungsdialog. Auf Produktionssystemen bitte mit Bedacht einsetzen.

### Website des Entwicklers
- [dev.yorhel.nl/ncdu](https://dev.yorhel.nl/ncdu)
- [ncdu auf GitHub](https://github.com/nicowillis/ncdu)

## 5. bat – cat mit Superkräften

### Was macht das Tool?

`bat` ist ein cat-Ersatz mit Syntax-Highlighting, Zeilennummern, Git-Integration,  automatischer Paginierung und mein absolutes "must-have". Es unterstützt hunderte von Programmiersprachen und Konfigurationsformaten direkt out of the box.

![bat][5]

### Warum unverzichtbar?

Weil `cat datei.yaml` in einer langen Konfigurationsdatei schlicht unlesbar ist. `bat` färbt Syntax korrekt ein, zeigt sofort welche Zeilen geändert wurden (Git-Diff-Integration) und macht das Lesen von Dateien im Terminal zu einem angenehmen Erlebnis. Klingt nach Kleinigkeit, ist aber ein echter Produktivitätsgewinn.

### Installation

**Ubuntu:**

```bash
sudo apt install bat -y
```

> **Achtung:** Auf Ubuntu heißt der Befehl "batcat". Einfach als `alias bat='batcat'` in ~/.bashrc eintragen.
{: .prompt-tip }

**macOS (Homebrew):**

```bash
brew install bat
```

### Pro-Tipp

bat lässt sich hervorragend mit anderen Tools kombinieren:

```bash
# Als Pager für man-Pages:
export MANPAGER="sh -c 'col -bx | bat -l man -p'"

# Mit ripgrep kombinieren:
rg "suchbegriff" --pretty | bat
```

Themes können mit bat --list-themes angezeigt und mit --theme=TwoDark gesetzt werden.

### Website des Entwicklers

- [bat auf GitHub](https://github.com/sharkdp/bat)

## 6. gping – Ping trifft Visualisierung

### Was macht das Tool?

`gping` ist ein Ping-Tool mit integrierter Echtzeit-Graphendarstellung direkt im Terminal. Man kann mehrere Hosts gleichzeitig pingen und deren Latenz als ASCII-Graph visualisieren – farblich getrennt und in Echtzeit.

![gping][6]

### Warum unverzichtbar?

Weil ein einfaches `ping google.com` zwar sagt, ob ein Host erreichbar ist, aber nichts über Latenz-Schwankungen oder Paketverluste über Zeit verrät. gping macht Netzwerkprobleme sichtbar, bevor sie jemand am Helpdesk meldet. Besonders nützlich beim Vergleich von mehreren Endpunkten oder beim Testen nach Netzwerkänderungen.

### Installation

**Ubuntu:**

```bash
# Via Cargo (Rust):
cargo install gping

# Oder via apt (neuere Ubuntu-Versionen):
sudo apt install gping
```

**macOS (Homebrew):**

```bash
brew install gping
```

### Pro-Tipp

```bash
gping google.com cloudflare.com 1.1.1.1 8.8.8.8
```

Vergleicht vier Hosts gleichzeitig in Echtzeit. Perfekt um zu sehen, ob ein Latenzproblem providerintern oder beim Zielhost liegt.

### Website des Entwicklers

- [gping auf GitHub](https://github.com/orf/gping)

## 7. jq / yq – JSON und YAML endlich lesbar

### Was macht das Tool?

`jq` ist ein mächtiger Kommandozeilen-Prozessor für JSON-Daten. `yq` ist das funktionale Äquivalent für YAML (und XML, TOML). Beide erlauben das Filtern, Transformieren und Formatieren von strukturierten Daten direkt in der Shell.

### Warum unverzichtbar?

Weil die Welt aus JSON und YAML besteht – API-Antworten, Kubernetes-Manifeste, CI/CD-Pipelines, Docker Compose Files. Wer diese Daten noch mit grep und sed bearbeitet, hat entweder sehr viel Geduld oder sehr wenig Selbstachtung. jq und yq machen strukturierte Datenverarbeitung in der Shell zu einem Vergnügen.

### Installation

**Ubuntu:**

```bash
sudo apt install jq -y

# yq via snap:
sudo snap install yq

# Oder via GitHub Releases:
wget https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64 \
  -O /usr/local/bin/yq && chmod +x /usr/local/bin/yq
```

**macOS (Homebrew):**

```bash
brew install jq yq
```

### Pro-Tipp

```bash
# API-Antwort parsen:
curl -s https://api.github.com/repos/sharkdp/bat/releases/latest | jq '.tag_name'

# Kubernetes Pods nach Namespace filtern:
kubectl get pods -A -o json | \
  jq '.items[] | select(.metadata.namespace=="kube-system") | .metadata.name'

# YAML-Wert setzen:
yq e '.spec.replicas = 3' deployment.yaml
```

### Website des Entwicklers

- [jqlang.github.io/jq](https://jqlang.github.io/jq/)
- [jq aufGitHub](https://github.com/jqlang/jq)
- [mikefarah.gitbook.io/yq](https://mikefarah.gitbook.io/yq/)
- [yq aufGitHub](https://github.com/mikefarah/yq)

## 8. lazydocker – Docker-Management für Menschen

### Was macht das Tool?

`lazydocker` ist eine Terminal-UI für Docker und Docker Compose. Es zeigt Container, Images, Volumes und Netzwerke in einer übersichtlichen, navigierbaren Oberfläche an – inklusive Logs, Stats und der Möglichkeit, Container zu starten, stoppen oder neu zu starten.

![lazydocker][8]

### Warum unverzichtbar?

Weil `docker ps -a | grep ...` irgendwann aufhört, Spaß zu machen. `lazydocker` gibt einen sofortigen Überblick über den gesamten Docker-Zustand eines Systems, ohne dass man sich durch zehn verschiedene Befehle arbeiten muss. Besonders auf Systemen mit vielen Containern ist das ein echter Zeitsparer Wer Docker nutzt, weiß was ich meine.

### Installation

**Ubuntu:**

```bash
curl https://raw.githubusercontent.com/jesseduffield/lazydocker/master/scripts/install_update_linux.sh | bash

# lazydocker liegt standardmäßig in $HOME/.local/bin und muss 
# noch Global verfügbar gemacht werden
sudo ln -s $HOME/.local/bin/lazydocker /usr/local/bin/
```

**macOS (Homebrew):**

```bash
brew install lazydocker
```

### Pro-Tipp

```bash
# Alias für schnellen Zugriff:
alias lzd='lazydocker'
```

Mit e auf einem Container öffnet man direkt eine Shell-Session, kein `docker exec -it container_name /bin/bash` mehr nötig. Die Log-Ansicht mit Echtzeit-Updates ersetzt `docker logs -f` komplett.

### Website des Entwicklers

- [lazydocker auf GitHub](https://github.com/jesseduffield/lazydocker)

## 9. ripgrep – grep auf Steroiden

### Was macht das Tool?

ripgrep (kurz: `rg`) ist ein extrem schnelles Such-Tool, das rekursiv in Dateien nach Mustern sucht. Es ist deutlich schneller als grep, respektiert `.gitignore`-Dateien automatisch, unterstützt reguläre Ausdrücke und zeigt Ergebnisse farblich hervorgehoben an.

### Warum unverzichtbar?

Weil `grep -r "suchbegriff" . langsam ist und die Ausgabe schwer lesbar ist. `ripgrep` macht dasselbe in einem Bruchteil der Zeit, ignoriert automatisch irrelevante Verzeichnisse und liefert sofort verwertbare Ergebnisse. In großen Codebasen ist der Unterschied dramatisch.

### Installation

**Ubuntu:**

```bash
sudo apt install ripgrep -y
```

**macOS (Homebrew):**

```bash
brew install ripgrep
```

### Pro-Tipp

```bash
# Nur in bestimmten Dateitypen suchen:
rg "TODO" --type py

# Mit Kontext-Zeilen:
rg "error" -C 3 /var/log/

# Dateien ohne Match anzeigen:
rg --files-without-match "Copyright"

# Kombiniert mit bat für schöne Ausgabe:
rg "config" --pretty | bat
```

`ripgrep ist auch die Standard-Such-Engine für fzf-Integrationen und viele VS-Code-Extensions – es lohnt sich also doppelt.

### Website des Entwicklers

- [ripgrep auf GitHub](https://github.com/BurntSushi/ripgrep)

## 10. duf – df, aber verständlich

### Was macht das Tool?

`duf` ist ein moderner Ersatz für `df` (Disk Free). Es zeigt den freien und verwendeten Speicherplatz aller eingebundenen Datenträger in einer übersichtlichen, farbkodierten Tabelle an – inklusive Netzwerklaufwerke, Loop-Devices (optional ausblendbar) und einer Fortschrittsbalken-Darstellung.

![duf][10]

### Warum unverzichtbar?

Weil `df -h` zwar funktioniert, aber die Ausgabe inklusive aller /snap-Loop-Devices und tmpfs-Einträge unleserlich ist. duf filtert automatisch sinnvoll, zeigt relevante Informationen sofort und sieht dabei noch gut aus. Kleines Tool, großer Effekt.

### Installation

**Ubuntu:**

```bash
sudo apt install duf -y
```

**macOS (Homebrew):**

```bash
brew install duf
```

### Pro-Tipp

```bash
# Nur lokale Datenträger anzeigen:
duf --only local

# Als JSON ausgeben (z.B. für Monitoring-Skripte):
duf --json | jq '.[] | select(.mount_point=="/") | .free'
```

### Website des Entwicklers

- [duf auf GitHub](https://github.com/muesli/duf)

## Alles auf einmal installieren

Weil niemand zehn Befehle einzeln tippen möchte, hier das komplette Setup als einzeiliges Skript:

**Ubuntu/Debian:**

```bash
sudo apt update && sudo apt install -y htop ncdu bat eza ripgrep duf jq && \
pip3 install glances && \
sudo snap install yq && \
curl https://raw.githubusercontent.com/jesseduffield/lazydocker/master/scripts/install_update_linux.sh | bash && \
cargo install gping
```

**macOS:**

```bash
brew install htop glances eza ncdu bat gping jq yq ripgrep duf lazydocker
```

Natürlich geht das auch mit Ansible in einem Playbook:

```yaml
tasks:
    - name: Update and upgrade apt packages
      apt:
        update_cache: yes
        cache_valid_time: 3600
        upgrade: dist
    - name: Install awesome utilities
      apt:
        name:
          - htop
          - eza
          - ncdu
          - bat
          - gping
          - jq
          - yq
          - ripgrep
          - duf        
        state: present
        update_cache: yes
```

## Fazit

### Das Terminal verdient bessere Werkzeuge

Diese zehn Tools sind kein Luxus – sie sind professionelle Grundausstattung für jeden, der ernsthaft mit Linux oder macOS arbeitet. Die Zeit, die man durch effizientere Werkzeuge gewinnt, summiert sich schnell auf Stunden pro Woche. Und mal ehrlich: Wenn ein modernes Terminal-Tool kostenlos ist, in 30 Sekunden installiert werden kann und die tägliche Arbeit spürbar angenehmer macht, gibt es keinen vernünftigen Grund, beim alten Status quo zu bleiben.

Meine Empfehlung, fang mit htop, bat und ripgrep an, wenn du noch keine CLI-Tool-Erfahrung hast. Diese drei haben die steilste Lernkurve nach oben, also den sofortigen Mehrwert ohne große Einarbeitung. Der Rest kommt mit der Zeit.

Was fehlt auf deiner Liste? Hast du Tools, die für dich absolut unverzichtbar sind und hier nicht auftauchen? Schreib es in die Kommentare, ich bin gespannt, was die Community noch so im Gepäck hat.

[1]: https://picsur.kngstn.eu/i/f1b3c197-e3bd-4788-b55c-7cdfeb4d38c5.png
[2]: https://picsur.kngstn.eu/i/29b24a67-a500-4b0f-aa19-96741e36326f.png
[3]: https://picsur.kngstn.eu/i/124cbd43-21db-4776-a468-1266652a07c8.png
[4]: https://picsur.kngstn.eu/i/f90d5176-fc4e-440c-86e1-659603c8e3f8.png
[5]: https://picsur.kngstn.eu/i/0bd6e5ae-8fe3-4699-b6cf-3576b4492775.png
[6]: https://picsur.kngstn.eu/i/5cedc428-5c1d-4623-8d0d-4ff5c4996983.png
[8]: https://picsur.kngstn.eu/i/2198df55-fc52-495c-a906-1c93116eef2c.png
[10]: https://picsur.kngstn.eu/i/3c9edd86-2591-44b0-ac5a-b42842527b5b.png
