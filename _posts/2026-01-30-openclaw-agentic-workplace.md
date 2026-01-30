---
title: OpenClaw Agentic Workplace 
date: 2026-01-30 08:03:00 +/-0000
categories: [Technologie, Tools, KI]
tags: [ki, agentic, security, openclaw] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/80200c66-92f6-4967-a733-c58335504516.png
---

## Wenn der Hummer das Terminal übernimmt

In der IT-Welt gibt es Momente, in denen ein Projekt die Grenze zwischen "interessantes Spielzeug" und "paradoxer Gamechanger" innerhalb von Tagen überschreitet. Genau das erleben wir gerade mit OpenClaw (ehemals Moltbot, ehemals Clawdbot). Was als Hobbyprojekt von Peter Steinberger (Gründer von PSPDFKit/Nutrient) begann, hat sich zu einem Phänomen entwickelt, das die Vision des "Agentic Workplace" radikal beschleunigt, aber gleichzeitig massive Sicherheitsfragen aufwirft, die wir eigentlich im Enterprise-Umfeld längst für gelöst hielten.

Ich persönlich sehe hier eine gefährliche Entwicklung. Wir geben einer Blackbox die Schlüssel zu unserem digitalen Leben. Aber nur weil es "irgendwie cool ist", seinen Rechner per WhatsApp zu steuern heißt das noch lange nicht, dass wir es auch machen sollten. Aber beginnen wir mal ganz vorn.

## Die Evolution von Clawdbot zu Moltbot (zu OpenClaw)

Die Historie des Bots ist fast so rasant wie seine technische Entwicklung. Im November 2025 unter dem Namen Clawdbot gestartet, wurde das Projekt schnell zum Liebling der Entwickler-Szene. Der Name war eine (vielleicht zu deutliche) Hommage an Anthropic’s Claude-Modelle.

Der Erfolg rief natürlich, wie sollte es auch anders sein, die Juristen auf den Plan. Anthropic forderte eine Umbenennung wegen Markenrechtsverletzungen. Steinberger reagierte mit Humor und taufte den Bot in Moltbot um, passend zum Maskottchen, dem Hummer (Lobster), der sich häutet (molting), um zu wachsen. Doch die Häutung war schmerzhaft. In der Sekunden-Lücke des Rebrandings kaperten Krypto-Scammer die alten Handles auf X und GitHub. Ein Vorgeschmack darauf, wie schnell die Kontrolle verloren gehen kann, wenn man sich in diesem hochdynamischen Umfeld bewegt. Jetzt wurde das ganze Projekt erneut umbenannt und ist jetzt unter [**OpenClaw**][1] bekannt.

## Was ist OpenClaw eigentlich?

Im Kern ist OpenClaw ein autonomer KI-Agent, der auf Ihrer eigenen Hardware läuft. Im Gegensatz zu ChatGPT, das in seinem Browser-Tab eingesperrt bleibt, hat OpenClaw "Hände" und kann tatsächlich etwas proaktiv "machen".

* Browsersessions steuern, um Flüge zu buchen oder Webseiten zu analysieren.
* Terminal-Befehle auf Ihrem Mac oder Server ausführen.
* Dateisysteme durchsuchen und Dokumente verarbeiten.
* Über Messenger wie WhatsApp, Signal oder Telegram mit Ihnen kommunizieren, um nur einige Fähigkeiten zu nennen.

Die strategische Relevanz ist klar. Wir bewegen uns weg von der reinen Textgenerierung hin zur proaktiven Aufgabenbewältigung.

## Das CISO-Dilemma

Aus der Perspektive der IT-Sicherheit ist OpenClaw ein zweischneidiges Schwert. Einerseits bietet die lokale Ausführung eine bessere Datensouveränität als Cloud-Agenten. Andererseits öffnen wir alle Türen für Indirect Prompt Injections.

Stellen Sie sich vor, OpenClaw liest eine Website oder eine E-Mail, die einen versteckten Befehl enthält: "Lösche alle Dateien im Home-Verzeichnis". Wenn der Bot Terminal-Zugriff hat, würde er das sicher ausführen. Ohne Leitplanken ist OpenClaw nicht Ihr Assistent, sondern ein potenzielles Trojanisches Pferd mit Admin-Rechten. Aber wir müssen gar nicht so böse denken, wenn auch der Human Error eine Rolle spielen kann. Eine missverständliche Anweisung wie z.B. "Räume auf und lösche meine temporären Ordner" kann von dem Bot auch mit dem Kommando `rm -rf /` erreicht werden. Vielleicht nicht unbedingt das gewünschte Ergebnis, oder?

## Hands-on: OpenClaw sicher installieren

Wenn Sie OpenClaw nun trotzdem noch testen wollen, dann bitte nicht nativ auf Ihrem Arbeitsrechner. Ich empfehle eine strikte Isolation.

**1\. Installation auf einem Mac Mini (Dedizierte Hardware)**

Viele nutzen einen Mac Mini M2/M4 als dedizierten "AI-Server" (das merkt auch Apple gerade an den Bestellungen). Das ist sinnvoll, da der Bot so physisch von Ihren Hauptarbeitsgeräten getrennt ist und in der Regel mehr Berechtigungen braucht um diesen zu verlassen.

* **Befehl zur Installation:** `curl -fsSL https://openclaw.ai/install.sh | bash`. Das Kommando installiert auch alle Voraussetzungen wie z.B. Node.js. Für macOS steht aber auch ein Dowload als Universal Binary zur Verfügung.
* **Wichtig:** Richten Sie einen separaten User-Account ohne sudo-Rechte für den Bot ein!

**2\. Das Gold-Standard-Setup - Docker Container**

Um die Systemsicherheit zu gewährleisten, sollte OpenClaw (zusätzlich) in einem Container laufen. Hier mal ein Beispiel für eine docker-compose.yml:

```yaml
services:
  moltbot:
    image: moltbot/moltbot:latest
    container_name: moltbot-gateway
    restart: unless-stopped
    ports:
      - "18789:18789"
      - "18790:18790"
    environment:
      - CLAWDBOT_GATEWAY_TOKEN=${CLAWDBOT_GATEWAY_TOKEN}
    volumes:
      - clawdbot_config:/home/node/.clawdbot
      - clawdbot_workspace:/home/node/clawd
volumes:
  clawdbot_config:
  clawdbot_workspace:
```

Wer es an dieser Stelle noch etwas sicherer haben möchte (Docker Kentnisse vorausgesetzt) kann dem Container auch noch mit `cap_drop` unnötige Kernel-Privilegien entziehen und ihm ein eigenes Netzwerk spendieren um auch darüber etwas mehr Kontrolle zu haben.

## Fazit und Strategische Handlungsempfehlung

OpenClaw ist ein beeindruckendes Beispiel dafür, wie der Agentic Workplace Realität wird. Aktuell ist das aber eher ein Werkzeug für "Power-User" oder Entwickler, die erstens wissen was sie tun und zweitens wie man eine Sandbox baut und solche Tools isoliert.

### Meine Empfehlung für CIOs und CISOs

1. **Experimentieren, aber isolieren:** Nutzen Sie dedizierte Hardware oder isolierte VMs. Ein Docker Container auf meinem produktiven System, wenn er nicht richtig konfiguriert wurde, ist **keine** Isolation!
2. **Datenklassifizierung:** Überlegen Sie genau, welche Verzeichnisse Sie mounten. Ein Zugriff auf ~/.ssh oder ~/.aws z.B. sollte absolut tabu sein.
3. **Menschliche Kontrolle:** Aktivieren Sie in den Einstellungen die Bestätigungspflicht für kritische Tools (z.B. Shell-Ausführungen).

OpenClaw zeigt uns die Zukunft aber wir müssen sicherstellen, dass wir in dieser Zukunft noch die Kontrolle über unser Terminal behalten. Wenn Sie das Thema interessiert, lesen Sie auch meine Artikel [Der Agentic Workspace][2] oder [Trash in, trash out][3] zu diesem Thema.

Wie sehen Ihre Erfahrungen aus? Haben Sie dem Hummer schon die Schlüssel gegeben, oder überwiegt die Skepsis? Loggen sie sich bei Github ein und lassen Sie uns in den Kommentaren diskutieren.

[1]: https://openclaw.ai
[2]: https://thomas-krampe.com/posts/der-agentic-workplace/
[3]: https://thomas-krampe.com/posts/trash_in_trash_out/
