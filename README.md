<div align="center">

# ⚡ Optimisation des services Windows

### Mode prudent — Sauvegarde avant modification

[![README](https://img.shields.io/badge/🏠_README-Accueil-2563EB?style=for-the-badge)](README.md)
[![Internet](https://img.shields.io/badge/🌐_Internet-Guide-16A34A?style=for-the-badge)](internet.md)
[![Services](https://img.shields.io/badge/⚙️_Services-Optimisation-F59E0B?style=for-the-badge)](services.md)

---

**Windows • PowerShell • Services • Performance • Sauvegarde**

</div>

---

## 🎯 Objectif

Ce script PowerShell permet de modifier certains **services Windows optionnels** afin de limiter les services fonctionnant inutilement en arrière-plan.

Le script adopte une approche prudente :

```text
Sauvegarde des services
        ↓
Détection du service
        ↓
Arrêt du service
        ↓
Passage en mode Manuel
        ↓
Redémarrage du PC
```

> 🛡️ Une sauvegarde de la configuration actuelle des services est créée avant toute modification.

---

## ⚠️ Important

Exécutez le script uniquement si vous comprenez les modifications effectuées.

Certains services peuvent être utiles selon votre configuration :

- 🖨️ Imprimante
- 🎮 Xbox et jeux Microsoft
- 🗺️ Cartes Windows
- ☁️ Synchronisation Microsoft
- 📊 Diagnostic Windows
- 🖥️ Logiciels Adobe
- 🌐 Microsoft Edge
- 🔄 Google Update

> ⚠️ Le script ne désactive pas définitivement les services sélectionnés. Il configure leur démarrage en mode `Manual`.

---

# 🛡️ Sauvegarde automatique

Avant toute modification, le script crée le fichier :

```text
services_backup.csv
```

Le fichier est enregistré sur le Bureau de l'utilisateur :

```text
C:\Users\VotreUtilisateur\Desktop\services_backup.csv
```

Il contient :

| Information | Description |
|---|---|
| `Name` | Nom technique du service |
| `DisplayName` | Nom affiché dans Windows |
| `Status` | État actuel du service |
| `StartType` | Type de démarrage |

---

# 🚀 Script PowerShell

> 🛡️ Lancez **PowerShell en tant qu'administrateur**.

```powershell
# ============================================================
# Optimisation des services Windows - Mode prudent
# Exécuter PowerShell en tant qu'administrateur
# ============================================================

$backupPath = "$env:USERPROFILE\Desktop\services_backup.csv"

# ------------------------------------------------------------
# Sauvegarde de la configuration actuelle des services
# ------------------------------------------------------------

Get-Service |
    Select-Object Name, DisplayName, Status, StartType |
    Export-Csv `
        -Path $backupPath `
        -NoTypeInformation `
        -Encoding UTF8

Write-Host ""
Write-Host "Sauvegarde créée ici :" -ForegroundColor Green
Write-Host $backupPath -ForegroundColor Cyan
Write-Host ""

# ------------------------------------------------------------
# Services optionnels à configurer en démarrage Manuel
# ------------------------------------------------------------

$optionalServices = @(
    "AdobeARMservice",
    "AGMService",
    "AGSService",
    "gupdate",
    "gupdatem",
    "MicrosoftEdgeElevationService",
    "edgeupdate",
    "edgeupdatem",
    "OneSyncSvc",
    "XboxGipSvc",
    "XblAuthManager",
    "XblGameSave",
    "XboxNetApiSvc",
    "MapsBroker",
    "Fax",
    "RetailDemo",
    "RemoteRegistry",
    "WerSvc",
    "DiagTrack",
    "dmwappushservice",
    "WMPNetworkSvc",
    "Spooler"
)

# ------------------------------------------------------------
# Traitement des services
# ------------------------------------------------------------

foreach ($serviceName in $optionalServices) {

    $service = Get-Service `
        -Name $serviceName `
        -ErrorAction SilentlyContinue

    if ($service) {

        try {

            Stop-Service `
                -Name $serviceName `
                -Force `
                -ErrorAction SilentlyContinue

            Set-Service `
                -Name $serviceName `
                -StartupType Manual

            Write-Host "[OK] $serviceName -> Manuel" `
                -ForegroundColor Yellow

        }
        catch {

            Write-Host "[ERREUR] Impossible de modifier : $serviceName" `
                -ForegroundColor Red
        }

    }
    else {

        Write-Host "[ABSENT] Service introuvable : $serviceName" `
            -ForegroundColor DarkGray
    }
}

Write-Host ""
Write-Host "============================================" `
    -ForegroundColor Cyan

Write-Host "Optimisation terminée." `
    -ForegroundColor Green

Write-Host "Redémarrez votre ordinateur." `
    -ForegroundColor Green

Write-Host "============================================" `
    -ForegroundColor Cyan
```

---

# ▶️ Comment exécuter le script

## 1. Créer le fichier PowerShell

Créez un fichier nommé :

```text
optimize-services.ps1
```

Copiez le script PowerShell dans ce fichier.

---

## 2. Ouvrir PowerShell en administrateur

Dans le menu **Démarrer** :

```text
PowerShell
```

Puis :

```text
Clic droit
    ↓
Exécuter en tant qu'administrateur
```

---

## 3. Accéder au dossier du script

Exemple :

```powershell
cd "$env:USERPROFILE\Desktop"
```

---

## 4. Exécuter le script

```powershell
.\optimize-services.ps1
```

---

## 🔒 Erreur de stratégie d'exécution

Si PowerShell bloque temporairement le script, vous pouvez lancer :

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

Puis :

```powershell
.\optimize-services.ps1
```

> ℹ️ La portée `Process` applique le changement uniquement à la session PowerShell actuelle.

---

# 📋 Services concernés

| Service | Fonction |
|---|---|
| `AdobeARMservice` | Mise à jour Adobe |
| `AGMService` | Adobe Genuine Monitor |
| `AGSService` | Adobe Genuine Software |
| `gupdate` | Google Update |
| `gupdatem` | Google Update |
| `MicrosoftEdgeElevationService` | Service d'élévation Edge |
| `edgeupdate` | Mise à jour Edge |
| `edgeupdatem` | Mise à jour Edge |
| `OneSyncSvc` | Synchronisation Windows |
| `XboxGipSvc` | Accessoires Xbox |
| `XblAuthManager` | Authentification Xbox Live |
| `XblGameSave` | Sauvegardes Xbox |
| `XboxNetApiSvc` | Réseau Xbox Live |
| `MapsBroker` | Cartes hors connexion |
| `Fax` | Service Fax |
| `RetailDemo` | Mode démonstration Windows |
| `RemoteRegistry` | Registre à distance |
| `WerSvc` | Rapport d'erreurs Windows |
| `DiagTrack` | Télémétrie et diagnostic |
| `dmwappushservice` | Service de messages WAP |
| `WMPNetworkSvc` | Partage Windows Media Player |
| `Spooler` | Gestion des impressions |

---

# ⚠️ Services à surveiller

## 🖨️ Spooler

Le service :

```text
Spooler
```

est nécessaire pour l'impression.

Ne le modifiez pas si vous utilisez :

- une imprimante
- une imprimante PDF dépendante du Spooler
- un serveur d'impression

Vous pouvez supprimer cette ligne du script :

```powershell
"Spooler"
```

---

## 🔄 Services de mise à jour

Les services suivants concernent les mises à jour de logiciels :

```text
gupdate
gupdatem
edgeupdate
edgeupdatem
AdobeARMservice
```

Les passer en mode `Manual` peut modifier le comportement des mises à jour automatiques.

> 🔐 Vérifiez régulièrement les mises à jour de sécurité de vos logiciels.

---

# 🔍 Vérifier l'état d'un service

Exemple :

```powershell
Get-Service -Name Spooler
```

Pour afficher plus d'informations :

```powershell
Get-Service -Name Spooler |
    Select-Object Name, DisplayName, Status, StartType
```

---

# 🔄 Réactiver un service

Exemple avec le service d'impression :

```powershell
Set-Service -Name Spooler -StartupType Automatic
Start-Service -Name Spooler
```

Pour Google Update :

```powershell
Set-Service -Name gupdate -StartupType Automatic
Start-Service -Name gupdate
```

---

# 📊 Résultat attendu

Le terminal peut afficher :

```text
Sauvegarde créée ici :
C:\Users\User\Desktop\services_backup.csv

[OK] AdobeARMservice -> Manuel
[OK] gupdate -> Manuel
[OK] XboxGipSvc -> Manuel
[ABSENT] Service introuvable : Fax
[OK] DiagTrack -> Manuel

============================================
Optimisation terminée.
Redémarrez votre ordinateur.
============================================
```

---

# 💡 Recommandation

Ne désactivez pas tous les services Windows au hasard.

La méthode recommandée est :

```text
Identifier le service
        ↓
Comprendre son rôle
        ↓
Créer une sauvegarde
        ↓
Passer en Manuel
        ↓
Redémarrer
        ↓
Tester Windows
```

> ✅ Le mode `Manual` est généralement plus prudent qu'une désactivation systématique avec `Disabled`.

---

<div align="center">

## ⚡ Windows Services Optimizer

**Backup First • Optimize Carefully • Test After Restart**

[![Retour au README](https://img.shields.io/badge/⬅️_Retour-README-2563EB?style=for-the-badge)](README.md)
[![Guide Internet](https://img.shields.io/badge/🌐_Guide-Internet-16A34A?style=for-the-badge)](internet.md)

</div>
