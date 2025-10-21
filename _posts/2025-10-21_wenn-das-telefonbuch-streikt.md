---
title: Wenn das Telefonbuch des Internets streikt
date: 2025-10-21 09:00:00 +/-0000
categories: [Technologie, Sicherheit, DNS]
tags: [dns, dnssec, ha] # TAG names should always be lowercase
image:
  path: https://picsur.myctx.net/i/9cd33a95-5097-4682-a422-3d7b3010ef88.png
---

## DNS-Sicherheit: Ausfallsicherheit und Verschlüsselung als Fundament der digitalen Souveränität

Die jüngsten massiven Ausfälle bei Amazon Web Services (AWS) und Microsoft Azure haben uns wieder einmal mehr vor Augen geführt, wie fragil und systemkritisch das Domain Name System (DNS) in unserer hochgradig vernetzten digitalen Infrastruktur ist. Ende letzten Jahres legte ein DNS-Fehler in einer Schlüsselregion von AWS Dienste von der Kryptobörse bis zum Streaming-Anbieter lahm, was Millionen von Nutzern weltweit betraf. Auch ein globaler Ausfall bei Azure Front Door zeigte kürzlich (09.10.2025), wie die zentrale Abhängigkeit von DNS-Komponenten in großen Cloud-Umgebungen aber auch in lokalen Netzwerken zu einen Single Point of Failure (SPOF) werden kann.

Diese Vorfälle sind ein Weckruf. Sie unterstreichen die Notwendigkeit, die eigene DNS-Infrastruktur nicht nur als reinen Namensauflösungsdienst, sondern als kritische Sicherheitssäule zu begreifen, die auf maximaler Ausfallsicherheit (Hochverfügbarkeit) und Datensicherheit (Verschlüsselung) basiert.

## Ausfallsicherheit: Das Prinzip der Redundanz auf DNS-Ebene

Hochverfügbarkeit im DNS ist nicht optional, sondern die Grundvoraussetzung für jeden ernstzunehmenden Betreiber eines Netzwerkes. Es geht darum, sicherzustellen, dass die Namensauflösung auch dann funktioniert, wenn einzelne Komponenten oder sogar ganze Rechenzentren ausfallen.

### Geografische und systemische Redundanz

Der Schlüssel liegt in der konsequenten Anwendung des Prinzips der Redundanz.

1\. **Multi-Provider-Strategie:** Verlassen Sie sich nicht nur auf einen einzigen DNS-Anbieter (wie es in den Cloud-Ausfällen der Fall war). Betreiben Sie Ihre kritischen Zonen über mindestens zwei, idealerweise drei verschiedene, geografisch verteilte DNS-Anbieter oder betreiben Sie eine eigene On-Premise-Infrastruktur zusätzlich zu einem Cloud-Dienst. Dies verhindert, dass ein Ausfall bei einem Anbieter Ihre Dienste komplett lahmlegt.

2\. **Anycast-Routing:** Setzen Sie auf Anycast für Ihre autoritativen Nameserver. Anycast ermöglicht es, dieselbe IP-Adresse von mehreren Servern an unterschiedlichen Standorten weltweit zu annoncieren. Fällt einer dieser Standorte aus, übernimmt das Routing-Protokoll (BGP) automatisch einen anderen, geografisch näheren und verfügbaren Server. Dies reduziert nicht nur die Latenz, sondern maximiert auch die Ausfallsicherheit.

![Unicast vs. Multicast](https://picsur.myctx.net/i/53bd8249-dfc0-4211-a60f-8ad7bc27ee8a.png)

3\. **Primär/Sekundär-Architektur:** Konfigurieren Sie Ihre Zonen mit mindestens einem Primären Nameserver (Master) und mehreren Sekundären Nameservern (Slaves), die über AXFR- oder IXFR-Protokolle synchronisiert werden. Diese Server sollten idealerweise in unterschiedlichen Autonomous Systems (AS) liegen.

4\. **TTL-Management**: Setzen Sie die Time-To-Live (TTL)-Werte für Ihre kritischen DNS-Einträge nicht zu hoch. Eine moderate TTL (z.B. 300 Sekunden / 5 Minuten) stellt sicher, dass Caches im Falle eines Ausfalls schnell auf einen neu verteilten oder geänderten Eintrag reagieren können. Eine zu niedrige TTL kann allerdings die Last auf Ihren Servern erhöhen. Hier ist etwas Balance gefragt.

## Verschlüsselung und Integrität: Der Schutz des DNS-Verkehrs

Ausfallsicherheit löst nur die Verfügbarkeit. Die Integrität und Vertraulichkeit der DNS-Abfragen adressieren wir durch Verschlüsselung und Authentifizierung.

### DNSSEC: Die Integritätssicherung

**DNS Security Extensions (DNSSEC)** ist der De-facto-Standard zur Sicherstellung der Datenintegrität im DNS. Es schützt Nutzer vor der Manipulation von DNS-Antworten, auch bekannt als DNS-Spoofing oder Cache Poisoning.

DNSSEC funktioniert durch die digitale Signierung von DNS-Zonendaten mit Public-Key-Kryptographie.

* **Signierung:** Die Zone wird mit einem Zone-Signing Key (ZSK) und einem Key-Signing Key (KSK) signiert.
* **Validierung:** Der Resolver des Endnutzers prüft die kryptografische Signatur (RRSIG-Eintrag) mithilfe des öffentlichen Schlüssels der Zone, der über den DS-Eintrag (Delegation Signer) in der übergeordneten Zone verankert ist.

> Empfehlung: Setzen Sie DNSSEC konsequent für alle Ihre kritischen und öffentlich zugänglichen Zonen ein. Das stellt sicher, dass Anfragen zu Ihren Domains authentisch sind und nicht auf manipulierte Adressen umgeleitet werden.
{: .prompt-tip }

### Verschlüsselung der Abfragen: DNS over TLS und HTTPS

Das klassische DNS-Protokoll (Port 53, UDP/TCP) überträgt Anfragen unverschlüsselt. Dies ermöglicht Lauschangriffe (Eavesdropping) und die Filterung des Traffics. Hier setzen modernere Protokolle an:

1\. DNS over TLS (DoT):

* Verschlüsselt DNS-Verkehr über TLS auf Port 853
* Bietet End-zu-End-Verschlüsselung zwischen dem Resolver und dem DNS-Server.
* Wird oft auf der Ebene des Betriebssystems oder im Unternehmensnetzwerk (z.B. zwischen internen Forwardern und externen Resolvern) eingesetzt.

2\. DNS over HTTPS (DoH):

* Verschlüsselt DNS-Verkehr über HTTPS auf Port 443.
* Der Vorteil: Der Traffic tarnt sich als regulärer HTTPS-Web-Traffic, was das Blockieren oder Filtern im Netzwerk erschwert.
* Wird häufig in Browsern und Apps eingesetzt, was jedoch im Unternehmenskontext die Transparenz und Kontrollmöglichkeit des Netzwerkadministrators über die DNS-Nutzung verringert.

>Empfehlung: Betreiben Sie Ihre internen DNS-Forwarder mit DoT oder DoH zu vertrauenswürdigen, validierenden (DNSSEC-fähigen) Upstream-Resolvern. Dies schützt die Privatsphäre der Nutzer und verhindert DNS-Manipulation innerhalb der Transportstrecke.
{: .prompt-tip }

## Aufbau einer robusten DNS-Infrastruktur: Technische Tipps

Der Aufbau einer sicheren und hochverfügbaren DNS-Infrastruktur folgt einem gestaffelten Modell.

1\. **Trennung von Autoritativen und Rekursiven Servern**

* Autoritative Nameserver: Beherbergen Ihre Zonendateien. Sie müssen hochverfügbar und nach außen hin abgeschirmt sein. Keine rekursive Auflösung zulassen.
* Rekursive Resolver: Dienen Ihren Clients zur Namensauflösung. Sie müssen DNSSEC validieren und sollten DoT/DoH-fähig sein, um mit externen Upstream-Servern sicher zu kommunizieren.

2\. **Implementierung des "Response Rate Limiting" (RRL)**
RRL ist eine essenzielle Maßnahme gegen Distributed Denial of Service (DDoS) Angriffe, insbesondere DNS-Amplification-Angriffe. RRL begrenzt die Anzahl identischer Antworten, die ein Nameserver pro Sekunde auf Anfragen aus einer bestimmten Quelle sendet.

3\. **Monitoring und Alerting**

* Proaktives Health-Check-Monitoring: Überwachen Sie die Latenz und die Erreichbarkeit jedes Nameservers von verschiedenen geografischen Standorten aus.
* Messung der DNSSEC-Validierung: Überprüfen Sie regelmäßig, ob Ihre Resolver die Signaturen erfolgreich validieren können.
* Cache-Überwachung: Monitoring der Cache-Hit-Rate zur Optimierung der Performance und zur Früherkennung von Cache-Poisoning-Versuchen.

## Fazit

Wer seine digitale Souveränität wahren will, muss das Fundament, in diesem Fall DNS, zwingend robuster gestalten und als kritisches System wahrnehmen.

*[DNS]: Domain Name System
*[SPOF]: Single Point of Failure
*[RRL]: Response Rate Limiting
*[DDoS]: Distributed Denial of Service
*[DNSSEC]: DNS Security Extensions
*[BGP]: Border Gateway Protocol
*[TTL]: Time-To-Live
