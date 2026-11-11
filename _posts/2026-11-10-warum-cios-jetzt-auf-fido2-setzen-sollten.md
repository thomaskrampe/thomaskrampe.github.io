---
title: Warum CIOs jetzt auf FIDO2 setzen sollten
date: 2026-11-10 12:30:00 +/-0000
categories: [Strategie, Sicherheit]
tags: [microsoft, m365, fido2, mfa,entraid] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/fb2d0e45-e36c-490f-a80a-b3fffa25bf08.jpg
---

Am 1. Februar 2027 stellt Microsoft SMS- und Sprachcodes für die meisten Nutzer in Entra ID ein. Wer dann nur noch ein Telefon als zweiten Faktor hat, muss bei der Anmeldung einen unterstützten Passkey registrieren, sonst geht es nicht mehr weiter. Drei Gründe sprechen dafür, jetzt auf FIDO2-Sicherheitsschlüssel umzustellen: die Frist, die Phishing-Anfälligkeit von SMS und Push sowie die Erwartungen aus NIS2 und von Cyber-Versicherern.

Für Admin-Zugriffe auf die Microsoft-Portale verlangt Microsoft in Entra ID schon heute MFA (Multi-Faktor-Authentifizierung). Wer die Umstellung selbst plant, statt sie von einem Anmeldedialog erzwingen zu lassen, behält die Kontrolle über Zeitpunkt und Methode.

## Was bei Microsoft schon gilt und was noch kommt

Zwei Entwicklungen werden gern vermischt. Die MFA-Pflicht für Admin-Zugriffe auf Microsoft-Umgebungen ist weitgehend abgeschlossen. Die Abschaltung der Microsoft-eigenen SMS- und Voice-Codes betrifft dagegen alle Nutzer und beginnt gerade erst.

### MFA-Pflicht für Admin-Portale

Seit Oktober 2024 verlangt Microsoft MFA bei der Anmeldung am Azure-Portal, am Microsoft Entra Admin Center und am Intune Admin Center, dort auch für lesende Zugriffe. Das Microsoft 365 Admin Center folgte ab Februar 2025. Die Phase 2 gilt seit dem 1. Oktober 2025 für Azure CLI, Azure PowerShell, die Azure-App, IaC-Werkzeuge und die REST-API, dort derzeit aber nur für Create-, Update- und Delete-Operationen. Ein rein lesender Zugriff bleibt bisher auch ohne MFA möglich. Tenants mit komplexen Umgebungen konnten Phase 2 bis zum 1. Juli 2026 aufschieben, diese Frist ist verstrichen.

Ausnahmen gibt es nicht mehr, auch Notfallkonten (Break Glass Accounts) müssen MFA nutzen. Nur Workload-Identitäten wie Managed Identities und Service Principals sind bisher nicht betroffen. Benutzerkonten, die als Dienstkonto für Skripte dienen, sind es aber sehr wohl.

### Abschaltung der Microsoft-eigenen SMS- und Voice-Codes

Microsoft stuft SMS und Voice nicht mehr als sichere Verfahren ein und macht Passkeys zum neuen Standard. Der Zeitplan sieht dabei wie folgt aus:

- **1\. September 2026:** Seit diesem Tag sind Benutzer, die für SMS oder Voice aktiviert sind, automatisch für Passkeys freigeschaltet. Die Registrierungskampagne steht auf „Microsoft Managed" und fordert diese Nutzer nach der nächsten MFA-Anmeldung zur Passkey-Registrierung auf. Der Hinweis lässt sich standardmäßig unbegrenzt verschieben.
- **1\. Februar 2027:** Microsoft stellt SMS und Voice für alle Nutzer, außer Global Administratoren und externen Nutzern, ein. Das gilt auch für die Passwortrücksetzung per Self-Service (SSPR).
- **Nach dem 1. Februar 2027:** Wer nur noch SMS oder Voice als MFA-Methode hat, muss bei der Anmeldung einen Passkey registrieren, bevor er weiterarbeiten kann. Dafür gibt es kein Opt-out. Ein Konto wird dabei nicht gesperrt, ohne Passkey-Registrierung geht die Anmeldung aber nicht weiter. Wer bereits TOTP registriert hat, bekommt die Aufforderung nicht.
- **1\. Juli 2027:** Dieselbe Regel gilt für Global Administratoren (solange die Rolle aktiv ist) und externe Nutzer.

Wer SMS oder Voice aus regulatorischen oder betrieblichen Gründen behalten muss, bindet einen Telefonie-Anbieter über den Microsoft Security Store an. Genannt werden Soprano und Telesign, die Anbieter-FAQ führt zusätzlich Infobip auf. Die Konfiguration ist seit dem 30. Oktober 2026 möglich, bei der Einstufung (Private Preview oder allgemein verfügbar) widersprechen sich die Microsoft-Seiten. Die Kosten richten sich nach Anbieter, Region und Nutzung, hinzu kommen Azure-Kosten für die Integration in der eigenen Subscription. Der Wechsel auf Passkeys kostet laut Microsoft nichts extra. Authenticator-Codes (TOTP) bleiben nutzbar, erfüllen aber keine Richtlinie, die phishingsichere MFA verlangt.

Wer mehr Zeit braucht, kann den gesamten Tenant bis zum 1. Februar 2027 per Opt-out von der automatischen Passkey-Aktivierung und der Registrierungskampagne ausnehmen (über Microsoft Graph, `beta`-Endpunkt). Die Abschaltung selbst lässt sich nicht aufschieben. Der Zeitplan gilt zudem nur für die öffentliche Microsoft Cloud.

Bis Februar 2027 ist genug Zeit für einen Pilot und einen gestaffelten Rollout, aber nicht für Abwarten.

## Bedrohungslage 2026 und warum SMS und Push-App nicht mehr ausreichen

Viele Unternehmen halten SMS-Codes oder Push-MFA auf dem Smartphone für ausreichend. Für Angreifer sind beide Verfahren kein Hindernis mehr. Microsoft nennt SMS und Voice in seinen FAQ selbst „among the most vulnerable authentication methods", also eines der anfälligsten Verfahren.

- Adversary-in-the-Middle (AiTM) Phishing: Angreifer schalten eine gefälschte Anmeldeseite zwischen Nutzer und Entra ID. Gibt der Nutzer Passwort und SMS-Code ein, greift der Angreifer das Session-Token ab und umgeht die MFA.
- MFA-Bombing und Push-Fatigue: Skripte überfluten Mitarbeiter mit Push-Anfragen, bis jemand genervt oder versehentlich bestätigt.
- SIM-Swapping: SMS wurden nie für Sicherheitsanwendungen konzipiert. Das Umleiten von Nachrichten über das Mobilfunknetz ist ein bekannter Angriffsweg.

Auch die eigene Authenticator-App mit Push-Anmeldung zählt in Entra ID nicht zu den phishing-resistenten Multi-Faktor-Authentifizierungen. Dazu gehören laut Microsoft derzeit FIDO2-Sicherheitsschlüssel, Windows Hello for Business (bzw. Plattform-Anmeldeinformationen) sowie zertifikatsbasierte Anmeldungen.

Schulungen allein lösen das Problem nicht. In einer auf der Black Hat 2025 vorgestellten Studie mit rund 19.500 Beschäftigten von UC San Diego Health senkte eingebettetes Phishing-Training die Fehlerquote im Schnitt nur um 1,7 Prozent gegenüber der Kontrollgruppe. Die Abwehr muss deshalb technisch auf Protokollebene erzwungen werden, eben durch phishing-resistente Authentifizierung.

## FIDO2 und Hardware-Token

FIDO2/WebAuthn bindet die Anmeldung an die tatsächliche Domain des Dienstes. Eine gefälschte Webseite kann einen FIDO2-Schlüssel nicht täuschen, der Login wird verweigert. In der Authentication Methods Policy heißt die Methode „Passkey (FIDO2)“, in der Methodenliste eines Nutzers erscheint sie als „Passkey (Device bound)“ oder „Passkey (Synced)“. Microsoft unterscheidet also synchronisierte von gerätegebundenen Passkeys. Die FIDO2-Hardware-Keys gehören zu den gerätegebundenen Passkeys.

![EntraID_authentifizierung](https://picsur.kngstn.eu/i/26707d7e-e12f-4c6c-8bae-6e2263406b5c.png)

*Registrierte Passkeys in Entra ID: ein gerätegebundener Passkey (iShield Key) und ein synchronisierter Passkey (iCloud Keychain).*

Passkeys auf dem Smartphone sind in vielen Szenarien nützlich, stoßen in vielen Unternehmen aber schnell an ihre Grenzen:

- Nicht alle Mitarbeiter haben ein Diensthandy (BYOD und Datenschutz)
- In sensiblen Bereichen wie Produktion, Reinräumen oder Hochsicherheitszonen sind Smartphones verboten
- Private Keychains sind aufwendig zu verwalten

### Worauf es bei Hardware-Keys ankommt

Für europäische Unternehmen und KRITIS-Betreiber zählt bei Sicherheits-Hardware die Lieferkette. Bei der Auswahl helfen vier Fragen:

- Wie ist der Schlüssel geschützt? Entscheidend ist ein zertifiziertes Secure Element, in dem der private Schlüssel gegen Auslesen gesichert ist.
- Wer entwickelt und fertigt die Hardware, und vor allem, wo?
- Steht das Modell auf Microsofts Liste der FIDO2-Keys, die Entra ID per Attestation prüfen kann?
- Passen Bauform und Schnittstellen (USB-A, USB-C, NFC) zu den Endgeräten, und braucht es Zusatzfunktionen wie Zutrittssysteme?

### Zwei verbreitete Modelle: Swissbit und YubiKey

Wie diese Prüfung aussehen kann, zeigen zwei verbreitete Modelle. Die Angaben zur Hardware stammen jeweils vom Hersteller.

#### Swissbit iShield Key 2

- Das Secure Element ist laut Swissbit nach Common Criteria EAL6+ eingestuft. Die Einstufung gilt für den Chip, nicht für den gesamten Key. Dazu gibt es eine Variante mit FIPS 140-3 Level 3.
- Swissbit entwickelt Hardware und Firmware selbst und fertigt den Key nach eigener Angabe in Berlin. Eine BSI-Zertifizierung nennt der Hersteller auf seiner Produktseite nicht.
- Es gibt USB-A und USB-C, jeweils mit NFC, sowie Varianten mit MIFARE DESFire EV3 für Zutritts- und Zeiterfassungssysteme.

#### YubiKey 5 Series von Yubico

- Die Serie gibt es mit USB-A, USB-C, NFC und Lightning. Neben FIDO2 unterstützt sie Smartcard (PIV), OTP und OpenPGP. Als Schutz nennt Yubico ein Hardware Secure Element, ohne dazu auf der Produktseite eine Zertifizierung anzugeben.
- Die FIPS-Validierung nach FIPS 140-3 gilt nur für die separate YubiKey 5 FIPS Series, nicht für die Standardserie. Yubico nennt für diese Serie eine Fertigung in den USA und in Schweden. Eine Fertigungsangabe für die Standardserie und eine BSI-Zertifizierung finden sich auf den Produktseiten ebenfalls nicht.

Für beide Hersteller führt Microsoft mehrere Modelle auf der Attestation-Liste für Entra ID, bei Swissbit etwa den iShield Key 2 samt Enterprise- und FIPS-Varianten, bei Yubico die YubiKey 5 Series, die FIPS Series und die Bio Series. Pro Modell stehen dort oft mehrere AAGUIDs, etwa getrennt nach Consumer- und Enterprise-Profil. Wer Key-Restrictions setzt, muss alle gewünschten Einträge übernehmen. Andere Hersteller auf derselben Liste lassen sich nach denselben Kriterien prüfen.

![MFA - Reifegradmodell](https://picsur.kngstn.eu/i/20da6c86-acba-4de9-9266-63e97a7280bf.png)

*MFA-Reifegradmodell: Level 4 sollte dabei Best Practice sein und Level 5 als passwortfreie Ausbaustufe. Als Ziel für 2027 bieten sich Level 4 oder 5 an.*

## NIS2, DORA und Cyber-Versicherungen

Die Modernisierung der Authentifizierung ist längst kein reines IT-Projekt mehr:

- NIS2: Das deutsche Umsetzungsgesetz gilt seit dem 6. Dezember 2025. § 30 Abs. 2 Nr. 10 BSIG nennt MFA oder kontinuierliche Authentifizierung ausdrücklich als Mindestmaßnahme des Risikomanagements, ebenso gesicherte Sprach-, Video- und Textkommunikation. Die europäische Vorlage steht in Art. 21 Abs. 2 Buchst. j der NIS2-Richtlinie. Phishingresistenz ist in beiden nicht ausdrücklich erwähnt.
- DORA: Für Finanzunternehmen verlangt die Verordnung starke Authentifizierung beim Zugriff auf kritische IT-Systeme. In der Praxis wird das meist mit MFA umgesetzt.
- Cyber-Versicherungen: Versicherer knüpfen ihre Bedingungen zunehmend an MFA-Nachweise, etwa für administrative Zugriffe, VPN, RDP und Cloud-Tenants. Ob eine fehlende Abdeckung zu höheren Prämien oder Leistungsausschlüssen führt, hängt jeweils vom einzelnen Vertrag ab.

## Handlungsempfehlungen für CIOs und IT-Leiter

Für einen geordneten Übergang empfiehlt sich dieses Vorgehen:

1. Bestandsaufnahme im Tenant: Finden Sie heraus, wer noch SMS oder Voice nutzt. Microsoft stellt dafür ein PowerShell-Skript „entra-sms-voice-usage-analyzer" bereit.
2. Aktivieren Sie den Passkey (FIDO2) in der Authentication Methods Policy und starten Sie für die SMS- und Voice-Nutzer eine eigene Kampagne. Wer mehr Zeit braucht, nutzt den Opt-out bis zum 1. Februar 2027. Entfernen Sie Nutzer aus dem Geltungsbereich von SMS und Voice, sobald sie ihren Ersatz getestet haben.
3. Passkey-Profil für Hardware-Keys anlegen: Ein Profil für gerätegebundene Passkeys mit erzwungener Attestation und den AAGUIDs der freigegebenen Key-Modelle stellt sicher, dass nur diese Schlüssel registriert werden. Die Attestation wird nur bei der Registrierung geprüft, bereits registrierte Keys bleiben nutzbar. Passkey-Profile lassen sich nach der Umstellung nicht mehr zurücknehmen und sind auf drei begrenzt, planen Sie die Aufteilung deshalb vorab. Synchronisierte Passkeys unterstützen laut Microsoft keine Attestation, dafür braucht es bei Bedarf ein eigenes Profil.
4. Conditional Access schärfen: Verlangen Sie die Authentifizierungsstärke Phishing-resistant MFA, gestaffelt nach Schutzbedarf. Beginnen Sie bei den Global Administratoren, dann folgen die Fachbereiche. Planen Sie auch die Notfallkonten ein, Microsoft und auch wir empfehlen dafür gerätegebundene Passkeys oder eine zertifikatsbasierte Anmeldung.
5. Hardware-Token ausrollen: Mitarbeiter ohne Diensthandy oder in Bereichen mit Smartphone-Verbot erhalten FIDO2-Hardware-Keys. Global Administratoren sollten nicht bis zum 1. Juli 2027 warten.
6. Temporary Access Pass (TAP) einführen: Nutzen Sie TAPs für das Onboarding neuer Mitarbeiter und die Erstregistrierung der Token ohne Passwort-Umwege.
7. Telefonie-Anbieter nur bei echtem Bedarf: Prüfen Sie Soprano oder Telesign erst dann, wenn ein regulatorischer oder betrieblicher Grund SMS oder Voice erzwingt. Für alle Nutzer ohne Telefonie-Anbieter und ohne andere MFA-Methode wird die Passkey-Registrierung ab dem 1. Februar 2027 zur Pflicht.

## Bereit für den Wechsel?

Ich helfe Ihnen natürlich auch gern bei der Implementierung und dem Rollout FIDO2 Token in Ihrem Unternehmen. Sprechen Sie mich einfach bei [LinkedIn](https://www.linkedin.com/in/thomaskrampe) an oder vereinbaren Sie gleich einen [Termin](https://t13k.de/meetme).
