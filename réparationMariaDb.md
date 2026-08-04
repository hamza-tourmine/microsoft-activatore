# 🗄️ Guide de réparation MariaDB sous XAMPP

<p align="center">

<img src="https://img.shields.io/badge/MariaDB-10.4+-003545?style=for-the-badge&logo=mariadb&logoColor=white">
<img src="https://img.shields.io/badge/XAMPP-Compatible-FB7A24?style=for-the-badge&logo=xampp&logoColor=white">
<img src="https://img.shields.io/badge/Windows-10%20%7C%2011-0078D6?style=for-the-badge&logo=windows&logoColor=white">
<img src="https://img.shields.io/badge/Laravel-Compatible-FF2D20?style=for-the-badge&logo=laravel&logoColor=white">

</p>

---

> [!IMPORTANT]
>
> Ce guide explique **pas à pas** comment diagnostiquer, sauvegarder et réparer une installation **MariaDB/XAMPP** tout en conservant les bases de données de votre projet Laravel.

---

# 📚 Sommaire

- [Configuration](#-configuration)
- [Prérequis](#-prérequis)
- [Étapes de réparation](#-étapes-de-réparation)
- [Vérifications finales](#-vérifications-finales)
- [FAQ](#-faq)

---

# ⚙️ Configuration

| Élément | Valeur |
|----------|---------|
| 💻 Système | Windows |
| 🗄️ MariaDB | 10.4.x |
| 📦 XAMPP | 8.x |
| 🚪 Port | 3307 |
| 🌐 Framework | Laravel |
| 📁 Projet | `C:\Dev\dpef-platform` |
| 💾 Base de données | `dpef_db_v20_07_2026` |
| 📂 Sauvegarde | `C:\mysql-backup-2026-08-04` |

---

# 📋 Prérequis

Avant de commencer :

- ✅ Fermer phpMyAdmin
- ✅ Fermer Laravel
- ✅ Arrêter MySQL
- ✅ Sauvegarder le dossier `mysql\data`

---

# 🔧 Étapes de réparation

## ① Vérifier si MariaDB fonctionne

> [!TIP]
>
> Si `mysqld.exe` n'apparaît pas, MariaDB est arrêté.

### PowerShell

```powershell
Get-Process mysqld -ErrorAction SilentlyContinue
```

### CMD

```cmd
tasklist /FI "IMAGENAME eq mysqld.exe"
```

---

## ② Vérifier le port

### PowerShell

```powershell
netstat -ano | Select-String ":3307"
```

### CMD

```cmd
netstat -ano | findstr ":3307"
```

---

## ③ Lire les erreurs

### PowerShell

```powershell
Get-Content "C:\xampp\mysql\data\mysql_error.log" -Tail 100
```

### CMD

```cmd
findstr /I /C:"[ERROR]" "C:\xampp\mysql\data\mysql_error.log"
```

---

## ④ Arrêter MariaDB

⚠️ Utiliser d'abord le bouton **Stop** de XAMPP.

Puis :

```powershell
mysqladmin shutdown
```

---

## ⑤ Sauvegarder les données

> [!WARNING]
>
> Ne jamais supprimer le dossier `data` avant une sauvegarde complète.

```powershell
Copy-Item ...
```

---

## ⑥ Réparer les journaux Aria

```powershell
Move-Item ...
```

---

## ⑦ Restaurer global_priv

```powershell
Copy-Item ...
```

---

## ⑧ Réparer les tables

```powershell
aria_chk --recover
```

---

## ⑨ Démarrer MariaDB

```powershell
mysql_start.bat
```

---

## ⑩ Vérifier Laravel

```bash
php artisan config:clear
```

```bash
php artisan migrate:status
```

---

# ✅ Vérifications finales

Toutes les vérifications suivantes doivent être validées :

| Vérification | Résultat |
|--------------|----------|
| MariaDB démarre | ✅ |
| Port 3307 | ✅ |
| Laravel se connecte | ✅ |
| phpMyAdmin fonctionne | ✅ |
| Base présente | ✅ |
| Migrations OK | ✅ |

---

# 🚨 Erreurs courantes

## Aria engine: log initialization failed

➡️ Régénérer les fichiers `aria_log`.

---

## Incorrect file format 'global_priv'

➡️ Restaurer les fichiers depuis :

```
C:\xampp\mysql\backup\mysql
```

---

## Could not open mysql.plugin

➡️ Réparer les tables système.

---

# 💡 Conseils

> [!TIP]
>
> Toujours créer une sauvegarde complète avant toute réparation.

> [!WARNING]
>
> Ne jamais lancer `aria_chk` lorsque MariaDB est démarré.

> [!NOTE]
>
> Les fichiers `ibdata1`, `ib_logfile0` et `ib_logfile1` ne doivent jamais être supprimés.

---

# 📁 Structure des dossiers

```text
C:\
│
├── Dev
│   └── dpef-platform
│
├── xampp
│   └── mysql
│       ├── backup
│       ├── bin
│       └── data
│
└── mysql-backup-2026-08-04
```

---

# 📖 Références

- MariaDB
- XAMPP
- Laravel

---

# 📜 Licence

MIT License

---

<p align="center">

**⭐ Si ce guide vous a aidé, pensez à laisser une étoile au dépôt GitHub ⭐**

</p>
