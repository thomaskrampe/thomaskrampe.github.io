---
title: Quanten-Ready und Performance-Stark
date: 2025-11-16 13:30:00 +/-0000
categories: [Strategie, Security, Kryptographie]
tags: [security, pqc, ecc, kryptographie] # TAG names should always be lowercase
image:
  path: https://picsur.myctx.net/i/d369ce55-4a67-4ce4-b657-04157e4c2325.png
---

## Warum Ihr Unternehmen jetzt die Schlüssel wechseln sollte

Als CIO oder CISO wissen Sie sicher, IT-Sicherheit darf kein statischer Zustand sein. Die Zeit drängt bereits, in der Kryptographie den nächsten Schritt zu gehen und auch für die Post-Quatum-Cryptography (PQC) gerüstet zu sein. Obwohl realistische Quantencomputer noch nicht flächendeckend im Einsatz, sollten wir heute schon wichtige Schritte zur Krypto-Agilität und Vorbereitung treffen. Die Gefahr des **"Harvest Now, Decrypt Later"** (*Heute verschlüsselte Daten stehlen, um sie später mit Quantencomputern zu entschlüsseln*) ist real.

Der Algorithmus von **Shor** kann bereits die mathematische Grundlage von RSA, die Faktorisierung großer Primzahlen, hocheffizient aufbrechen. ECC ((Elliptic Curve Cryptography) ist, auch wenn nicht quantensicher, weniger exponiert und die modernen kryptographischen Entwicklungen basieren oft auf ECC-Prinzipien. Der Wechsel zu ECC ist somit das wichtigste Zwischenziel zur Post-Quanten-Kryptographie.

Dieser Wechsel ist tatsächlich nicht nur eine vage Empfehlung von mir, sondern in Deutschland bereits eine zwingende Notwendigkeit, wie das Beispiel der Telematikinfrastruktur (TI) zeigt. Bis zum 1. Januar 2026 muss das Gesundheitswesen die Umstellung vollzogen haben, weil ältere RSA-Verfahren dann unzulässig werden (gemäß BSI TR-02102-1). Über das Thema wird aktuell in der Presse, im Zusammenhang mit dem E-Rezept, auch ausführlich berichtet.

Das sollte ein Weckruf für alle Unternehmen sein, denn die Gründe warum dieser Schritt nötig ist, betrifft früher oder später auch die Infrastrukturen, die nicht zu Konnektoren, Heilberufsausweisen (eHBA), Praxisausweisen (SMC-B) und Kartenterminals gehören.

## Was können und sollten wir heute bereits machen?

Was in vielen Bereichen gültig ist, gilt auch hier. Was wir nicht kennen, können wir nicht schützen. Deshalb ist der wichtigste Schritt zuerst die Inventur und die Nutzung hybrider Verfahren in Pilotprojekten zu testen.

### 1. Inventur

* **Identifizieren Sie alle kryptografischen Abhängigkeiten:** Finden Sie heraus, wo genau in Ihrer Infrastruktur RSA oder bereits ECC (Elliptic Curve Cryptography) für Schlüsselaustausch und Signaturen verwendet wird.

* **Priorität:** SSL/TLS-Zertifikate, VPN-Zugänge, Code-Signierungsschlüssel, und Langzeit-Archivierungsdaten sollten hier Priorität haben.

* **Überprüfen Sie die Krypto-Agilität:** Stellen Sie fest, wie leicht sich die kryptografischen Bibliotheken (z. B. OpenSSL, BoringSSL) in Ihren Systemen austauschen lassen. Veraltete Software ist ein großes Hindernis.

### 2. Proof-of-Concept

* **Starten Sie Pilotprojekte mit hybrider Kryptografie:** Bei diesem Ansatz sichern Sie die Kommunikation doppelt ab.

* **Alt und Neu:** Eine Verbindung wird sowohl mit einem klassischen Algorithmus (z. B. ECC) als auch mit einem Post-Quanten-Algorithmus (zum Beispiel CRYSTALS-Kyper) sowie gesichert. Empfohlen wird der hybride  Ansatz für den Schlüsseltausch, das heißt, der Sitzungsschlüssel wird sowohl durch den bewährten ECC-Schlüsselaustausch (z. B. ecdh-sha2-nistp256) als auch durch den quantenresistenten Kyber-Schlüsselaustausch gesichert.

* **Sicherheitsvorteil:** Die Verbindung ist nur dann unsicher, wenn beide Algorithmen gebrochen werden. Dies schützt Sie gegen den Durchbruch der PQC-Algorithmen und gibt Ihnen gleichzeitig eine sofortige Abwehr gegen zukünftige Quantencomputer.

* **Testen Sie die Performance:** Messen Sie, wie sich die neuen, oft größeren Schlüssel und die längeren Rechenzeiten der PQC-Algorithmen auf Ihre Server-Latenz und den Speicherbedarf auswirken.

### 3. Bibliothek-Update

* Aktualisieren Sie, wenn möglich, Ihre kryptografischen Bibliotheken auf die aktuellen Versionen, die bereits PQC-fähige Algorithmen unterstützen (*z. B. OpenSSL 3.5 oder BoringSSL*).

## PQC Algorithmen, die wir heute bereits einsetzen können

Wir können heute bereits die Algorithmen nutzen, die vom **NIST** als zukünftige Standards ausgewählt wurden. Die folgenden Algorithmen bieten derzeit die größte Sicherheit und sind für den Praxiseinsatz konzipiert:

|            Anwendungsfall             |     Algorithmus-Familie      |   NIST-Gewinners   |
| :-----------------------------------: | :--------------------------: | :----------------: |
|     Schlüsselaustausch (TLS/SSL)      | Gitter-basierte Kryptografie |   CRYSTALS-Kyber   |
| Digitale Signaturen (Code-Signierung) | Gitter-basierte Kryptografie | CRYSTALS-Dilithium |

### Empfehlung für beste Sicherheit & Performance

CRYSTALS-Kyber ist der Algorithmus, der derzeit die größte Sicherheit bei guter Performance und relativ einfacher Implementierung bietet.

* **Sicherheit:** Er basiert auf sogenannten Learning with Errors (LWE)-Problemen in Gitternetzen, die als extrem schwer lösbar gelten und von Experten als die sicherste PQC-Familie angesehen werden.

* **Performance:** Kyber wurde so konzipiert, dass es schnell ist und Schlüsselgrößen sowie Chiffriertexte produziert, die für heutige Internet-Protokolle (wie TLS) praktikabel sind. Es ist in Geschwindigkeit und Bandbreite vergleichbar mit ECC, aber langsamer als RSA.

* **Implementierung:** Er ist der ausgewählte Standard für den Schlüsselaustausch und wird in immer mehr kommerziellen und Open-Source-Bibliotheken unterstützt.

Sollten Sie aus irgendwelchen Gründen noch nicht auf die neueren PQC-Algorithmen umstellen können, sollten Sie dennoch von RSA auf ECC wechseln. Die Hauptschwäche von RSA ist das schlechte Verhältnis von Sicherheit zu Schlüsselgröße.

|     Kriterium      | ECC (256 Bit) | RSA (3072 Bit) |
| :----------------: | :-----------: | :------------: |
|   **Sicherheit**   |     Hoch      |     Gleich     |
| **Schlüssellänge** |   Sehr kurz   |   Sehr lang    |
|  **Performance**   |    Schnell    |    Langsam     |

Selbst wenn Sie auf RSA-Schlüssel mit 4096 Bit oder mehr aufrüsten, um die Sicherheit zu erhöhen, bezahlen Sie mit drastisch langsameren Prozessen und einem höheren Rechen- und Energieverbrauch. ECC liefert die gleiche oder höhere Sicherheit mit minimalem Aufwand.

## Der pragmatische Plan für den Schlüsseltausch

Ein Wechsel muss nicht kompliziert sein. Der technische Weg, den viele Experten empfehlen, ist schrittweise und kontrolliert:

**1\. Kurzfristig (Klassisch) RSA-Signaturen ersetzen:** Wechseln Sie Ihre Host- und Benutzer-Schlüsselpaare von RSA auf ED25519 oder ECDSA. Diese sind deutlich resistenter und schneller als RSA.

So generieren Sie z.B. ein stärkeres Schlüsselpaar:

```shell
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519
```

Der heutige Umstieg auf ED25519 ist der einfachste und wirksamste Schritt, den Sie zur Verbesserung der SSH-Sicherheit unter den derzeit offiziell verfügbaren Algorithmen unternehmen können.

**2\. Mittelfristig (Hybrid-Pilot) Quantenresistente Signaturen einführen:** Generieren Sie mit einem gepatchtem SSH hybride Schlüsselpaare (z. B. ED25519 + Dilithium). Diese können heute in Pilotprojekten verwendet werden.

**3\. Langfristig (Vollständig PQC) vollständige Migration:** Ersetzen Sie alle klassischen Schlüssel durch den PQC-Signaturalgorithmus CRYSTALS-Dilithium, sobald dieser offiziell in OpenSSH integriert ist.

Warten Sie nicht, bis externe Vorgaben oder ein Sicherheitsvorfall Sie zum Handeln zwingen. Nutzen Sie die Umstellung jetzt als Chance, um Ihre IT-Sicherheit zukunftsfähig, schneller und effizienter zu machen.

*[PQC]: Post Quantum Cryptography
*[RSA]: Rivest–Shamir–Adleman
*[ECC]: Elliptic Curve Cryptography
*[TLS]: Transport Layer Security
*[SSL]: Secure Sockets Layer
*[SSH]: Secure Shell
*[LWE]: Learning With Errors
*[NIST]: National Institute of Standards and Technology
*[VPN]: Virtual Private Network
