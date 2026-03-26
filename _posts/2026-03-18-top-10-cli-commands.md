---
title: Top 10 CLI-Kommandos für Linux und macOS
date: 2026-03-18 06:55:00 +/-0000
categories: [macOS, Linux, Tools]
tags: [homebrew, ubuntu, cli, how-to] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/d954a7a9-418d-44d4-9123-1b6d8f0fd556.png
---

Nachdem ich bereits meine [10 besten CLI Tools](https://thomas-krampe.com/posts/top-10-cli-tools/) vorgestellt habe, möchte ich nicht den Eindruck erwecken, dass alle Kommandos die im OS bereits enthalten sind, nichts taugen.

Stellen wir uns doch kurz vor, wir sitzen vor dem Terminal, jonglieren mit Verzeichnissen, Logs und Skripten und greifen doch immer wieder zu den üblichen Verdächtigen wie `ls`, `cd` oder `grep` und denken uns, da hat doch so ein Typ einen Artikel geschrieben, was ich alles noch installieren muss. Dabei stecken in Linux und macOS auch ohne meinen Artikel bereits viele unterschätzte Kommandos, die bereits vorinstalliert sind und uns das Leben massiv erleichtern können. Diese 10 Kommandos werden deshalb unterschätzt, weil sie nicht sehr bekannt sind, aber in der Praxis wirklich Zeit sparen. Zeit also, ides mal kennenzulernen.

> **Wichtig:** Alles läuft "out-of-the-box" auf macOS und gängigen Linux-Distributionen (Ubuntu, Fedora etc.), keine Paketmanager nötig (ok, mit einer kleinen Ausnahme, aber dazu mehr bei Nummer 4).
{: .prompt-info }

## 1. Darf ich vorstellen, `sudo !!` – Das "Mist, sudo vergessen!" Kommando

Habt ihr schon mal z.B. ein langes Kommando ohne sudo ausgeführt und musstet danach alles noch mal tippen oder erst umständlich den Cursor bewegen? `sudo !!` wiederholt den letzten Befehl einfach mit `sudo` davor.

**Beispiel:**  

```bash
$ apt update
# Fehlermeldung: Permission denied
$ sudo !!
sudo apt update
```

Spart mit Sicherheit 5 Sekunden pro Tag, die sich summieren. Funktioniert in Bash und Zsh.

## 2. Das Dream-Team `pushd` und `popd` – Verzeichnis-Stack für Multitasking

Vergesst `cd` Marathons zwischen Projekten. `pushd` legt Verzeichnisse auf einen Stack, `popd` holt sie ab. `dirs -v` zeigt den Stack an.  

**Beispiel:**  

```bash
$ pushd ~/projekte/app1
~/projekte/app1 ~
$ pushd ~/projekte/app2
~/projekte/app2 ~/projekte/app1 ~
$ popd
~/projekte/app1 ~
```

**Pro-Tipp:**

`pushd +2` rotiert den Stack ohne zu wechseln, ideal für parallele Branches.

## 3. `script` – Terminal-Session als Datei aufnehmen

Macht ihr vielleicht ein How-to-Video oder eine Dokumentation? `script` zeichnet eure gesamte Session (Eingaben + Ausgaben) in eine Datei auf.  

**Beispiel:**  

```bash
$ script session.log
Script started, file is session.log
$ ls -la; echo "Test"
[...]
$ exit
Script done, file is session.log
```

Perfekt für Support-Tickets oder Blog-Demos, inklusive Timing und Farben.

## 4. `watch` – Live-Überwachung von Befehlen

Statt `while true; do ...` einzutippen, `watch` führt einen Befehl zyklisch aus und aktualisiert den Screen.  

**Beispiel:**  

```bash
watch -n 2 df -h
```

Zeigt die Festplatten-Nutzung alle 2 Sekunden. `-d` hebt dabei Änderungen hervor, super für Log-Tail oder Prozess-Monitoring.

> **Tip:** Auf macOS kann es tatsächlich bei der Standard Variante zu Fehlern kommen. Besser hier die GNU Variante per Homebrew nachinstallieren.
{: .prompt-tip }

```bash
brew install watch
```

## 5. `fold` – Text umbrechen ohne Formatverlust

Lange Logs oder Configs sind unlesbar? `fold` bricht Zeilen bei festgelegter Breite um, ohne Wörter zu zerhacken.  

**Beispiel:**  

```bash
fold -w 40 /var/log/syslog | less
```

Pipes in Editoren oder für präzise PDF-Exports. Besser als `fmt`, weil es Leerzeichen respektiert.

## 6. `tee` – Ausgabe in Datei UND Terminal

`cat datei | grep fehler > log.txt` – aber es ist nichts zu sehen? `tee` teilt den Stream: Ausgabe + Datei.  

**Beispiel:**  

```bash
ps aux | grep python | tee prozesse.log
```

Ideal für CI/CD-Logs oder Live-Debugging.

## 7. `xargs` – Massenverarbeitung aus Pipes

`find . -name "*.log" | rm` scheitert bei vielen Dateien? `xargs` baut Argumentlisten.  

**Beispiel:**  

```bash
find . -name "*.tmp" | xargs rm
```

**Powertip:**

`-I {}` für komplexe Loops, z.B. `find ... | xargs -I {} mv {} backup/`.

## 8. `alias` – Permanente Shortcuts (interaktiv)

Nicht persistent, aber sofort: `alias ll='ls -la'` im Terminal. Für Sessions mit viel Tipparbeit.  

**Beispiel:**  

```bash
alias gs='git status'
gs
```

>**Tipp:** In die Datei `.zshrc` oder `.bashrc` geschrieben, spart das Tastenanschläge und macht das permanent (nach einem `source .zshrc`).
{: .prompt-tip }

## 9. `envsubst` – Variablen in Textdateien ersetzen

Config-Templates? `envsubst` ersetzt `${VAR}` durch Umgebungsvariablen, ganz ohne sed/perl.  

**Beispiel:**  

```bash
export DB_HOST=localhost
cat config.tpl | envsubst > config.prod
```

Gerade für Docker-Compose oder Ansible, sauberer als String-Replace.

## 10. `column` – Tabellen aus Flachdateien zaubern

CSV oder Space-separierte Logs? `column -t` formatiert zu sauberen Tabellen.  

**Beispiel:**  

```bash
cat prozesse.txt | column -t -s $'\t'
PID   USER  CPU%
1234  root  15.2
5678  user  2.1
```

Macht `ps`-Ausgaben lesbar, auch ohne Excel.

## Meine Top-Empfehlung

Beginnt mit `pushd/popd` und `watch`, die ändern euren Workflow sofort. Mit diesen 10 CLI-Kommandos spare ich in Projekten Stunden.

## Bonustip

Für jedes Kommando könnt ihr euch natürlich die entsprechende `man` Page mit `man <command>` anschauen. Aber ehrlich, die sind teilweise ziemlich lang und unübersichtlich. Hier könnt ihr aber einfach `tldr` (steht für too long; didn't read) nachinstallieren. Ihr bekommt dann nur eine kurze Beschreibungungen und Beispiele statt langer und unübersichtlicher man-pages. Einfach `tldr <command>` statt `man <command>`verwenden.

**Ubuntu**

```bash
sudo apt install tldr
# bei Problemen mit apt geht auch snap 
sudo snap install tldr
# oder pip
pip3 install tldr

# Zum Schluss noch den Offline-Cache herunterladen, dann geht es auch ohne Internet
tldr --update
```

**macOS (Apple Silicon/Intel)**

```bash
brew install tldr
tldr --update
```

Habt ihr Favoriten die ich hier vielleicht vergessenen habe? Teilt mir eure bevorzugten Kommandos in den Kommentaren mit.
