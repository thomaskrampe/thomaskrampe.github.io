---
title: Sichere Automatisierung - Credentials in PowerShell verwalten
date: 2025-11-27 11:35:00 +/-0000
categories: [Microsoft, Automatisierung, Sicherheit]
tags: [how-to, automation, powershell] # TAG names should always be lowercase
image:
  path: https://picsur.myctx.net/i/a59e41e6-13e3-4e33-8bf8-4ceb5daf740d.png
---

## Einleitung - Die Herausforderung der Automatisierung

Hardcodierte Benutzernamen und Passwörter in Skripten stellen ein massives Sicherheitsrisiko dar ("Clear Text Passwords"). Während interaktive Skripte Anmeldedaten sicher via Get-Credential abfragen können, fehlt diese Möglichkeit bei automatisierten Aufgaben (z. B. im Task Scheduler).

Interaktive Skripte können natürlich zur Laufzeit die Anmeldedaten über `Get-Credential` vom Benutzer abfragen. Vorteil ist hier, dass die Daten nicht im Klartext im Skript hinterlegt werden müssen, für unbeaufsichtigt laufende Skripte funktioniert das leider so nicht. Automatisch ausgeführte Skripte benötigen einen Speicherort, an dem sie das Credential-Objekt sicher abrufen können. Idealerweise passiert das natürlich verschlüsselt.

Wie speichert man Credentials sicher, sodass ein Skript sie entschlüsseln kann, ein Unbefugter jedoch nicht? PowerShell bietet hierfür zwei Lösungswege.

Der erste und mit Abstand einfachste Weg, ist das speichern der Credentials in einer externen XML Datei.

```powershell
Get-Credential | Export-Clixml -Path "C:\temp\Credentials.xml"
```

Mit dem entsprechenden Import Kommando lassen sich die Credentials dann auch wieder in einem Skript nutzen.

```powershell
$Cred = Import-Clixml -Path "C:\temp\Credentials.xml"
Write-Host "Benutzer: " $Cred.UserName
Write-Host "Password: " $Cred.GetNetworkCredential().Password
```

**Vorteil:**

* Super einfach anzuwenden und (teilweise) verschlüsselt.

**Nachteil:**

* `Export-Clixml` serialisiert das Credential-Objekt, was es damit nur zu 50&% verschlüsselt. Da der UserName ein einfacher String ist, bleibt er in der exportierten Datei im Klartext lesbar. Das Password im Gegenzug ist ein SecureString und wird daher durch die DPAPI (Data Protection API von Windows) verschlüsselt. Damit kann nur der Benutzer der die Credentials exportiert hat und auch nur auf der gleichen Maschine auf der diese XML Datei exportiert wurde, das Passwort entschlüsseln.
* Das macht das Passwort in der XML Datei relativ sicher, ohne sich noch um eine komplexe Kryptografie kümmern zu müssen. Allerdings kann die XML Datei auch nicht mit anderen geteiltund genutzt werden.
* Ein weiterer Nachteil aus der Praxis ist, dass ich für jedes Credential Objekt eine eigene XML Datei anlegen muss oder vor einem Export alle benötigten Credentials in eine Hashtable schreiben muss. Eine weitere Verwaltung der Credentials ist nicht möglich.

```powershell
# Beispiel mit zwei Credentials
$cred1 = Get-Credential -Message "Bitte erstes Credential eingeben"
$cred2 = Get-Credential -Message "Bitte zweites Credential eingeben"

# In eine Hashtable speichern
$allCreds = @{
    "SQL" = $cred1
    "API" = $cred2
}

# Exportieren
$allCreds | Export-Clixml -Path "C:\temp\multiCredentials.xml"
```

In einem Skript lässt sich das dann wieder laden.

```powershell
$allCreds = Import-Clixml -Path "C:\temp\multiCredentials.xml"
$allCreds["SQL"].UserName
$allCreds["API"].GetNetworkCredential().Password
```

Etwas praktikabler geht es mit einem Passwort Vault, welcher für die lokale Nutzung von Microsoft auch in einem entsprechenden Secure Store Modul umgesetzt wurde.

Das **PowerShell SecretManagement Modul** bietet uns eine Abstraktionsschicht, um Benutzernamen und Passwörter sicher in einem Vault zu speichern und auch wieder sicher abzurufen und ist gerade für wiederkehrende Aufgaben (Task Scheduler) gedacht, da dort ja keine Benutzerinteraktion möglich ist.

Das Modul `Microsoft.PowerShell.SecretStore` bietet uns den eigentlich lokalen Vault und speichert dort die Credentials. Für die automatische Ausführung z.B. über den Task Scheduler (Non-Interactive) müssen wir den Vault so konfigurieren, dass er **kein** Master-Passwort zur Entsperrung verlangt, sondern den Kontext des ausführenden Benutzers (z.B. ein Service Account) nutzt. Standardmäßig wird der Vault sonst mit einem Master-Passwort geschützt, in diesem Fall lässt sich ein Skript natürlich wieder nur Interaktiv verwenden.

Ähnlich wie vorher, wird dieser Vault über die in Windows enthaltene DPAPI verschlüsselt und ist ausschließlich von dem Benutzer und der Maschine die den Vault erstellt haben nutzbar (AES bzw. TripleDES abhängig von der Windows Version).

```powershell
# Installation der beiden benötigten Module
Install-Module Microsoft.PowerShell.SecretManagement, Microsoft.PowerShell.SecretStore -Scope CurrentUser

# Registrierung des Vaults für den Task Scheduler
# WICHTIG: 'DoNotRequirePassword' nutzt DPAPI (User Context) zur Verschlüsselung
$VaultParams = @{
    DoNotRequirePassword = $true
}
Register-SecretVault -Name "AutoTaskVault" -ModuleName Microsoft.PowerShell.SecretStore -VaultParameters $VaultParams

# Speichern des Geheimnisses (z.B. API-Key oder Passwort)
$SecureString = Read-Host -AsSecureString "Bitte Passwort eingeben"
Set-Secret -Name "DbAccess" -Secret $SecureString -Vault "AutoTaskVault"
```

Im Skript, das dann vom Task Scheduler ausgeführt wird, sind keine Credentials sichtbar. Und auch der Vault auf dem entsprechenden System ist nicht so einfach kopierbar.

```powershell
# Abruf der Credentials zur Laufzeit
$Creds = Get-Secret -Name "DbAccess" -AsPSCredential -Vault "AutoTaskVault" -ErrorAction Stop

if ($Creds) {
    # Connect-SQLServer ...
} else {
    Write-Warning "Keine Credentials im Vault gefunden."
}
```

**Vorteile:**

* Skripte können damit auch einfach versioniert werden (Git), ohne Geheimnisse zu leaken.
* Der Backend-Speicher (Vault) kann getauscht werden, ohne das Skript zu ändern.
* Die Credentials lassen sich einfach verwalten. Es können sowohl zusätzliche Secrets hinzugefügt, aber auch Secrets gelöscht oder geändert werden.

```powershell
# Secrets auflisten (jeder Benutzer sieht dabei nur die eigenen Vaults bzw. Secrets)
Get-SecretVault
Get-Secret
Get-SecretInfo

# Secret löschen
Remove-Secret -Name "Dbaccess"

# Secret ändern - bestehendes Secret einfach überschreiben
Set-Secret -Name "Dbaccess" -Secret (Get-Credential)
```

**Nachteile & Risiken:**

* **DPAPI Abhängigkeit:** Da wir `DoNotRequirePassword` nutzen, basiert die Sicherheit rein auf dem Windows-Benutzerprofil. Wenn ein Angreifer diesen Account kompromittiert, hat er Zugriff auf den Vault. Jeder Prozess, der unter diesem Benutzer läuft, kann das Passwort auslesen. Fängt man sich Schadcode ein, der unter diesem User läuft, ist der Vault offen.
* **User-Kontext:** Der Task im Task Scheduler **muss** unter exakt demselben Account laufen, der auch den Vault erstellt hat. Der verschlüsselte Vault kann nur auf dem gleichen Rechner und unter dem gleichen Benutzerkonto wieder entschlüsselt werden.

## Vergleich der Lösungen

| Kriterium         | SecretManagement Modul                                          | Export-Clixml                                     |
| :---------------- | :-------------------------------------------------------------- | :------------------------------------------------ |
| Skalierbarkeit    | Sehr gut – geeignet für große Umgebungen und Automatisierung    | Gering – eher für Einzelplatz oder kleine Skripte |
| Typen von Secrets | Strings, SecureStrings, PSCredentials, komplexe Objekte         | PSCredentials und einfache Objekte                |
| Verwaltung        | Vollständig – Hinzufügen, Aktualisieren, Löschen, Auflisten     | Nur Export und Import möglich                     |
| Integration       | Nahtlos mit sicheren Vault und CI/CD-Pipelines                  | Keine native Integration mit externen Vaults      |
| Einrichtung       | Erfordert Installation des Moduls und Konfiguration eines Vault | Keine zusätzliche Einrichtung nötig               |
| Best Practice     | Für produktive Umgebungen und Automatisierung                   | Für einfache lokale Skripte oder Testzwecke       |

## Alternativen

1. **Nutzung eines Remote-Vaults:** Z.B. Azure Key Vault oder Hshicorp Vault (Empfohlen für Enterprise). Sicherer, da Access Policies und Audit Logs existieren sowie Zertifikats-Authentifizierung für den Zugriff möglich ist. Damit sind diese Vaults universell im Team nutzbar, benötigen aber auch Zugriff auf das Internet.
2. **Passwordmanager**: Diverse Passwort-Manager (z.B. KeePass) bieten auch eine API auf die mit PowerShell zugegriffen werden kann. Oft ist eine Implementierung aber deutlich komplexer als die vorher beschriebenen Varianten.
3. **GMSA (Group Managed Service Accounts):** Für Server-zu-Server-Kommunikation oft die sicherste Methode, da Windows das Passwort-Management komplett übernimmt (kein `Get-Secret` nötig). Für Skripte aber eher ungeeignet.

## Fazit

Nutzen Sie `Export-Clixml` nur für temporäre lokale Tests. Stellen Sie für alle produktiven Task-Scheduler-Aufgaben auf das SecretManagement-Modul um. Es ist zukunftssicherer, da Sie das Backend (z. B. Wechsel zu Azure KeyVault) später austauschen können, ohne Ihr Skript umschreiben zu müssen.

## Bonus
Die beiden PowerShell Module SecretStore und SecretManagement sind auch auf anderen Betriebssystemen wie z.B. macOS oder Linux verfügbar. Wer also wie ich seine Scripte auf macOS nutzt, kann auch dort wirksam verhindern, dass die Credentials im Script zu lesen sind.

Was denkt ihr, ist das ein nützlicher Weg oder habt ihr eine andere Methode?
