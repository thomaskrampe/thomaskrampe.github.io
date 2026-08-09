---
title: SSH-Authentifizierung über OIDC mit opkssh
date: 2026-08-09 10:42:00 +/-0000
categories: [Tools, Linux]
tags: [ssh,linux,security,compliance] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/c8e347f4-51eb-4bfd-9c2b-a8d08aada834.png
---

## Wenn der Identity Provider die SSH-Tür bewacht

In meinem ersten Artikel [SSH Config meistern](https://thomas-krampe.com/posts/SSH-Config-meistern/) ging es um die Struktur, im zweiten [SSH Keys mit YubiKey und Secure Enclave schützen](https://thomas-krampe.com/posts/ssh-keys-yubikey-secure-enclave/) darum, den privaten Schlüssel selbst unangreifbar zu machen. Beide Artikel befinden sich auf der Seite des Anwenders und lösen allerdings ein wesentliches Problem nicht. Der Schlüssel bleibt trotzdem dauerhaft gültig und ist immer ein Unsicherheitsfaktor für sich, wenn er nicht widerrufen wird. Ein YubiKey mit Touch-ID-Freigabe ist kaum zu stehlen, aber wenn ein Mitarbeiter das Unternehmen verlässt, muss trotzdem jemand daran denken, seinen öffentlichen Schlüssel aus jeder einzelnen `authorized_keys`-Datei zu entfernen. Auf zehn Kundensystemen ist das eine Fleißaufgabe, auf hundert ein massives Sicherheitsrisiko.

> Ein naheliegender Zwischenschritt, bevor wir in das Thema tiefer einsteigen, ist natürlich die `authorized_keys`-Dateien zentral in einem Repository zu pflegen und automatisiert per Ansible, Puppet oder eine CI-Pipeline auf die Zielsysteme zu verteilen. Viele haben das bereits genauso umgesetzt. Ein zu entfernender Key ist damit an nur einer Stelle zu löschen statt auf zehn oder hundert Servern einzeln, das entschärft das Revocation-Problem spürbar. Dieser Zwischenschritt würde aber keines der Probleme lösen, die ich mit diesem Artikel behandeln möchte.
{: .prompt-tip }

Genau hier setzt eine andere Idee an, SSH-Zugriffe nicht mehr über einen eigenen, lokal verwalteten Schlüssel zu regeln, sondern über die Identität, die die meisten ohnehin schon zentral im Unternehmen pflegen. Das Werkzeug dafür heißt [opkssh](https://github.com/openpubkey/opkssh) und baut auf einem Protokoll namens OpenPubkey auf.

## Was opkssh eigentlich macht

Die Grundidee ist, den klassischen SSH-Schlüssel durch ein OIDC-Token zu ersetzen. Also durch dieselbe Art von Identitätsnachweis, mit dem man sich heute schon bei Microsoft Entra ID, Google Workspace oder Okta anmeldet. OpenPubkey erweitert ein normales OIDC-Login um einen Zusatzschritt, der Client erzeugt lokal ein kurzlebiges Schlüsselpaar und sendet einen Hash dieses öffentlichen Schlüssels als nonce-Parameter an den Identity Provider. Der Provider signiert daraufhin das ID-Token, inklusive dieses Hashes.

Der Client kombiniert das vom IdP signierte Token mit dem öffentlichen Schlüssel und einer eigenen Signatur zum sogenannten PK-Token. Der Identity Provider bürgt damit indirekt kryptografisch dafür, dass genau dieser öffentliche Schlüssel zu der angemeldeten Identität gehört, ganz ohne dass der IdP selbst umgebaut werden muss oder von OpenPubkey wissen muss.

Der SSH-Server muss diesem Schlüssel nicht mehr selbst vertrauen, sondern nur noch dem Identity Provider, der das Token signiert hat. Es gibt also keinen zweiten, parallelen Vertrauensstrang mehr, der separat gepflegt werden muss.

## Auch mit selbstgehosteten Providern

opkssh ist nicht auf einen bestimmten Anbieter festgelegt. Auf der offiziellen Kompatibilitätsliste stehen unter anderem Google, Microsoft Entra ID, GitLab, Keycloak, Zitadel, Kanidm und, für den eigenen Betrieb besonders relevant, auch Authentik. Für andere selbstgehostete Provider ohne fertiges Kurzformat, reicht die generische Anmeldung mit Issuer und Client-ID:

```bash
opkssh login --provider="https://auth.example.com/application/o/ssh/,<client-id>"
```

Zwei Dinge sollten dabei trotzdem geprüft werden. Erstens muss der Provider den von OpenPubkey gesetzten Nonce, den Hash des Ephemeral-Public-Keys, unverändert in das signierte ID-Token übernehmen, das ist das Standardverhalten bei einer sauberen OIDC-Implementierung. Zweitens muss der SSH-Server den JWKS-Endpoint des Providers erreichen können, um dessen öffentlichen Signaturschlüssel abzurufen, das heißt der Identity Provider muss aus dem Server-Netz heraus erreichbar sein, nicht nur aus dem Netz der Clients.

Für den eigenen Betrieb mit einer selbstgehosteten Identity-Plattform statt einem Cloud-Anbieter ändert das nichts an der grundsätzlichen Architektur, nur der Vertrauensanker liegt dann eben im eigenen Rechenzentrum statt bei Microsoft oder Google.

## Der Ablauf beim Login

Auf dem Client sieht der Login aus wie ein ganz normaler Browser-basierter OIDC-Flow:

```bash
opkssh login
```

Der Befehl öffnet den Standard-Browser, der Nutzer meldet sich beim Identity Provider an, genau wie bei jedem anderen Cloud-Dienst auch. Danach liegt lokal ein kurzlebiges Schlüsselpaar plus PK-Token unter ~/.ssh/id_ecdsa. Wie lange dieses Login gültig ist, hängt vom Identity Provider ab. Standard-OIDC-Tokens laufen meist nach 1 Stunde ab. Erst nach Ablauf dieses Fensters muss sich der Anwender erneut über opkssh login authentifizieren. Innerhalb dieses Zeitfensters ändert sich der eigentliche SSH-Aufruf für den Anwender nicht:

```bash
ssh kunde-a-prod
```

Auf dem Server läuft kein klassischer Abgleich gegen `authorized_keys` mehr, sondern ein `AuthorizedKeysCommand`, das opkssh in `sshd_config` registriert:

```bash
# /etc/ssh/sshd_config
AuthorizedKeysCommand /usr/local/bin/opkssh verify %u %k %t
AuthorizedKeysCommandUser opksshuser
```

Dieses Kommando prüft das PK-Token gegen die Signatur des Identity Providers, gleicht die enthaltene Identität (typischerweise die E-Mail-Adresse) gegen eine lokale Policy ab und gibt bei einem Treffer den temporären öffentlichen Schlüssel zurück, den SSH dann wie einen ganz normalen `authorized_keys`-Eintrag behandelt. Läuft das Zeitfenster ab, ist der Schlüssel automatisch ungültig, ganz ohne manuellen Widerruf.

Der gesamte Ablauf von der lokalen Schlüsselgenerierung bis zur serverseitigen Prüfung noch einmal im Überblick, hier am Beispiel Microsoft Entra ID als Identity Provider:

![opkssh_diagram](https://picsur.kngstn.eu/i/057b487e-3059-4d94-9f0e-ba7c4953da6e.png)

## Policy statt verteilter Schlüsseldateien

Die Zuordnung, wer sich als welcher Unix-User einloggen darf, liegt zentral in einer Policy-Datei auf dem Server, `/etc/opk/auth_id`. Angelegt wird ein Eintrag über einen eigenen Befehl, nicht durch manuelles Editieren:

```bash
sudo opkssh add thomas thomas@domain.org azure
sudo opkssh add support support-team@domain.org azure
```

Das Ergebnis ist eine space-separierte Datei mit drei Spalten, Unix-Principal, Identität und Issuer (`azure` ist dabei nur das Kurzformat für die lange Entra-ID-Issuer-URL):

```bash
# /etc/opk/auth_id
# principal   email                       issuer
thomas        thomas@domain.org           azure
support       support-team@domain.org     azure
```

Der entscheidende Unterschied zur bisherigen Struktur ist, das diese Datei keine Schlüssel mehr enthält, nur noch Identitäten. Verlässt ein Mitarbeiter das Unternehmen, reicht die Deaktivierung seines Kontos beim Identity Provider. Jedes System, das über opkssh angebunden ist, verliert diesen Zugriff spätestens nach Ablauf der Token-Gültigkeit, ohne dass jemand eine einzige `authorized_keys`-Datei manuell anfassen muss. Das löst genau das Problem, das ein noch so gut gesicherter YubiKey nicht lösen kann, die zentrale Instanz für den Widerruf.

## Der eigentliche Mehrwert liegt aber vor der SSH-Verbindung

Der Wechsel auf OIDC ist an sich schon ein Fortschritt bei der Verwaltung, aber der weitaus größere Gewinn entsteht durch das, was sich bei den meisten Identity Provider noch zusätzlich konfigurieren lässt, bevor überhaupt ein Token ausgestellt wird.

**Risk-basierte Zugriffsrichtlinien.** Ein moderner Identity Provider bewertet jeden Anmeldeversuch anhand von Werten wie Standort, Gerätezustand oder ungewöhnlichem Verhalten. Eine Anmeldung aus einem bislang unbekannten Land oder über ein anonymisiertes VPN kann automatisch eine zusätzliche Bestätigung verlangen oder komplett blockiert werden, noch bevor ein PK-Token überhaupt ausgestellt wird. Diese Logik existiert unabhängig von SSH bereits, opkssh bringt SSH lediglich unter dasselbe Dach.

**Login nur vom Managed Device.** Über Geräte-Compliance-Richtlinien lässt sich festlegen, dass ein Token nur ausgestellt wird, wenn das anfragende Gerät vom Unternehmen verwaltet wird, aktuelle Patches hat und Festplattenverschlüsselung aktiv ist. Ein privater Laptop, ein Kundenrechner oder ein kompromittiertes Gerät ohne diese Nachweise bekommt gar keine Chance, überhaupt ein gültiges Token zu erzeugen. Das ist eine Kontrolle, die auf Ebene des einzelnen SSH-Servers praktisch nicht abbildbar wäre, bei den meisten Identity Provider aber bereits existiert.

**Passkeys statt Passphrase.** Die Anmeldung beim Identity Provider selbst lässt sich auf Passkeys umstellen, also biometrische Freigabe per Touch ID, Windows Hello oder einem FIDO2-Sicherheitsschlüssel. Aus Anwendersicht ist der SSH-Login damit am Ende genauso ein Fingerabdruck-Moment wie beim YubiKey-Ansatz aus dem letzten Artikel, nur dass die biometrische Prüfung jetzt zentral beim Identity Provider passiert und automatisch für jeden weiteren Dienst gilt, der an denselben Provider angebunden ist. Ein einziger Passkey ersetzt damit potenziell ein ganzes Bündel dienstspezifischer Zugangsmittel.

In Summe verschiebt sich die eigentliche Sicherheitsarbeit von "welcher Schlüssel liegt wo" zu "wie vertrauenswürdig ist diese spezifische Anmeldung gerade", eine Frage, die eine zentrale Identitätsplattform naturgemäß besser beantworten kann als eine verteilte Sammlung von `authorized_keys`-Dateien.

## Wo opkssh an Grenzen stößt

So praktisch das Modell auch ist und so gut es sich in diesem Artikel liest, es passt leider nicht für jedes Szenario.

Ohne Verbindung zum Identity Provider gibt es kein neues Token. Für Systeme in einem abgeschotteten Netz ohne Internetzugang oder für den seltenen Fall, dass der Identity Provider selbst nicht erreichbar ist, braucht es einen Notfallzugang, einen klassischen, hardware-gebundenen Break-Glass-Key. Dieser wird am besten für einen dedizierten Notfall-User in dessen lokaler authorized_keys hinterlegt, damit der Zugriff auch dann funktioniert, wenn die Anbindung an den IdP oder das AuthorizedKeysCommand fehlschlägt.

Die kurze Gültigkeit der Tokens ist beim interaktiven Login ein Vorteil, bei lang laufenden, unbeaufsichtigten Prozessen wie Ansible-Läufen oder Cronjobs dagegen eine zusätzliche Komplexität. Für Automatisierung bleiben Service-Accounts mit eigenen, separat verwalteten Zugangsdaten oft die pragmatischere Wahl, opkssh zielt in erster Linie auf den menschlichen, interaktiven Zugriff.

Und schließlich verschiebt sich das Vertrauen vollständig auf den Identity Provider. Das ist beabsichtigt, denn genau diese Bündelung ist der Vorteil, es bedeutet aber auch, dass der Identity Provider selbst konsequent abgesichert sein muss, inklusive der eigenen Konditional-Access-Konfiguration. Ein schwach konfigurierter Identity Provider wird damit zum Single Point of Failure für sämtliche daran angebundene Infrastruktur, nicht nur für SSH.

## Einordnung in die bestehende Kundenstruktur

Für den eigenen Alltag mit vielen unterschiedlichen Kundenumgebungen bedeutet das nicht, die per-Kunde-Struktur aus der SSH Config meines ersten Artikels zu verwerfen. Bastion Hosts, `ProxyJump`-Ketten und `Include`-Dateien bleiben bestehen, opkssh ersetzt lediglich, wie der einzelne Host den vorgelegten Schlüssel prüft. 

Für Kunden mit eigenem, zentralem Identity Provider und einer bestehenden Conditional-Access-Struktur ist opkssh ein naheliegender nächster Schritt. Für kleinere Umgebungen ohne einen solchen Unterbau, oder für einzelne Systeme, die man ohnehin nur selbst administriert, bleibt der hardware-gebundene YubiKey- oder Secure-Enclave-Ansatz aus dem [vorherigen Artikel](https://thomas-krampe.com/posts/ssh-keys-yubikey-secure-enclave/) die einfachere und ausreichend robuste Lösung. 

Aber Achtung bei Bastion-Hosts, die in den meisten seriösen Setups bewusst so gehärtet sind, dass keinerlei ausgehende Verbindungen ins Internet aufgebaut werden dürfen. Egress-Filterung auf Bastion Hosts ist z. B. auch oft eine explizite Auditor- oder Compliance-Anforderung (ISO 27001, BSI-Grundschutz-Bausteine für Jump-Server). Genau diese Grundannahme bricht opkssh aber auf, sobald der IdP ein Cloud-Dienst ist (Entra ID, Google, Okta). Der Bastion Host braucht jetzt zwingend ausgehenden HTTPS-Zugriff auf den JWKS-Endpoint des Providers, sonst schlägt jeder Login fehl. Das entschärft sich etwas bei selbstgehosteten IdPs, da hier nur interner Netzwerkverkehr nötig ist, muss aber zwingend in die Architekturbetrachtung einfließen.

## Was ich empfehle

Wo bereits ein zentraler Identity Provider mit Risk Policies und Geräte-Compliance existiert, würde ich opkssh für den administrativen SSH-Zugriff auf Kundeninfrastruktur ernsthaft prüfen, gerade weil der Widerruf beim Mitarbeiterausscheiden dann nur noch eine einzige zentrale Aktion ist statt einer Liste von Systemen, die manuell abgearbeitet werden muss. Ein hardwaregebundener Break-Glass-Key bleibt dabei Pflicht, für den Moment, in dem der Identity Provider selbst nicht erreichbar ist. Aber auch hier gilt: Ein Notfallzugang, der die ganze OIDC-Kette umgeht, ist selbst ein Risiko. Monitoring und Alerting bei jeder Nutzung dieses Keys sowie eine sichere Aufbewahrung sind daher Pflicht.

Ein YubiKey macht den Schlüssel selbst unangreifbar, opkssh macht den Schlüssel überflüssig, indem er ihn durch eine zeitlich begrenzte, zentral geprüfte Identität ersetzt. Beide Ansätze schließen sich nicht aus, im Gegenteil, der Break-Glass-Zugang für den Ausfall des Identity Providers sollte ohnehin hardwaregebunden sein. Was sich ändert, ist die Frage, wer am Ende für den Zugriff verantwortlich ist.
