---
title: Subnetrechner per API nutzen
date: 2025-02-07 12:00:00 +/-0000
categories: [Technologie, Tools]
tags: [how-to, network] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/e0783825-bbf1-42d0-bc51-b2c8da9441f1.png
---

Um einfach mal eine bestimmte IP-Adresse einem Subnet zuzuordnen, bedienen wir uns einfach einer frei verfügbaren API im Internet. Einfach die IP-Adresse in CIDR Notation z.B. per curl an die API schicken.

```shell
curl https://networkcalc.com/api/ip/10.0.1.15/27
```

Das Ergebnis ist dann eine Antwort in JSON:

```json
{"status":"OK","meta":{"permalink":"https://networkcalc.com/subnet-calculator/10.0.1.15/27","next_address":"https://networkcalc.com/api/ip/10.0.1.16/27"},"address":{"cidr_notation":"10.0.1.15/27","subnet_bits":27,"subnet_mask":"255.255.255.224","wildcard_mask":"0.0.0.31","network_address":"10.0.1.0","broadcast_address":"10.0.1.31","assignable_hosts":30,"first_assignable_host":"10.0.1.1","last_assignable_host":"10.0.1.30"}}
```

Wer das etwas lesbarer haben möchte kann die Ausgabe auch in eine Datei umleiten:

```shell
curl https://networkcalc.com/api/ip/10.0.1.15/27 > ip.txt
```

Und danach mit dem Tool `jq` auswerten (_auf macOS vorher z.B. mit Homebrew und dem Kommando `brew install jq' installieren_): 

```shell
cat ip.txt | jq '.address.cidr_notation'  
"10.1.101.0/28"
cat ip.txt | jq '.address.subnet_mask'                     
"255.255.255.240"
cat ip.txt | jq '.address.first_assignable_host'
"10.1.101.1"
cat ip.txt | jq '.address.last_assignable_host'
"10.1.101.14"
```

Da JSON wie XML hierarchisch auf gebaut ist gehen wir mit `.address` in den dritten Abschnitt (1. = .status, 2. = .meta, 3. = .address) und holen uns dort das Objekt .cidr_notation. Da wir das ja bereits kennen, weil wir das ja beim curl erfasst haben, wäre zum Beispiel die Subnetmaske interessant (die Bit kennen wir bereits).

Das ganze geht natürlich auch mit PowerShell:

```powershell
(Invoke-RestMethod https://networkcalc.com/api/ip/10.0.1.15/27).address
```

Ausgabe der API:

```powershell
PS /Users/thomas> (Invoke-RestMethod https://networkcalc.com/api/ip/10.0.1.15/27).address

cidr_notation         : 10.0.1.15/27
subnet_bits           : 27
subnet_mask           : 255.255.255.224
wildcard_mask         : 0.0.0.31
network_address       : 10.0.1.0
broadcast_address     : 10.0.1.31
assignable_hosts      : 30
first_assignable_host : 10.0.1.1
last_assignable_host  : 10.0.1.30
```
