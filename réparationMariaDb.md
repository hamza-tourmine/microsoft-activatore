Guide complet de réparation MariaDB/XAMPP
Ce guide présente chaque commande dans les deux environnements :
PowerShell
Invite de commandes Windows — CMD
Votre configuration :
Projet Laravel : C:\Dev\dpef-platform
XAMPP           : C:\xampp
Port MariaDB    : 3307
Base            : dpef_db_v20_07_2026
Sauvegarde      : C:\mysql-backup-2026-08-04
Votre problème est déjà corrigé. Ne répétez pas les opérations de réparation sur le serveur fonctionnel. Conservez ce guide pour une future récupération.

1. Vérifier si MySQL fonctionne
PowerShell
Get-Process mysqld -ErrorAction SilentlyContinue
CMD
tasklist /FI "IMAGENAME eq mysqld.exe"
Si aucun processus mysqld.exe n’apparaît, MySQL est arrêté.
2. Vérifier le port 3307
PowerShell
netstat -ano | Select-String ":3307"
CMD
netstat -ano | findstr ":3307"
Lorsque MySQL fonctionne, vous devez voir :
0.0.0.0:3307    LISTENING
3. Consulter les erreurs MariaDB
PowerShell — dernières lignes
Get-Content "C:\xampp\mysql\data\mysql_error.log" -Tail 100
CMD — rechercher les erreurs
findstr /I /C:"[ERROR]" "C:\xampp\mysql\data\mysql_error.log"
Pour afficher le fichier complet dans CMD :
type "C:\xampp\mysql\data\mysql_error.log"
Les erreurs rencontrées étaient :
Aria engine: log initialization failed
Plugin 'Aria' registration as a STORAGE ENGINE failed
Could not open mysql.plugin table
Failed to initialize plugins
Incorrect file format 'global_priv'
4. Arrêter MySQL
Utilisez d’abord le bouton Stop devant MySQL dans XAMPP.
Si MariaDB répond encore, vous pouvez demander un arrêt propre.
PowerShell
& "C:\xampp\mysql\bin\mysqladmin.exe" `
    --host=127.0.0.1 `
    --port=3307 `
    --user=root `
    shutdown
CMD
C:\xampp\mysql\bin\mysqladmin.exe --host=127.0.0.1 --port=3307 --user=root shutdown
Vérifiez ensuite son arrêt.
PowerShell
Get-Process mysqld -ErrorAction SilentlyContinue
CMD
tasklist /FI "IMAGENAME eq mysqld.exe"
Ne lancez jamais aria_chk pendant que MySQL fonctionne.
5. Créer une sauvegarde complète
PowerShell
New-Item `
    -ItemType Directory `
    -Path "C:\mysql-backup-2026-08-04" `
    -Force
Copy-Item `
    -Path "C:\xampp\mysql\data" `
    -Destination "C:\mysql-backup-2026-08-04\data" `
    -Recurse
CMD
mkdir "C:\mysql-backup-2026-08-04"
xcopy "C:\xampp\mysql\data" "C:\mysql-backup-2026-08-04\data\" /E /I /H /K /Y
Vérifiez la sauvegarde.
PowerShell
Get-ChildItem "C:\mysql-backup-2026-08-04\data"
CMD
dir "C:\mysql-backup-2026-08-04\data"
Ne supprimez jamais ces fichiers :
ibdata1
ib_logfile0
ib_logfile1
6. Sauvegarder et régénérer les journaux Aria
Cette étape doit uniquement être utilisée si le journal contient :
Aria engine: log initialization failed
Créez un dossier pour conserver les journaux.
PowerShell
New-Item `
    -ItemType Directory `
    -Path "C:\mysql-backup-2026-08-04\aria-reset" `
    -Force
CMD
mkdir "C:\mysql-backup-2026-08-04\aria-reset"
Déplacez ensemble les fichiers aria_log.* et aria_log_control.
PowerShell
Get-ChildItem "C:\xampp\mysql\data" -Filter "aria_log*" |
    Move-Item -Destination "C:\mysql-backup-2026-08-04\aria-reset"
CMD
move /Y "C:\xampp\mysql\data\aria_log*" "C:\mysql-backup-2026-08-04\aria-reset\"
MariaDB les recréera au prochain démarrage.
7. Sauvegarder la table global_priv corrompue
Cette étape est nécessaire uniquement si le journal contient :
Incorrect file format 'global_priv'
Créez un dossier de sauvegarde.
PowerShell
New-Item `
    -ItemType Directory `
    -Path "C:\mysql-backup-2026-08-04\global-priv-corrupt" `
    -Force
CMD
mkdir "C:\mysql-backup-2026-08-04\global-priv-corrupt"
Déplacez les trois fichiers corrompus.
PowerShell
Move-Item `
    "C:\xampp\mysql\data\mysql\global_priv.frm" `
    "C:\mysql-backup-2026-08-04\global-priv-corrupt"

Move-Item `
    "C:\xampp\mysql\data\mysql\global_priv.MAD" `
    "C:\mysql-backup-2026-08-04\global-priv-corrupt"

Move-Item `
    "C:\xampp\mysql\data\mysql\global_priv.MAI" `
    "C:\mysql-backup-2026-08-04\global-priv-corrupt"
CMD
move /Y "C:\xampp\mysql\data\mysql\global_priv.frm" "C:\mysql-backup-2026-08-04\global-priv-corrupt\"
move /Y "C:\xampp\mysql\data\mysql\global_priv.MAD" "C:\mysql-backup-2026-08-04\global-priv-corrupt\"
move /Y "C:\xampp\mysql\data\mysql\global_priv.MAI" "C:\mysql-backup-2026-08-04\global-priv-corrupt\"
8. Restaurer la copie saine de global_priv
XAMPP conserve une copie originale dans :
C:\xampp\mysql\backup\mysql
PowerShell
Copy-Item `
    "C:\xampp\mysql\backup\mysql\global_priv.frm" `
    "C:\xampp\mysql\data\mysql\global_priv.frm"

Copy-Item `
    "C:\xampp\mysql\backup\mysql\global_priv.MAD" `
    "C:\xampp\mysql\data\mysql\global_priv.MAD"

Copy-Item `
    "C:\xampp\mysql\backup\mysql\global_priv.MAI" `
    "C:\xampp\mysql\data\mysql\global_priv.MAI"
CMD
copy /Y "C:\xampp\mysql\backup\mysql\global_priv.frm" "C:\xampp\mysql\data\mysql\global_priv.frm"
copy /Y "C:\xampp\mysql\backup\mysql\global_priv.MAD" "C:\xampp\mysql\data\mysql\global_priv.MAD"
copy /Y "C:\xampp\mysql\backup\mysql\global_priv.MAI" "C:\xampp\mysql\data\mysql\global_priv.MAI"
Cette restauration réinitialise les comptes MySQL aux valeurs XAMPP par défaut. Elle ne modifie pas les utilisateurs de l’application Laravel.
9. Réparer toutes les tables système Aria
MySQL doit être complètement arrêté.
PowerShell
$ariaChk = "C:\xampp\mysql\bin\aria_chk.exe"
$dataRoot = "C:\xampp\mysql\data"

Get-ChildItem "C:\xampp\mysql\data\mysql" -Filter "*.MAI" |
    ForEach-Object {
        & $ariaChk `
            --recover `
            --backup `
            --zerofill `
            --datadir=$dataRoot `
            $_.FullName
    }
CMD
La commande suivante doit être saisie directement dans CMD :
for %F in ("C:\xampp\mysql\data\mysql\*.MAI") do "C:\xampp\mysql\bin\aria_chk.exe" --recover --backup --zerofill --datadir="C:\xampp\mysql\data" "%~fF"
Dans un fichier .bat, utilisez %%F au lieu de %F :
for %%F in ("C:\xampp\mysql\data\mysql\*.MAI") do "C:\xampp\mysql\bin\aria_chk.exe" --recover --backup --zerofill --datadir="C:\xampp\mysql\data" "%%~fF"
10. Réparer columns_priv avec la méthode sûre
Cette commande est nécessaire si la réparation normale affiche :
aria_sort_buffer_size is too small
Create index by sort failed
PowerShell
& "C:\xampp\mysql\bin\aria_chk.exe" `
    --safe-recover `
    --force `
    --backup `
    --zerofill `
    --datadir="C:\xampp\mysql\data" `
    "C:\xampp\mysql\data\mysql\columns_priv.MAI"
CMD
C:\xampp\mysql\bin\aria_chk.exe --safe-recover --force --backup --zerofill --datadir="C:\xampp\mysql\data" "C:\xampp\mysql\data\mysql\columns_priv.MAI"
11. Démarrer MySQL
PowerShell
& "C:\xampp\mysql_start.bat"
Pour le lancer dans une fenêtre cachée :
Start-Process `
    -FilePath "C:\xampp\mysql\bin\mysqld.exe" `
    -ArgumentList "--defaults-file=mysql\bin\my.ini","--standalone" `
    -WorkingDirectory "C:\xampp" `
    -WindowStyle Hidden
CMD
C:\xampp\mysql_start.bat
Vous pouvez aussi utiliser le bouton Start dans XAMPP.
12. Vérifier que MySQL répond
PowerShell
& "C:\xampp\mysql\bin\mysql.exe" `
    --protocol=TCP `
    --host=127.0.0.1 `
    --port=3307 `
    --user=root `
    --connect-timeout=5 `
    --execute="SELECT VERSION(); SHOW DATABASES LIKE 'dpef_db_v20_07_2026';"
CMD
C:\xampp\mysql\bin\mysql.exe --protocol=TCP --host=127.0.0.1 --port=3307 --user=root --connect-timeout=5 --execute="SELECT VERSION(); SHOW DATABASES LIKE 'dpef_db_v20_07_2026';"
Le résultat doit afficher :
10.4.32-MariaDB
dpef_db_v20_07_2026
Si root possède un mot de passe, ajoutez -p.
13. Réparer les dernières tables système en ligne
Cette étape doit être exécutée uniquement après le redémarrage réussi de MariaDB.
PowerShell
$mysql = "C:\xampp\mysql\bin\mysql.exe"

$tables = @(
    "columns_priv",
    "db",
    "event",
    "func",
    "global_priv",
    "proc",
    "procs_priv",
    "proxies_priv",
    "roles_mapping",
    "servers",
    "tables_priv",
    "time_zone",
    "time_zone_leap_second",
    "time_zone_name",
    "time_zone_transition",
    "time_zone_transition_type"
)

foreach ($table in $tables) {
    & $mysql `
        --protocol=TCP `
        --host=127.0.0.1 `
        --port=3307 `
        --user=root `
        --execute="REPAIR NO_WRITE_TO_BINLOG TABLE mysql.$table; CHECK TABLE mysql.$table;"
}
CMD
À saisir directement dans CMD :
for %T in (columns_priv db event func global_priv proc procs_priv proxies_priv roles_mapping servers tables_priv time_zone time_zone_leap_second time_zone_name time_zone_transition time_zone_transition_type) do C:\xampp\mysql\bin\mysql.exe --protocol=TCP --host=127.0.0.1 --port=3307 --user=root --execute="REPAIR NO_WRITE_TO_BINLOG TABLE mysql.%T; CHECK TABLE mysql.%T;"
Dans un fichier .bat, remplacez %T par %%T.
Chaque contrôle doit se terminer par :
status OK
14. Réparer la table help_topic
PowerShell
& "C:\xampp\mysql\bin\mysql.exe" `
    --protocol=TCP `
    --host=127.0.0.1 `
    --port=3307 `
    --user=root `
    --execute="REPAIR NO_WRITE_TO_BINLOG TABLE mysql.help_topic; CHECK TABLE mysql.help_topic;"
CMD
C:\xampp\mysql\bin\mysql.exe --protocol=TCP --host=127.0.0.1 --port=3307 --user=root --execute="REPAIR NO_WRITE_TO_BINLOG TABLE mysql.help_topic; CHECK TABLE mysql.help_topic;"
15. Contrôler toutes les tables
PowerShell
& "C:\xampp\mysql\bin\mysqlcheck.exe" `
    --protocol=TCP `
    --host=127.0.0.1 `
    --port=3307 `
    --user=root `
    --databases mysql dpef_db_v20_07_2026 `
    --check
CMD
C:\xampp\mysql\bin\mysqlcheck.exe --protocol=TCP --host=127.0.0.1 --port=3307 --user=root --databases mysql dpef_db_v20_07_2026 --check
Les tables doivent afficher :
OK
16. Accéder au projet Laravel
PowerShell
Set-Location "C:\Dev\dpef-platform"
La forme courte fonctionne aussi :
cd "C:\Dev\dpef-platform"
CMD
cd /d "C:\Dev\dpef-platform"
L’option /d permet aussi de changer de disque.
17. Vérifier la configuration Laravel
Le fichier .env doit contenir :
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3307
DB_DATABASE=dpef_db_v20_07_2026
DB_USERNAME=root
PowerShell
Get-Content ".env" |
    Select-String "^(DB_CONNECTION|DB_HOST|DB_PORT|DB_DATABASE|DB_USERNAME)="
CMD
findstr /R /C:"^DB_CONNECTION=" /C:"^DB_HOST=" /C:"^DB_PORT=" /C:"^DB_DATABASE=" /C:"^DB_USERNAME=" ".env"
18. Vider le cache Laravel
Cette commande est identique dans les deux terminaux.
PowerShell
php artisan config:clear
CMD
php artisan config:clear
19. Vérifier les migrations
PowerShell
php artisan migrate:status --no-interaction
CMD
php artisan migrate:status --no-interaction
Si les migrations s’affichent avec le statut Ran, Laravel communique correctement avec MariaDB.
20. Test final rapide
PowerShell
& "C:\xampp\mysql\bin\mysql.exe" `
    --host=127.0.0.1 `
    --port=3307 `
    --user=root `
    --execute="SELECT 'MYSQL_OK';"
CMD
C:\xampp\mysql\bin\mysql.exe --host=127.0.0.1 --port=3307 --user=root --execute="SELECT 'MYSQL_OK';"
Le résultat attendu est :
MYSQL_OK
Votre installation est maintenant réparée et la sauvegarde complète reste disponible dans :
C:\mysql-backup-2026-08-04


10:04




















Accès complet







5.6 TerraTrès élevé5.6 TerraTrès élevé5.6 SolMoyen
