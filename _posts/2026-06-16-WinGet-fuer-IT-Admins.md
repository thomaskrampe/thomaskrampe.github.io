---
title: WinGet für IT-Admins
date: 2026-06-16 14:43:00 +/-0000
categories: [Tools, Windows]
tags: [software,windows,paketieren,compliance] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/ad2bac2b-a565-4366-8463-51313137ac0f.png
---

## Das Tool, das die meisten übersehen

Wer im Mittelstand für die Softwareverteilung zuständig ist, kennt das Feld gut: PDQ Deploy hier, Chocolatey dort, vielleicht noch ein alter SCCM-Unterbau und dazwischen Intune, das seit zwei Jahren ausgebaut werden soll. Und sind wir mal ehrlich, die ganzen Tools von Drittanbietern sehe ich auch noch in Hülle und Fülle. Ich selbst zum Beispiel, nutze seit Jahren bei vielen Kunden das Tool LoginAM (fka. Automation Machine) meines Arbeitgebers. Jedes dieser Werkzeuge hat seine Berechtigung und keines davon verschwindet von selbst (warum auch).

Linux Benutzer lachen, wenn ich von Softwareverteilung spreche. Was für die UX Fraktion schon immer ein Standard ist, liefert Microsoft seit Windows 10 1809 ebenfalls mit einem Paketmanager direkt im Betriebssystem, der für viele Szenarien ausreicht und nichts kostet. WinGet soll dabei nicht als Ersatz für alles, was bereits funktioniert sein. Aber als guter erster Schritt für Umgebungen, die gerade anfangen, Deployment zu strukturieren, und für alle, die den Drittanbieter-Overhead reduzieren wollen.

## Was WinGet ist und was nicht

WinGet wird von Microsoft als Teil des **App Installers** bereitgestellt und ist auf Windows 11, modernen Windows-10-Versionen (ab 1809) und Windows Server 2025 damit meist bereits direkt verfügbar. Auf älteren Systemen lässt sich WinGet über den Microsoft Store oder als direkten Download nach installieren. [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/windows/package-manager/winget/)

Im Kern macht WinGet das, was man von einem Paketmanager auch erwartet. WinGet sucht Software in einem zentralen Repository (dem winget-pkgs-Katalog auf GitHub mit derzeit über 7.000 Paketen) und installiert sie reproduzierbar, ohne dass jemand einen Installer manuell durchklicken muss.

```powershell
winget search vscode
winget install Microsoft.VisualStudioCode
winget upgrade --all
```

Das ist die Kurzversion und reicht für den Anfang, aber dann wäre der Artikel bereits hier zu Ende. Für den echten Admin-Alltag wird es jetzt aber noch etwas interessanter.

## Updates ohne Drittanbieter-Tool

Der häufigste Einstiegspunkt in WinGet und eine echte Arbeitserleichterung für Admins ist nicht die Installation von Anwendungen, sondern das Update-Management. Ein einziger Befehl aktualisiert alle installierten Anwendungen, die WinGet kennt:

```powershell
winget upgrade --all --silent --accept-package-agreements --accept-source-agreements
```

Das lässt sich als geplanter Task oder Intune-Remediation-Skript ausrollen. Kein Agent, keine Lizenz, keine externe Abhängigkeit. Für Umgebungen, in denen Drittanbieter-Software wie 7-Zip, VLC, Notepad++ oder Firefox regelmäßig gepflegt werden muss, ist das oft völlig ausreichend. Ich sage in solchen Situationen oft: "*Manchmal ist ausreichend einfach gut genug.*"

Für den echten Einsatz habe ich hier mal ein etwas robustes PowerShell-Skript mit entsprechender Fehlerbehandlung sowie Logging vorbereitet.

```powershell
#Requires -RunAsAdministrator
<#
.SYNOPSIS
    Aktualisiert alle WinGet-Pakete mit Fehlerbehandlung und Logging.
.NOTES
    Geeignet für geplante Tasks und Intune-Remediationen.
    Logs für die Fehlersuche unter: C:\ProgramData\WinGet-Updates
#>

param(
    [string]$LogDir = "C:\ProgramData\WinGet-Updates"
)

$ErrorActionPreference = 'Stop'
$scriptStartTime = Get-Date

# Log-Verzeichnis anlegen
if (-not (Test-Path -LiteralPath $LogDir)) {
    New-Item -ItemType Directory -Path $LogDir -Force | Out-Null
}

$logFile = Join-Path $LogDir ("winget-update-{0:yyyy-MM-dd}.log" -f (Get-Date))

# Logging-Funktion
function Write-Log {
    param(
        [string]$Message,
        [ValidateSet('INFO','WARN','ERROR')]
        [string]$Level = 'INFO'
    )
    $entry = "[{0}] [{1}] {2}" -f (Get-Date -Format 'yyyy-MM-dd HH:mm:ss'), $Level, $Message
    Add-Content -Path $logFile -Value $entry -Encoding UTF8 -ErrorAction Continue
    Write-Host $entry
}

Write-Log "=== WinGet Update-Lauf gestartet ===" "INFO"

# WinGet-Verfügbarkeit prüfen
$winget = Get-Command winget.exe -ErrorAction SilentlyContinue
if (-not $winget) {
    Write-Log "FEHLER: winget.exe nicht gefunden. Überprüfen Sie die App Installer-Installation oder nutzen Sie 'Add-AppxPackage -RegisterByFamilyName -MainPackage Microsoft.DesktopAppInstaller_8wekyb3d8bbwe'." "ERROR"
    exit 1
}

Write-Log "WinGet gefunden: $($winget.Source)" "INFO"
Write-Log "WinGet Version: $(winget --version)" "INFO"

# Administrator-Prüfung (bereits durch #Requires, aber für Logging hilfreich)
$isAdmin = [bool]([System.Security.Principal.WindowsIdentity]::GetCurrent().Groups -match "S-1-5-32-544")
if (-not $isAdmin) {
    Write-Log "WARNUNG: Skript läuft nicht als Administrator. Einige Updates könnten fehlschlagen." "WARN"
}

# Update-Lauf mit Retry-Logik
$maxRetries = 3
for ($attempt = 1; $attempt -le $maxRetries; $attempt++) {
    Write-Log "Starte Update-Lauf $attempt von $maxRetries..." "INFO"

    try {
        & winget upgrade --all `
            --silent `
            --accept-package-agreements `
            --accept-source-agreements `
            --verbose-logs `
            --log "$logFile"

        $exitCode = $LASTEXITCODE

        if ($exitCode -eq 0) {
            Write-Log "Upgrade erfolgreich abgeschlossen (Exit Code: 0)." "INFO"
            $duration = (Get-Date) - $scriptStartTime
            Write-Log "Laufzeit: $($duration.TotalSeconds) Sekunden" "INFO"
            exit 0
        }

        Write-Log "WinGet endete mit Exit-Code $exitCode. Details in der Logdatei oder mit 'winget error $exitCode' einsehen." "WARN"

        if ($attempt -lt $maxRetries) {
            Write-Log "Wiederhole in 10 Sekunden..." "INFO"
            Start-Sleep -Seconds 10
        }
    }
    catch {
        Write-Log "Fehler bei winget-Ausführung: $_" "ERROR"
        if ($attempt -lt $maxRetries) {
            Start-Sleep -Seconds 10
        }
    }
}

Write-Log "Maximale Anzahl an Wiederholungen erreicht. Abbruch." "ERROR"
exit 1
```

Warum jetzt soviel Skript statt eines Einzeilers?

- **Defensives Exit-Code-Handling**: Ich verlasse mich nicht auf spezifische Codes, stattdessen loggen und prüfen
- **`--verbose-logs`**: Für Troubleshooting Schritt 1 nutzen
- **Retry-Logik**: Behandelt flüchtige Netzwerkfehler, die ja immer mal passieren können
- **Pfad-Validierung**: Log-Verzeichnis wird geprüft und ggf. vorher angelegt
- **WinGet-Verfügbarkeitsprüfung**: Mit Hinweis auf `Add-AppxPackage` wenn nicht vorhanden
- **Version-Tracking**: Protokolliert WinGet-Version für das Debugging

>**Bitte nicht als universal "Production-Ready" sehen**, sondern als belastbarer Ausgangspunkt für Validierung in Ihrer Umgebung.
{: .prompt-tip }

## Intune-Integration Win32-Apps via WinGet

Microsoft dokumentiert offiziell die Integration von **Microsoft Store Apps (new)** in Intune, inklusive Win32-Store-Apps. Ein praxisorientierter Weg für verwaltete Umgebungen ist auch, eigene Win32-App-Pakete mit PowerShell-Installationsskripten zu bauen, die WinGet nutzen. Das reduziert Paketierungsaufwand, wenn Sie Paket-IDs, Detection Rules und Ausführungskontext validieren. [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/intune/app-management/deployment/add-microsoft-store)

### Installation und Deinstallation mit Fehlerbehandlung

Das folgende Skript ist Production-Ready für Intune Win32-Apps:

**Install-Skript (install-app.ps1):**

```powershell
#Requires -RunAsAdministrator
<#
.SYNOPSIS
    Installiert ein Paket via WinGet für Intune.
.PARAMETER PackageId
    Die exakte WinGet Package ID (z.B. "Git.Git", "Microsoft.VisualStudioCode").
    Mit 'winget search <name>' ermitteln.
.NOTES
    Exit-Codes: 0 = Erfolg, alles andere = Fehler (an Intune zurückgeben).
#>

param(
    [Parameter(Mandatory = $true)]
    [string]$PackageId
)

$logDir = "C:\ProgramData\Intune\WinGet"
if (-not (Test-Path -LiteralPath $logDir)) {
    New-Item -ItemType Directory -Path $logDir -Force | Out-Null
}

$logFile = Join-Path $logDir ("install-{0:yyyyMMdd-HHmmss}.log" -f (Get-Date))

try {
    Write-Host "[$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')] Installiere: $PackageId"
    Add-Content -Path $logFile -Value "[$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')] Start: $PackageId" -Encoding UTF8

    & winget install `
        --id $PackageId `
        --exact `
        --scope machine `
        --silent `
        --accept-package-agreements `
        --accept-source-agreements `
        --verbose-logs `
        --log $logFile

    $exitCode = $LASTEXITCODE

    if ($exitCode -eq 0) {
        Write-Host "[$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')] Installation erfolgreich: $PackageId"
        Add-Content -Path $logFile -Value "[$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')] Erfolg (Exit Code: 0)" -Encoding UTF8
        exit 0
    }

    Write-Host "[$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')] Installation fehlgeschlagen (Exit Code: $exitCode)"
    Write-Host "Details siehe: $logFile"
    Add-Content -Path $logFile -Value "[$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')] Fehler (Exit Code: $exitCode)" -Encoding UTF8
    exit $exitCode
}
catch {
    Write-Host "[$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')] FEHLER: $_"
    exit 1
}
```

**Uninstall-Skript (uninstall-app.ps1):**

```powershell
#Requires -RunAsAdministrator

param(
    [Parameter(Mandatory = $true)]
    [string]$PackageId
)

$logDir = "C:\ProgramData\Intune\WinGet"
if (-not (Test-Path -LiteralPath $logDir)) {
    New-Item -ItemType Directory -Path $logDir -Force | Out-Null
}

$logFile = Join-Path $logDir ("uninstall-{0:yyyyMMdd-HHmmss}.log" -f (Get-Date))

try {
    Write-Host "[$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')] Deinstalliere: $PackageId"
    Add-Content -Path $logFile -Value "[$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')] Start: $PackageId" -Encoding UTF8

    & winget uninstall `
        --id $PackageId `
        --exact `
        --silent `
        --accept-source-agreements `
        --verbose-logs `
        --log $logFile

    $exitCode = $LASTEXITCODE

    if ($exitCode -eq 0) {
        Write-Host "[$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')] Deinstallation erfolgreich: $PackageId"
        exit 0
    }

    Write-Host "[$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')] Deinstallation endete mit Code: $exitCode"
    Write-Host "Details siehe: $logFile"
    exit $exitCode
}
catch {
    Write-Host "[$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')] FEHLER: $_"
    exit 1
}
```

### Intune-Konfiguration

Wichtige Voraussetzungen:

- PowerShell-Execution-Policy muss `-ExecutionPolicy Bypass` zulassen (nötig für Intune-Skripting)
- Paket-IDs sollten mit `winget search <name>` ermittelt werden [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/windows/package-manager/winget/install)
- Detection Rule muss paketspezifisch validiert sein

**Detection Rule Beispiel** (PowerShell-Skript - Git):**

```powershell
# Überprüft ob Git installiert ist
# Paketspezifisch validiert und mehrfach abgesichert

$appPath = "C:\Program Files\Git\bin\git.exe"
$regPath = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\Git_is1"
$displayVersion = $null

try {
    $regItem = Get-ItemProperty -Path $regPath -ErrorAction SilentlyContinue
    if ($regItem -and $regItem.DisplayVersion) {
        $displayVersion = $regItem.DisplayVersion
    }
}
catch { }

if ((Test-Path -LiteralPath $appPath) -or $displayVersion) {
    Write-Host "Git is installed (Version: $displayVersion)"
    exit 0
}

Write-Host "Git not found"
exit 1
```

**In Intune konfigurieren:**

- **Install Command:** `powershell.exe -ExecutionPolicy Bypass -File install-app.ps1 -PackageId "Git.Git"`
- **Uninstall Command:** `powershell.exe -ExecutionPolicy Bypass -File uninstall-app.ps1 -PackageId "Git.Git"`
- **Detection Rule:** PowerShell-Skript (paketspezifisch)

>**Wichtig:** Detection Rules sind **paketspezifisch**. Der gezeigte Pfad ist für Git.Git validiert, aber jedes Paket kann an anderen Orten installieren oder andere Registry-Keys nutzen. Vor dem Rollout also immer prüfen.
{: .prompt-warning }

### Wichtige Hinweise zu Intune und WinGet

1. **Execution Policy**: In Intune müssen Sie `-ExecutionPolicy Bypass` verwenden, da die Standard-Policy Skriptausführung blockiert
2. **Exit-Codes**: Intune bewertet den Exit-Code. 0 = Erfolg, alles andere = Fehler. Das Skript muss daher die korrekten Exit-Codes setzen
3. **Package-ID Eindeutigkeit**: Immer `winget search <name>` nutzen, um die exakte Package ID zu ermitteln. Ähnliche Pakete (z.B. `Git.Git` vs. `Git.Git.LFS`) können zu fehlgeschlagenen Installationen führen
4. **Logging**: Beide Skripte schreiben nach `C:\ProgramData\Intune\WinGet\` für Debugging

Für standardisierte Software, die im winget-Katalog vorhanden ist, reduziert das den Paketierungsaufwand erheblich. Kein IntuneWinAppUtil, keine Testschleife für den Installer nötig.

## Eigene Pakete und private Repositories

Wer eigene Software oder Lizenzsoftware verteilen möchte, die nicht im öffentlichen Katalog liegt, kann ein privates WinGet-Repository betreiben. Das ist ein REST-kompatibler Dienst, den WinGet als zusätzliche Quelle einbinden kann.

Microsoft stellt dafür die `winget-cli-restsource`-Referenzimplementierung bereit, die sich auf Azure Functions oder einem eigenen Server betreiben lässt. Für kleinere Umgebungen reicht oft eine einfachere Lösung: ein lokales Verzeichnis mit Manifest-Dateien, das als Offline-Quelle eingebunden wird.

### Lokale Repository einrichten

Für private Quellen muss die Quelle korrekt vorindiziert und strukturiert sein. Das ist übrigens nicht trivial. Microsoft dokumentiert die Anforderungen unter "Quellen" und "Repositories". [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/windows/package-manager/winget/source)

Nachfolgend mal ein Beispiel:

```powershell
# Lokale Quelle hinzufügen (mit Validierung)
param(
    [string]$SourcePath = "\\server\winget-repo",
    [string]$SourceName = "intern"
)

# Pfad-Verfügbarkeit prüfen
if (-not (Test-Path -LiteralPath $SourcePath)) {
    Write-Error "Fehler: Quelle $SourcePath existiert nicht oder ist nicht erreichbar."
    exit 1
}

Write-Host "Validiere Repository-Struktur in $SourcePath..."

# Für Microsoft.PreIndexed.Package: index.db muss vorhanden und aktuell sein
$indexPath = Join-Path $SourcePath "index.db"
if (-not (Test-Path -LiteralPath $indexPath)) {
    Write-Warning "index.db nicht gefunden. Repository muss mit Microsoft.WinGet.RestSource oder äquivalentem Tool indiziert werden."
    Write-Warning "Siehe: https://learn.microsoft.com/en-us/windows/package-manager/winget/source"
}

# Quelle hinzufügen
try {
    winget source add `
        --name $SourceName `
        --arg $SourcePath `
        --type "Microsoft.PreIndexed.Package" `
        --accept-source-agreements
    
    if ($LASTEXITCODE -eq 0) {
        Write-Host "✓ Quelle '$SourceName' erfolgreich hinzugefügt: $SourcePath"
        winget source list
    }
    else {
        Write-Error "Fehler beim Hinzufügen der Quelle (Exit Code: $LASTEXITCODE)"
        exit $LASTEXITCODE
    }
}
catch {
    Write-Error "Fehler: $_"
    exit 1
}
```

>**Hinweis:** Lokale Sources müssen korrekt strukturiert und indiziert sein, um zu funktionieren. Das ist ist nicht trivial, bitte unbedingt die Microsoft-Dokumentation zu Repositories dazu lesen. Es passiert jetzt nichts schlimmes, aber es geht dann nur einfach nicht.
{: .prompt-tip }

### Manifest-Struktur

Die Manifest-Dateien sind YAML basiert und folgen einem klar definierten Schema. Hier mal ein Beispiel für eine interne Anwendung:

```yaml
# manifests/i/Internal/MyApp/1.0.0/Internal.MyApp.yaml
PackageIdentifier: Internal.MyApp
PackageVersion: 1.0.0
PackageName: My Company Application
Publisher: Internal IT
PackageUrl: https://internal.company.com/myapp
License: Proprietary
ShortDescription: Interne Geschäftsanwendung
Installers:
  - Architecture: x64
    InstallerType: msi
    InstallerUrl: https://server/winget-repo/installers/myapp-1.0.0.msi
    InstallerSha256: [SHA256-HASH DER DATEI]
    InstallerSwitches:
      Silent: /quiet
      SilentWithProgress: /quiet
```

**Wichtig:**

- Die Manifest-Datei muss sich in der Struktur `manifests/[Initialen]/[Publisher]/[AppName]/[Version]/` befinden
- WinGet erwartet dort HTTPS-URL, keine UNC-Pfade.
- Der SHA256-Hash ist erforderlich (mit `Get-FileHash` generieren)
- Die `index.db` muss mit dem WinGet-Index-Generator aktualisiert werden

Einmal erstellt, lassen sich Manifeste natürlich Versionieren und in ein bestehendes Git-Repository einpflegen, quasi Infrastructure as Code für die Softwareverteilung.

## Wo WinGet an Grenzen stößt

Eines ist hier besonders wichtig, WinGet ist kein vollwertiger Ersatz für Intune, SCCM oder PDQ in komplexen Umgebungen. Wer folgendes braucht, kommt mit WinGet allein nicht weit:

- **Komplexe Abhängigkeiten**: Wenn Paket A vor Paket B installiert sein muss und beide Parameter voneinander abhängen, ist ein dediziertes Tool besser geeignet. WinGet verwaltet keine Abhängigkeitsketten.
- **Granulares Reporting & Compliance**: WinGet liefert keine zentralisierte Übersicht, welche Geräte welche Version haben. Intune oder ein dediziertes Inventar-Tool bleibt notwendig für Compliance-Audits.
- **MSI-Transforms und Konfigurationsdateien**: Anwendungen, die beim Deployment individuelle Konfigurationsdateien oder MST-Dateien benötigen, erfordern klassisches Packaging.
- **Ältere Windows-Versionen**: Vor Windows 10 1809 gibt es keinen nativen WinGet-Support  (für Windows 7/8.1 ist vielleicht Chocolatey eine Alternative).
- **Privilegierte Installationen**: WinGet auf Benutzer-Ebene kann nur User-Scope-Installationen durchführen. System-Scope benötigt Administrator-Rechte.

## Best Practices und erweiterte Tipps

### 1. Paket-IDs exakt recherchieren

`winget search` zeigt oft mehrere Ergebnisse. Nutzen Sie `--exact` beim Install und Uninstall. [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/windows/package-manager/winget/install)

```powershell
# Suche nach "Git" — zeigt mehrere Treffer
winget search git

# Ausgabe beispielsweise:
# Name                   Id             Version
# Git                    Git.Git        2.45.0
# Git Credential Helper  Git.Git...     (weitere Varianten)
# Git LFS                Git.Git.LFS    3.4.0

# RICHTIG: Exakte ID mit --exact und --id
winget install --id Git.Git --exact --scope machine

# Auch bei Uninstall mit --exact arbeiten
winget uninstall --id Git.Git --exact
```

**Warum --exact?** Verhindert Mehrdeutigkeiten und stellt sicher, dass die korrekte ID installiert wird. Microsoft dokumentiert `--exact` auch als Best Practice. [\[learn.microsoft.com\]](https://learn.microsoft.com/en-us/windows/package-manager/winget/install)

### 2. Timeout und langsame Verbindungen handhaben

Für Clients mit einer schlechten Verbindung: `--include-unknown` entfernen, das beschleunigt die Updates etwas (das ist jetzt aber kein Speed Parameter):

```powershell

# Vollständig, aber langsamer (Standard)
winget upgrade --all --silent --include-unknown

# Schneller, aber weniger umfassend
winget upgrade --all --silent
```

### 3. PowerShell Execution Policy für Intune

Für Intune-Remediationen ist `-ExecutionPolicy Bypass` erforderlich, weil die Device-Policy Skriptausführung blockiert:

```powershell
# Intune Win32-App Install Command:
powershell.exe -ExecutionPolicy Bypass -File install.ps1 -PackageId "Microsoft.VisualStudioCode"

# NICHT verwenden: funktioniert nicht in Intune
powershell.exe -File install.ps1
```

### 4. Logging optimieren

Für Troubleshooting detailliertes Logging einschalten:

```powershell
# WinGet mit Verbose-Log
winget upgrade --all --silent --log "C:\Logs\winget-verbose.log"

# Im Skript: Log-Datei auslesen und auf Fehler prüfen
$logContent = Get-Content "C:\Logs\winget-verbose.log" -Tail 50
if ($logContent -match "error|failed") {
    Write-Warning "Fehler im WinGet-Log erkannt"
}
```

### 5. Intune-Remediationen mit Compliance-Check

Eine vollständige Remediation sollte die Compliance prüfen, das ist paketspezifisch und kann über Registry, Dateiexistenz oder `Get-AppPackage` erfolgen:

```powershell
# compliance-check.ps1 — wird von Intune ausgeführt
# Beispiel für Visual Studio Code (WinGet: Microsoft.VisualStudioCode)

# Methode 1: Datei-Existenz prüfen
$appPath = "C:\Users\*\AppData\Local\Programs\Microsoft VS Code\Code.exe"
if (Test-Path -LiteralPath $appPath) {
    Write-Host "VS Code installiert"
    exit 0
}

# Methode 2: Registry prüfen
$regPath = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*"
$appFound = Get-ItemProperty -Path $regPath -ErrorAction SilentlyContinue | 
    Where-Object { $_.DisplayName -like "*Visual Studio Code*" }

if ($appFound) {
    Write-Host "VS Code gefunden (Version: $($appFound.DisplayVersion))"
    exit 0
}

# Nicht gefunden — Remediation erforderlich
Write-Host "VS Code nicht gefunden"
exit 1
```

>**Wichtig:** Jedes Paket hat andere Installationspfade und Registry-Einträge. Validieren Sie die Pfade **vor** dem Rollout für Ihre spezifischen Pakete!
{: .prompt-warning }

## Bewertung

Ich mag ja immer ein Fazit, hier möchte ich das aber mal Bewertung nennen. WinGet ist sicherlich kein Allheilmittel, aber ein durchaus ernstzunehmendes Werkzeug, das in vielen Umgebungen helfen kann. Damit meine ich allerdings nicht *"besser als nichts"*, denn das würde dem Tool nicht gerecht werden. Für Update-Automatisierung und schnelles Software-Deployment via Intune ohne klassischen Packaging-Overhead ist es ein richtig gutes Tool, wenn ein paar Grundregeln wie z.B. Paket-IDs, Detection Rules, Exit-Codes und Ausführungskontext in Ihrer Umgebung zu validieren beachtet werden.

### Zusammengefasst

| Szenario                                 | WinGet geeignet? | Hinweis                                           |
| ---------------------------------------- | ---------------- | ------------------------------------------------- |
| Update-Automatisierung Standard-Software | Ja               | Getestet und skalierbar                           |
| Schnelles Deployment ohne Packaging      | Ja               | Mit `--exact`, `--scope machine`, Detection Rules |
| Interne bzw. Lizenzsoftware              | Eingeschränkt    | Private Repos müssen korrekt indiziert sein       |
| Pakete mit starken Abhängigkeiten        | Nein             | WinGet verwaltet keine Abhängigkeitsketten        |
| Windows 7/8.1 Support                    | Nein             | Chocolatey ist hier eine Alternative              |

**Alle Skripte in diesem Artikel sind erstmal nur als Beispiele zu sehen:** Vor einem produktivem Rollout sollten Sie immer die Paket-IDs, Detection Rules, Exit-Code-Verhalten und Ausführungskontext in Ihrer Umgebung validieren und natürlich Skripte aus "irgendwelchen" Quellen (auch nicht von mir) nicht ungetestet einsetzen. Aber das ist eine andere Geschichte.

Haben Sie Fragen zur Softwareverteilung oder Paketierung in Ihrer Umgebung oder möchten Sie WinGet in Kombination mit Intune evaluieren? Meine Kollegen und ich helfen gerne bei der Planung und Implementierung, kontaktieren Sie mich einfach über [LinkedIn](https://www.linkedin.com/in/thomaskrampe) oder per [E-Mail](mailto:t.krampe@previder.de).
