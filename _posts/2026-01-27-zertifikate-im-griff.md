---
title: Strategisches Zertifikatsmanagement
date: 2026-01-27 08:00:00 +/-0000
categories: [Strategie, Automatisierung, Sicherheit]
tags: [security, pki, acme, automation] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/b94c4e30-69c7-4673-8a58-f03d9d8e6e8a.png
---

## Warum die 47-Tage-Deadline Ihre IT-Infrastruktur verändert

In der IT-Welt gab es bisher, man soll es kaum glauben, auch Konstanten, auf die wir uns jahrelang verlassen konnten. Eine davon ist die Verwaltung von SSL-Zertifikaten, die nur einmal im Jahr getauscht werden mussten. Doch diese Ära geht nun zu Ende, da Google und Apple die Verkürzung der Gültigkeitsdauer massiv voran treiben. Für Sie als CIO oder Techlead bedeutet das, wer jetzt nicht auf Automatisierung beim Zertifikatsmanagement setzt, wird spätestens in diesem Jahr ein Compliance- und Verfügbarkeitsproblem bekommen.

## Was und Warum - von 398 Tagen zu 47 Tagen

Die Entwicklung hin zu "Short-lived Certificates" ist bereits seit längerem festgelegt und startet in diesem Jahr. Die Roadmap für die Laufzeit der Zertifikate sieht dabei wie folgt aus:

* Ab März 2026: Reduzierung auf eine Gültigkeit von maximal 200 Tagen.
* Ab März 2027: Reduzierung auf eine Gültigkeit von maximal 100 Tagen.
* Ab März 2029: Zielmarke von 47 Tagen.

Hintergrund dieser Entwicklung ist die kryptographische Agilität. Kürzere Laufzeiten verringern das Missbrauchsfenster kompromittierter Schlüssel und bereiten Infrastrukturen auf den schnellen Wechsel von Verschlüsselungsstandards vor (z. B. [Post-Quantum-Cryptography][1]) Wenn eine Schwachstelle in einem Verschlüsselungsalgorithmus (wie z.B. bei RSA) entdeckt wird oder auch ein Schlüssel abhanden kommt (was ihnen hoffentlich nie passiert), dauert es bei einer Laufzeit von 398 Tagen imungünstigsten Fall eben ein ganzes Jahr, bis alle Zertifikate im Internet getauscht sind. Bei 47 Tagen ist das gesamte Internet innerhalb von maximal 7 Wochen "geheilt", ohne dass Zertifikate mühsam manuell widerrufen (Revoke) werden müssen, was technisch oft unzuverlässig ist.

Der Wechsel zu Kurzzeit-Zertifikaten ist auch kein "Mal-eben-so-nebenbei-Update". Er birgt viele operative Risiken, bietet aber auch strategische Vorteile.

1. **Compliance-Falle (NIS2 & DORA)**: Unter neuen EU-Regularien wie NIS2 oder DORA wird Business Continuity großgeschrieben. Ein abgelaufenes Zertifikat in einem kritischen System gilt nicht mehr als "kleiner Fehler", sondern kann als meldepflichtiger Vorfall eingestuft werden, da es die Verfügbarkeit von Diensten aktiv verhindert.

2. **Manuelles Management als Zeitbombe**: Das größte Risiko ist der menschliche Faktor. Ein manueller Prozess, der alle 47 Tage angestoßen werden muss, wird zwangsläufig irgendwann vergessen. Die Folge sind abgelaufene Zertifikate, blockierte Webseiten und unterbrochene API-Kommunikationen.

## Warum 47 Tage?

Warum jetzt 47 Tage und nicht 45 oder 48? 

Das war die erste Frage, die ich von Korrekturlesern bekommen habe und tatsächlich ist das eine durchaus berechtigte Frage. Es wirkt fast so, als hätte jemand gewürfelt. Aber hinter dieser "krummen" Zahl steckt eine sehr präzise regulatorische Logik, die auf der Wochenstruktur basiert.

1\. **Die "Vierteljährliche" Logik**
Das Ziel der Browser-Hersteller ist es, Zertifikatslaufzeiten in einen Rhythmus zu bringen, der in Quartale passt.

* Ein Jahr hat 52 Wochen.
* Ein Vierteljahr hat dementsprechend 13 Wochen.
* Wenn man jetzt die Laufzeit so verkürzt, dass sie **deutlich** unter einem Quartal liegt (< 91 Tage), zwingt man Unternehmen dazu, die Erneuerung mindestens einmal pro Quartal (und idealerweise öfter) durchzuführen.

2\. **Die 39-Tage-Regel plus Puffer**
Ursprünglich gab es den Vorschlag für eine 30-Tage-Gültigkeit. Die Industrie (CA's und Unternehmen) intervenierte jedoch, da dies zu extrem sei. Man einigte sich auf ein Fenster, das etwa anderthalb Monate umfasst. Die technische Empfehlung sieht oft 39 Tage als Basis vor, aber man addierte eine 8-tägige "Grace Period" (Gnadenfrist).

**39 Tage (Kernlaufzeit) + 8 Tage (Puffer) = 47 Tage.**

Diese 8 Tage Puffer sind kritisch, um ein Wochenende oder einen Feiertag zu überbrücken, falls die automatisierte Erneuerung am Freitagabend fehlschlägt und erst am Montag jemand eingreifen kann.

3\. **Warum jetzt nicht 48 oder 45?**

In der Standardisierung von TLS-Zertifikaten (durch das CA/B-Forum) wird oft versucht, eine maximale Laufzeit zu definieren, die genau unterhalb einer runden monatlichen Grenze liegt, um "kreative Auslegungen" zu verhindern.

* 45 Tage wären genau 1,5 Monate (zu knapp bei Schaltmonaten).
* 48 Tage würde fast 7 Wochen entsprechen.
* 47 Tage stellt sicher, dass ein Zertifikat innerhalb einer 7-Wochen-Frist zwingend ablaufen muss, egal wie man rechnet.

**Zusammenfassend können wir sagen:** 47 Tage ist der "Sweet Spot" zwischen maximalem Sicherheitsdruck durch die Browser-Hersteller und der minimalen operativen Überlebensfähigkeit für IT-Abteilungen.

## Der Schutz der verwendeten Schlüssel

Automatisierung des Zertifkatsmanagements bedeutet auch eine höhere Frequenz bei der Schlüsselerstellung. Umso wichtiger ist es, dass diese hochsensiblen "Privaten Schlüssel" niemals ungeschützt auf dem Dateisystem liegen. Der Einsatz von Key Management Services (KMS) wird jetzt zur Pflicht. Einige Beispiele von KMS sind:

* Azure Key Vault / AWS KMS: Hochverfügbare Cloud-Tresore, die Schlüssel oft in Hardware (HSM) speichern, sodass sie niemals im Klartext exportiert werden können.
* HashiCorp Vault: Ideal für Hybrid-Cloud-Szenarien, um Schlüssel zentral über verschiedene Plattformen hinweg zu verwalten.
* Lokal betriebene Passwortmanager bieten in den Enterprise Versionen auch oft eine Verwaltung der Schlüssel, sind aber in der Regel auch nur intern erreichbar, im Gegensatz zu den Cloud Diensten.

Darüber hinaus ist die automatische Key-Rotation und das generieren eines komplett neuen Schlüsselpaares bei jeder Erneuerung der Zertifikate ein absolutes Pflichtprogramm.

## Die technische Umsetzung mit dem ACME Protokoll

Tatsächlich ist die Automatisierung durch das ACME Protokoll implementiert und wird bereits von einigen CA's (Certificate Authorities) wie z.B. Let's Encrypt seit Jahren verwendet.

Hier mal zwei Beispiele, wie die automatische Erstellung umgesetzt werden kann. Beide Beispiele bitte so nicht produktiv einsetzen, sondern als Hilfe für eigene Skripte verwenden.

### Powershell und Azure

Für Windows-Administratoren bietet sich das Modul Posh-ACME an. Es lässt sich perfekt in bestehende Deployment-Pipelines integrieren.

```powershell
# 1. Powershell Modul installieren
Install-Module -Name Posh-ACME -Scope CurrentUser

# 2. Azure-Credentials für DNS-Challenge hinterlegen
$azParams = @{
    AZSubscriptionId = 'ihre-subscription-id'
    AZTenantId = 'ihre-tenant-id'
}

# 3. Zertifikat abrufen und direkt in den Azure Key Vault exportieren
# Dies stellt sicher, dass der Key niemals unsicher lokal gespeichert bleibt. Wenn kein spezifischer Server-Parameter angegeben wird, nutzt Posh-ACME automatisch Let's Encrypt (Produktionsumgebung) als Standard-CA. 
New-PACertificate 'www.ihre-domain.de' -DnsPlugin Azure -PluginArgs $azParams -AcceptTOS

# Export in den Key Vault (Beispiel-Skriptsequenz)
$cert = Get-PACertificate 'www.ihre-domain.de'
$pfxPassword = ConvertTo-SecureString -String "IhrSicheresPasswort" -AsPlainText -Force
$pfxPath = "$env:TEMP\cert.pfx"

Export-PfxCertificate -Cert "Cert:\CurrentUser\My\$($cert.Thumbprint)" `
                      -FilePath $pfxPath ` 
                      -Password $pfxPassword `

# Upload zum Azure Key Vault
Import-AzKeyVaultCertificate -VaultName 'IhrVaultName' -Name 'WebserverZertifikat' -FilePath "./cert.pfx" -Password $pfxPassword
```

### Linux und acme.sh

Mal davon abgesehen, dass auch unter Linux die Powershell genutzt werden kann, bevorzugen die meisten Admins doch Linux-native Lösungen. Unter den meisten Linux-Systemen ist das `acme.sh` Skript die bevorzugte Wahl, da es lediglich eine einfache Shell benötigt und keinerlei Abhängigkeiten zu anderen Anwendungen aufweist. Um das folgende Beispiel vergleichbarer zu machen, nutze ich hier ebenfalls den Azure Key Vault für die Schlüssel. Während `acme.sh` nativ viele "Deploy-Hooks" (z. B. für HashiCorp Vault) unterstützt, wird für den Azure Key Vault in der Regel die Azure CLI (`az`) als Brücke genutzt, um das Zertifikat und die Schlüssel sicher zu übertragen.

```bash
# 1. Installation von acme.sh
curl https://get.acme.sh | sh -s email=admin@ihre-domain.de

# 2. Zertifikat abrufen (z.B. via DNS-Challenge mit dem DNS von Cloudflare in diesem Beispiel)
export CF_Token="Ihr_Cloudflare_Token"
export CF_Account_ID="Ihre_Account_ID"
acme.sh --issue --dns dns_cf -d www.ihre-domain.de

# 3. Deployment in den Azure Key Vault
# Wir nutzen die Azure CLI, um das Zertifikat als PKCS12-Datei (.pfx) zu importieren
# Hinweis: Das Zertifikat wird im Key Vault automatisch versioniert.

# Zuerst in PFX konvertieren (acme.sh erledigt dies intern im cert-Ordner)
# Dann der Import-Befehl:
CERT_PFX_PATH="/home/user/.acme.sh/www.ihre-domain.de/www.ihre-domain.de.pfx"

az keyvault certificate import \
  --vault-name "IhrKeyVaultName" \
  --name "WebserverZertifikat" \
  --file "$CERT_PFX_PATH"
```

## Kontrolle und Monitoring der Automatisierung

Automatisierung des Zertifikatsmanagement benötigt zwingend eine Zustandsüberwachung (Observability) sowie ein begleitendes Monitoring der Automatisierungsprozesse. Wir müssen hier natürlich prüfen, ob unsere automatisierten Prozesse auch erfolgreich funktionieren.

* **Fehler-Alarmierung**: Tools wie z.B. Prometheus helfen uns, den Status der TLS-Endpunkte kontinuierlich zu prüfen. Hier können wir schnell feststellen, ob die Zertifkate als gültig und somit als erfolgreich erneuert gesehen werden.
* **Certificate Transparency (CT) Logs**: Überwachen der CT-Logs ist genauso wichtig, um in Echtzeit zu sehen, welche Zertifikate für Ihre Domains ausgestellt wurden, um so eventuell unberechtigte Ausstellungen sofort zu erkennen.
* **Skript Logs und Fehlerbehandlung**: Wenn Sie ihr Zertifikatsmanagement mit Skripts automatisieren, sollten die Skripte entsprechende Fehlerbehandlungsroutinen sowie ein Logging enthalten. Auch das muss über proaktives Monitoring zu einer Alarmierung führen.

## Fazit

Die Verkürzung der Zertifikatslaufzeiten sollte nicht als ein technisches Ärgernis gesehen werden, sondern eher als ein strategischer Impuls. Wer heute noch Zertifikate manuell beantragt, per E-Mail anfordert und per Copy-Paste verteilt, handelt grob fahrlässig gegenüber der eigenen Betriebssicherheit. Die technische Automatisierung via ACME oder PowerShell ist das Fundament und bei einer Gültigkeit von "nur" 47 Tagen bleibt kein Spielraum für manuelle Eingriffe, falls eine automatisierte Erneuerung fehlschlägt (z. B. durch DNS-Probleme oder API-Fehler der CA). Es ist daher ratsam, den Erneuerungsprozess bereits nach der Hälfte der Laufzeit zu initiieren. Dies schafft ein Zeitfenster von über drei Wochen, um im Fehlerfall eine Alarmierung zu erhalten und eingreifen zu können, bevor ein Dienst tatsächlich ausfällt.

Auch ist die durch ACME geschaffene "Kryptographische Agilität" nicht nur für die kurzen Laufzeiten wichtig. Sie bereitet die Infrastruktur auch darauf vor, in naher Zukunft auf quantenresistente Algorithmen umzustellen. Wer heute automatisiert, wird morgen den Austausch von Tausenden Zertifikaten gegen PQC-fähige Varianten per Knopfdruck erledigen können.

Der Weg führt weg von "Zertifikaten als jährlich wiederkehrende Aufgabe" hin zu "Identity als Managed Service". Nutzen Sie die kommenden Monate, um automatisierte Workflows mit sicherer Schlüsselverwahrung zu etablieren. Dann sind 47 Tage Laufzeit auch kein Risiko mehr, sondern ein Zeichen für eine moderne, abgesicherte IT-Infrastruktur.

*[PQC]: Post Quantum Cryptography
*[CA]: Certificate Authority
*[ACME]: Automated Certificate Management Environment
*[API]: Application Programming Interface
*[NIS2]: Network and Information Security Directive 2
*[DORA]: Digital Operational Resilience Act

[1]: https://thomas-krampe.com/posts/quanten-ready-und-performance-stark/
