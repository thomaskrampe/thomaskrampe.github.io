---
title: Docker Compose Best Practices 
date: 2026-04-20 13:47:28 +/-0000
categories: [technologie, Container, DevOps]
tags: [docker,how-to,devops] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/7be1a8af-3897-4ccd-95f1-6b2a03043555.png
---

## 7 Best Practices, die euer Leben leichter machen

Ich habe heute mal meine ganzen Docker Container organisiert und dabei festgestellt, dass sich nicht alle meine Docker Compose Dateien an meine "eigenen" Best Practices halten. Ihr kennt das, mal schnell eine `compose.yml` von irgendwo heruntergeladen, ausgeführt und wenn es dann auch gleich funktonioniert, wird es auch unverändert so verwendet.

Die ganze Organisation hat jetzt etwas Zeit gekostet, weshalb ich beschlossen habe, meine persönlichen Best Practices aufzuschreibe und diese hier zu teilen.

## Chaotische Compose-Dateien

Viele heruntergeladene oder auch schnell "selbstgeschriebene" Compose Dateien sind ein reiner Flickenteppich. Über hartcodierte Passwörter, fehlende Limits und Services, die nur so semi-gut konfiguriert sind, ist meistens alles dabei. Das Ergebnis sind Ausfälle, Datenverluste und zum Teil endlose Debug-Sessions. Dabei sollte eine gute Compose-Datei als Teil von "Infrastructure as Code" einfach zu warten, skalierbar und sicher sein.

## Meine Best Practices um das zu erreichen

### 1. Versionierung

In den Docker Compose Dateien steht oft als erste Zeile die Version. Seit der Compose Specification (neuere Docker Compose-Plugin-Versionen) ist die Version nur noch informativ für Abwärtskompatibilität zu alten Dateien. Docker Compose verwendet automatisch immer das neueste Schema, ignoriert die Angabe und warnt bei obsoleten Versionen.

```yaml
version: "3.9"
services:
  app:
```

- **Vorteil ohne Version:** Einfachere Dateien, immer aktuelle Features, keine keine Warnungen.
- **Nachteil:** Bei sehr alten Docker-Versionen (pre-1.10) könnten Features fehlschlagen. Aber mal ganz ehrlich, habt ihr das wirklich noch?

**Meine Empfehlung:** In neuen Projekten einfach die Version entfernen.

### 2. Namen verwenden

Egal ob bei Containern, Volumes oder Netzwerken, solltet ihr immer aussagekräftige Namen verwenden. Das ist natürlich nur etwas für Menschen, aber es ist tatsächlich sehr hilfreich, wenn ihr z.B. mit `docker ps` einen bestimmten Container sucht oder in einem Tool darauf referenzieren wollt.

```yaml
services:
  db:
    image: postgres:15
    container_name: postgres
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - app-net
 
volumes:
  db_data:

networks:
  app-net:
  
```

### 3. Environment-Variablen immer verwenden

Das ist ein echter "must-have" Tip. Ich entdecke immer wieder Dateien auf GitHub in denen die Credentials (meist sogar funktionierende) hart in die compose.yaml geschrieben wurden. Gewöhnt euch an, kritische Werte wie z.B. Passwörter, Secrets, API Tokens usw. in `.env` Environment Dateien auszulagern. Wenn ihr euch nicht sicher seid was kritisch ist, nehmt alles raus und ersetzt es durch Variablen.

Beispiel `.env`File

```plaintext
DB_USER=superuser
DB_PASS=secretpassword
```

Variablen dann in der `compose.yaml` nutzen:

```yaml
  environment:
    - DB_USER=${DB_USER}
    - DB_PASS=${DB_PASS}
```

> **Aber Achtung:** Wenn ihr eure Projekte auf GitHub o.ä. bereitstellt oder hostet, achtet darauf `.env` in die lokale oder am besten globale `.gitignore` Datei aufzunehmen, sonst könntet ihr die Daten auch im Klartext in der `compose.yaml` lassen.
{: .prompt-warning }

Beispiel `.gitignore`:

```plaintext
# --- Geheimnisse & Lokale Konfiguration ---
.env
.env.local
config/local.yaml
```

Damit werden die aufgeführten Datei-Muster nicht in das Remote Repository hochgeladen.

> **Mein Tip**: Ich lege trotzdem eine Datei mit dem Namen `.env.example` an, in der ich die Variablen kurz beschreibe. Diese pushe ich dann in das Remote Repository. Ein Beispiel mit der echten `.env` Datei von oben würde dann so aussehen.
{: .prompt-tip }

```plaintext
DB_USER=<Hier DB Username eintragen>
DB_PASS=<Hier DB Password eintragen>
```  

Falls es ein öffentliches Repository ist, kann der Benutzer gleich sehen, was er für Variablen ausfüllen muss. Bei nur einer handvoll Variablen und sprechenden Namen sicher überflüssig, aber bei längeren Compose Dateien mit vielen Werten, ist das extrem hilfreich für den Nutzer.

Ich verwende z.B. für mein ganzes Homelab auch Github, allerdings in einem privaten Repository.

### 4. depends_on und Health Checks zusammen verwenden

Ich sehe oft `depends_on: db` in den Compose Dateien. Hier wird aber lediglich gewartet bis der Service gestartet ist, sobald der entsprechende DB Service läuft ist depends_on erfüllt. Aber Postgres zum Beispiel benötigt 10–30 Sekunden für die Initialisierung nach dem Start, der Service der dann basierend auf `depend_on` startet wird sicher fehlschlagen.

Der bessere Weg ist es, `depends_on` ausschließlich mit einem Health-Check wie `condition: service_healthy` zu kombinieren.

```yaml
services:
  app:
    depends_on:
      db:
        condition: service_healthy  # Warte auf Health Check!

  db:
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]  # Leichtgewichtig, real check
      start_period: 30s  # Init-Zeit geben
```

`condition: service_healthy` setzt natürlich einen Health-Check in allen Service voraus.

Ein einfacher Health-Check für Web Services könnte wie folgt aussehen:

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost"]
  interval: 30s
  timeout: 5s
  retries: 3        # Mehr Versuche für den Startup
  start_period: 40s # Manche Service brauchen etwas mehr Zeit zu starten
```

Unabhängig von `depends_on` solltet ihr für jeden Service in einer Compose Datei auch immer einen Health-Check einbauen. Gerade für Tools zur Verwaltung von Docker Containern (z.B. [Dockhand][1] oder [Portainer][2]) erleichtert es das Monitoring. Denn ein `docker ps` zeigt mir nur ob der Container gestartet ist, der Heath-Check zeigt mir ob der Service auch funktioniert (wenn der Health Check auch gut erstellt wurde).

### 5. Mehrere Dateien pro Umgebung

Gerade im CI/CD Umfeld sollte man die Basis-Konfiguration zentral halten und Umgebungen (Dev, Prod, Test) modular überschreiben.

Wie sieht die Datei-Struktur dafür aus:

- **docker-compose.yml** (Basis): Gemeinsame Services, Images, Volumes, Netzwerke.
- **docker-compose.dev.yml** (Entwicklung): Volumes für Hot-Reload, Debug-Ports, lokale Builds.
- **docker-compose.prod.yml** (Produktion): Resource-Limits, Secrets, Restart-Policies, keine Dev-Volumes.
- **.env**: Variablen

```plaintext
├── docker-compose.yml      # Basis: app + db
├── docker-compose.dev.yml  # Dev: Volumes + Ports
├── docker-compose.prod.yml # Prod: Limits + Secrets
└── .env                    # Variablen 
```

Docker Compose führt die Dateien dann sequentiell zusammen (Reihenfolge = Priorität):

- **Überschreiben:** Gleicher Service-Feld → Letzte Datei gewinnt (z. B. die Ports in Dev überschreibt Basis).
- **Erweitern:** Neue Felder/Services werden hinzugefügt (z. B. Limits werden nur in Prod eingefügt).
- **Automatisch:** docker-compose.override.yml wird immer mit docker-compose.yml gemerged (Dev-Standard).

**Beispiel:**

```yaml
# Dev: Basis + Dev-Overrides
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d

# Prod: Basis + Prod-Overrides
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d

# Nur Basis (z. B. Test)
docker compose up -d
```

> **Meine Empfehlung:** Immer auch mit entsprechenden .env-Files kombinieren (env_file: .env.dev).  Zum validieren `docker compose -f base.yml -f prod.yml config` verwenden. Das zeigt das finale Merge-Ergebnis und ist unverzichtbar für Debugging.
{: .prompt-tip }

### 6. Resource Limits setzen

Tatsächlich haben Docker-Container standardmäßig keine Limits, sie nutzen so viel CPU/Memory wie der Host bereitstellt. Limits z.B. in Multi-Service-Stacks schützen unseren Host und verhindern Downtimes in der Produktion durch fehlerhafte Services (z.B. Out-of-Memory). Bei Limits unterscheiden wir **Harte Limits** (`limits:`) die der Container nicht überschreiten darf und **Soft Limit** (`reservations:`) die Mindestressourcen garantieren. Beides sollte zusammen genutzt werden.

**Beispiel:**

```yaml
services:
  app:
    deploy:  
      resources:
        limits:
          cpus: '1.5'    # Max 1,5 CPU-Cores
          memory: 512M   # Max 512 MB RAM
        reservations:
          cpus: '0.5'    # Garantiert 0,5 Cores
          memory: 256M   # Garantiert 256 MB

```

> **Tip:** Limits und Reservierungen immer setzen, ohne Ausnahme und auch in Dev Umgebungen. Zum schauen, wo die Ressourcenfresser sind `docker stats` verwenden.
{: .prompt-tip }

```plaintext
CONTAINER ID   NAME                  CPU %     MEM USAGE / LIMIT     
0dc110f39313   authentik-postgres    4.22%     58.54MiB / 3.778GiB   
2bf9f531445d   authentik-redis       0.38%     6.766MiB / 256MiB 
dcaf6795a186   authentik-worker      0.15%     367.6MiB / 1GiB       
2bb1b19ce5da   authentik-server      0.66%     501.3MiB / 3.778GiB   
```

Zur Veranschaulichung mal meinen [Authentik][3] Container. Mein Host hat 4 GB RAM, authentik-postgres und authentik-server haben kein Limit und dürfen nehmen, was da ist. Im Gegensatz dazu ist authentik-worker auf 1 GB und authentik-redis auf 256 MB beschränkt.

Aber aufpassen, bei Reservierungen (garantierte Mindestressourcen) immer auch etwas Power für das Hostsystem übrig lassen.

### 7. init: true aktivieren

Wenn wir einen Compose-Stack mit `docker compose down` herunterfahren, soll dieser natürlich graceful heruntergefahren werden (Daten speichern, Connections schließen etc.). Stattdessen sendet Docker ein SIGKILL nach 10 Sekunden, als Ergebnis davon hängen oft Prozesse als Zombie Prozesse.

Das Problem ist, dass in Containern die PID 1 meist der App-Prozess (z. B. node app.js) ist. Dieser ist kein richtiger Init und handhabt Signale (SIGTERM, SIGINT) nicht korrekt. Was dann passiert sind:

- Zombie-Prozesse: Kind-Prozesse werden nicht richtig gestopped.
- Graceful Shutdown fehlt: Kein SIGTERM-Forwarding an die Kind-Prozesse.
- SIGKILL-Falle: Docker wartet 10s auf den Stopp, dann erfolgt ein Brutal-Kill.

In meinen Projekten passierte das regelmäßig, bis ich `init: true` verwendet habe.

Aktiviert wird das ganze mit nur einer Zeile:

```yaml
services:
  app:
    image: node:20-alpine
    init: true        # Das war's!
```

> **Meine Empfehlung:** `init: true` in jeden Service, gerade auch wenn mehrere Services in der Compose Datei erstellt werden. Spart Stunden an Wiederherstellungszeit, wenn ein Host aufgrund von Zombiprozessen neu gestartet werden muss und dabei Daten verloren gehen. In meinen Stacks seit 2 Jahren keine Zombie-Probleme mehr.
{: .prompt-tip }

Ich kann nur empfehlen, jede eurer Compose-Datei zu prüfen und Health-Checks sowie Limits nachzurüsten. In meinem Team und auch in meinem privaten Homelab hat das Ausfälle um bis zu 70 % gesenkt.

Haben Sie schon mal einen Compose-Fehler erlebt, der Prod lahmgelegt hat? Hinterlasst gern mal eure Erfahrung!

[1]: https://dockhand.pro/
[2]: https://www.portainer.io/
[3]: https://goauthentik.io/
