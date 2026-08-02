---
title: SSH Keys hardware-gebunden schützen
date: 2026-08-02 16:43:00 +/-0000
categories: [Tools, Linux]
tags: [ssh,linux,security,compliance] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/bc4f0a53-683b-4415-a5af-ebe3f12061da.png
---

## Warum eine Passphrase allein nicht mehr reicht

In  meinem vorherigen Artikel [SSH Config meistern](https://thomas-krampe.com/posts/SSH-Config-meistern/) ging es darum, wie eine saubere `~/.ssh/config` die tägliche Arbeit mit mehreren Kunden etwas produktiver macht. Ein Punkt fehlte dort bewusst, was eigentlich mit dem privaten Key selbst passiert, wenn ein Notebook gestohlen wird oder z.B. Malware auf `~/.ssh` zugreift. Tatsächlich ist der Hintergrund dieses Artikels keine "böse" Maleware im herkömmlichen Sinn, sondern ein unerlaubter Zugriff eines Admins über eine Verwaltungsschnittstelle, in diesem Fall Intune.

Ein lokal gespeicherter, privater Ed25519-Key mit Passphrase geschützt, ist der aktuelle Standard und das aus gutem Grund. Aber die Passphrase schützt nur die Datei auf dem Datenträger, nicht den Moment, in dem der Key tatsächlich benutzt wird. Sobald der Key im SSH-Agent liegt oder ein Angreifer die Passphrase mitliest, zum Beispiel per Keylogger, ist die Datei vollständig kompromittiert und lässt sich beliebig kopieren und nutzen. Der private Schlüssel existiert als eine Datei auf dem Datenträger und alles was als Datei existiert, kann auch exfiltriert werden.

Genau hier setzen hardware-gebundene Keys an. Der private Schlüssel wird nicht mehr auf der Festplatte gespeichert, sondern in einem Secure Element erzeugt und verlässt dieses nie. Jede Signatur erfordert eine physische Aktion, ein Touch auf den YubiKey oder eine Touch-ID-Bestätigung auf dem Mac. Ein gestohlener Laptop bringt einem Angreifer ohne den physischen Token gar nichts, und ein gestohlener YubiKey ohne PIN ebenfalls nicht.

## Zwei Wege zum hardware-gebundenen Key: FIDO2 und PIV

YubiKeys der Serie 5 unterstützen für SSH zwei grundsätzlich unterschiedliche Mechanismen.

**FIDO2 (`-sk`-Keys)** ist seit OpenSSH 8.2 (2020) direkt in `ssh-keygen` und `ssh` integriert, ohne Umweg über Smartcard-Software. Der Schlüsseltyp heißt `ed25519-sk` oder `ecdsa-sk`, das `-sk` steht für "security key". Das ist der pragmatischste Einstieg und für die meisten Anwendungsfälle die richtige Wahl.

**PIV (Personal Identity Verification)** nutzt den Smartcard-Slot des YubiKeys über PKCS#11. Das ist der schwergewichtigere Weg, historisch aus Enterprise-Zertifikatsinfrastrukturen und Windows Smartcard-Anmeldung gewachsen. Der Vorteil: Der daraus resultierende öffentliche SSH-Key ist ein ganz normaler `ecdsa-sha2-nistp256`-Key, kein `-sk`-Typ. Das bedeutet, jeder SSH-Server versteht ihn, auch ein alter mit OpenSSH 7.x. Bei FIDO2-Keys muss der Server hingegen selbst OpenSSH 8.2 oder neuer sprechen, sonst lehnt er den Schlüsseltyp beim Login ab.

Für die eigene Werkzeugkiste heißt das: FIDO2 für alles, wo man die Serverseite kontrolliert oder weiß, dass sie aktuell ist. PIV als Fallback, wenn Kunden noch auf veralteten OpenSSH-Versionen unterwegs sind und ein Wechsel des Schlüsseltyps schneller geht als ein Update der Serverfarm.

## YubiKey mit FIDO2 einrichten

Auf allen drei Plattformen ist das Grundprinzip identisch, nur die Vorbereitung unterscheidet sich.

**Linux** braucht `libfido2`, meist über den Paketmanager:

```bash
sudo apt install libfido2-1 libfido2-doc  # Debian/Ubuntu
sudo dnf install libfido2                 # Fedora/RHEL
```

**macOS** bringt seit Monterey (12.x) einen `ssh-keygen`, der FIDO2-Keys bereits nativ unterstützt. Wer eine ältere Version einsetzt oder auf Nummer sicher gehen will, installiert die Bibliothek zusätzlich über Homebrew:

```bash
brew install libfido2
```

**Windows** liefert seit Windows 10 1809 ein eingebautes OpenSSH mit, FIDO2-Unterstützung kam allerdings erst schrittweise über Windows-Updates dazu. `ssh -V` zeigt die Version. Bei einem OpenSSH unter 8.9 lohnt sich ein Blick auf die aktuellen Releases von [Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH), die `libfido2.dll` bereits mitbringen und sich parallel zur Windows-Standardinstallation nutzen lassen.

Der Key selbst wird überall mit demselben Befehl erzeugt:

```bash
ssh-keygen -t ed25519-sk -O resident -O verify-required \
  -f ~/.ssh/keys/yubikey/id_ed25519_sk -C "thomas@yubikey"
```

`-O resident` legt den Schlüssel als sogenannte "discoverable credential" direkt auf dem YubiKey ab, dazu gleich mehr. `-O verify-required` verlangt zusätzlich zum Touch eine PIN-Eingabe auf dem Gerät, nicht nur die physische Berührung. Ohne diese Option reicht ein einfacher Touch, was in gemeinsam genutzten Umgebungen (Kunden-Notebook, Konferenzraum-Rechner) zu wenig ist.

Die PIN wird einmalig auf dem YubiKey selbst gesetzt, unabhängig vom SSH-Setup:

```bash
ykman fido access change-pin
```

## Der Schlüssel lebt auf dem Token, nicht auf der Platte

Der Unterschied zwischen resident und non-resident Keys wird oft unterschätzt, ist aber im Kunden-Alltag der entscheidende Punkt.

Bei einem **non-resident Key** legt `ssh-keygen` eine lokale Datei an, die aber nicht den privaten Schlüssel selbst enthält, sondern nur einen verschlüsselten "Key Handle". Dieser Handle ist wertlos ohne den passenden YubiKey, verrät also für sich genommen nichts. Aber ohne diese lokale Datei kommt man an den Schlüssel auf einem neuen Rechner nicht mehr heran, selbst  nicht mit dem richtigen YubiKey.

Ein **resident Key** (`-O resident`) legt den Handle zusätzlich auf dem YubiKey selbst ab. Das erlaubt, den öffentlichen Key und den lokalen Stub auf jedem beliebigen Rechner neu herunterzuladen:

```bash
ssh-keygen -K
```

Das ist der eigentliche Praxisgewinn, neues Notebook, YubiKey einstecken, `ssh-keygen -K` ausführen, fertig. Kein Schlüsselmaterial muss synchronisiert, kopiert oder per Mail verschickt werden, was bei klassischen Ed25519-Keys sonst der übliche, unschöne Workaround wäre.

> **Verwechslungsgefahr auf macOS:** `ssh-add -K` bedeutet dort etwas völlig anderes, nämlich die Passphrase im macOS-Schlüsselbund speichern (mittlerweile durch `--apple-use-keychain` ersetzt). Das `-K` bei `ssh-keygen -K` zum Herunterladen resident gespeicherter FIDO2-Keys hat damit nichts zu tun, nur zufällig denselben Buchstaben.
{: .prompt-warning }

Ein YubiKey kann mehrere resident Keys gleichzeitig speichern (begrenzt durch Firmware und Platz, bei aktuellen YubiKey-5-Modellen üblicherweise deutlich über 20). Für die Praxis reicht in der Regel ein einziger persönlicher Key, eingebunden in die bestehende Kundenstruktur aus der SSH Config.

## Alternative: PIV-Slot für ältere Server-Landschaften

Wo FIDO2 serverseitig nicht akzeptiert wird, bleibt PIV. Der Schlüssel wird direkt im Smartcard-Slot 9a erzeugt und verlässt den YubiKey ebenfalls nie:

```bash
ykman piv keys generate 9a pubkey.pem
ykman piv certificates generate --subject "CN=thomas" 9a pubkey.pem
```

Für SSH wird der öffentliche Schlüssel über das passende PKCS#11-Modul ausgelesen, unter Linux typischerweise über OpenSC:

```bash
ssh-keygen -D /usr/lib/x86_64-linux-gnu/opensc-pkcs11.so
```

Unter macOS liegt das Modul je nach Installationsweg unter `/usr/local/lib/opensc-pkcs11.so` (Homebrew) oder wird über Yubicos eigenes `libykcs11` bereitgestellt. Unter Windows übernimmt der YubiKey Smart Card Minidriver die Einbindung, kombiniert mit OpenSC oder Yubicos `yubico-piv-tool`.

In die SSH Config eingebunden sieht das so aus:

```bash
Host kunde-legacy-server
    HostName 10.0.5.10
    User thomas
    PKCS11Provider /usr/lib/opensc-pkcs11.so
```

`ssh-add -s /usr/lib/opensc-pkcs11.so` lädt den Schlüssel einmalig in den Agent, danach verhält sich die Verbindung wie gewohnt, inklusive PIN-Abfrage pro Sitzung.

## macOS: Secure Enclave statt externem Token

Wer ausschließlich am Mac arbeitet und keinen zusätzlichen USB-Stick mit sich herumtragen möchte, kann die Secure Enclave nutzen, den Krypto-Coprozessor, der in jedem Mac mit Apple-Silicon-Chip oder T2-Chip steckt. OpenSSH selbst kennt die Secure Enclave nicht, hier braucht es ein zusätzliches Tool. Etabliert hat sich [Secretive](https://github.com/maxgoedjen/secretive) von Max Goedjen, Open Source und ohne Cloud-Anbindung.

Secretive erzeugt den privaten Schlüssel direkt in der Secure Enclave und stellt ihn über einen eigenen SSH-Agent-Socket bereit. Jede Signatur verlangt eine Touch-ID-Bestätigung, ganz ohne externes Gerät.

```bash
brew install --cask secretive
```

Nach der Einrichtung trägt man den Agent-Socket in die Umgebung ein, meist automatisch über die Secretive-App, alternativ manuell:

```bash
export SSH_AUTH_SOCK=~/Library/Containers/com.maxgoedjen.Secretive.SecretAgent/Data/socket.ssh
```

Zwei Einschränkungen sind wichtig, bevor man sich darauf verlässt:

Erstens unterstützt die Secure Enclave hardwareseitig nur ECDSA mit der Kurve P-256, kein Ed25519. Das ist kein Sicherheitsproblem, P-256 gilt weiterhin als solide, aber es widerspricht der sonst üblichen Empfehlung, durchgängig auf Ed25519 zu setzen. Wer sich fragt, warum ECC überhaupt die richtige Wahl ist und wie sich das Feld angesichts künftiger Quantencomputer entwickelt, findet Hintergründe in [Quanten-ready und performance-stark](https://thomas-krampe.com/posts/quanten-ready-und-performance-stark/).

Zweitens ist der Schlüssel unwiderruflich an genau dieses eine Gerät gebunden. Es gibt keinen Export, keine Synchronisation, kein Backup im klassischen Sinn. Geht der Mac verloren oder wird er zurückgesetzt, ist der Schlüssel weg, endgültig. Das ist beim YubiKey anders lösbar, siehe Backup-Strategie weiter unten.

Als Alternative mit anderem Vertrauensmodell nutzen manche auch den SSH-Agent von 1Password 8: Schlüssel liegen dort verschlüsselt im 1Password-Vault, freigeschaltet per Touch ID, aber geräteübergreifend synchronisiert. Wer die Ein-Geräte-Bindung der Secure Enclave als Nachteil empfindet, bekommt darüber Portabilität zurück, allerdings auf Kosten des reinen Hardware-Only-Modells.

## Einbindung in die bestehende Kundenstruktur

Die per-Kunde-Struktur aus dem vorherigen Artikel bleibt unverändert, nur die `IdentityFile`-Zeile würde jetzt auf den `-sk`-Key statt auf einen klassischen Ed25519-Key zeigen.

```bash
# ~/.ssh/config.d/kunde-a

Host kunde-a-*
    User thomas
    IdentityFile ~/.ssh/keys/yubikey/id_ed25519_sk
    IdentitiesOnly yes
    ProxyJump kunde-a-bastion
```

`IdentitiesOnly yes` funktioniert mit `-sk`-Keys genauso wie mit klassischen Keys. Beim Verbindungsaufbau fordert SSH dann einen Touch auf dem YubiKey an, sichtbar an der blinkenden LED, bevor die Verbindung zustande kommt.

## Serverseitige Kompatibilität prüfen

Bevor der gesamte Kundenbestand auf FIDO2-Keys umgestellt wird, lohnt sich ein Blick auf die tatsächliche OpenSSH-Version je Server:

```bash
ssh -v kunde-a-prod 2>&1 | grep "remote software"
```

Alles ab OpenSSH 8.2 akzeptiert `sk-ssh-ed25519@openssh.com` in `authorized_keys` ohne weitere Anpassung. Ältere Versionen brechen die Authentifizierung mit einer Meldung ab, die auf den ersten Blick wie ein falscher Schlüssel aussieht, tatsächlich aber ein nicht unterstützter Schlüsseltyp ist. Genau für diesen Fall bleibt PIV die robustere Wahl, weil der resultierende Schlüsseltyp serverseitig wie ein gewöhnlicher ECDSA-Key erscheint.

## Backup-Strategie: Was passiert, wenn der Token weg ist

Ein hardwaregebundener Key ohne Backup ist ein Single Point of Failure, der sich schlecht anfühlt, sobald der YubiKey tatsächlich verloren geht oder kaputt ist. Die gängige Praxis:

- Zwei YubiKeys anschaffen, beide mit demselben Resident-Key-Setup einrichten (jeweils eigenständig erzeugt, da sich private Schlüssel zwischen zwei YubiKeys nicht kopieren lassen), den zweiten öffentlichen Schlüssel ebenfalls in `authorized_keys` aller Server hinterlegen.
- Den zweiten YubiKey physisch getrennt vom ersten aufbewahren, nicht im selben Rucksack.
- Bei PIV-Setups gilt dasselbe Prinzip, ein zweites Zertifikat auf einem zweiten Token.
- Bei der Secure Enclave gibt es keine Geräte-übergreifende Backup-Option. Wer sie nutzt, sollte parallel einen YubiKey-Key als Fallback in `authorized_keys` hinterlegen, für den Fall, dass der Mac nicht verfügbar ist.

## Was ich empfehle

Für den eigenen Kunden-Alltag mit vielen unterschiedlichen Zielsystemen ist ein FIDO2-Resident-Key auf zwei YubiKeys der pragmatischste Ausgangspunkt, kombiniert mit der Kundenstruktur aus der bestehenden SSH Config. PIV bleibt in der Hinterhand für einzelne Kunden mit veralteter Serverlandschaft. Die Secure Enclave über Secretive eignet sich gut als komfortable Ergänzung auf dem eigenen MacBook, etwa für den Zugriff auf die eigene Infrastruktur, aber nicht als alleinige Lösung für Systeme, auf die von unterschiedlichen Geräten aus zugegriffen werden muss.

## Fazit

Eine Passphrase schützt eine Datei. Ein hardware-gebundener Key schützt den Vorgang selbst, weil der private Schlüssel nie in einer Form vorliegt, die sich kopieren lässt. Für den Aufwand, einmalig einen YubiKey einzurichten oder Secretive auf dem Mac zu installieren, ist der Sicherheitsgewinn erheblich, gerade wer wie ich regelmäßig auf fremde Kundennetze zugreift. Der nächste gestohlene Laptop bleibt dann ein Ärgernis, aber keine Kompromittierung der Kundenumgebung mehr.
