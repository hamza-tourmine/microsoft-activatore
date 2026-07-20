Guide complet de résolution d'un problème « Boot Device Not Found (3F0) »
Étape 1 : Vérifier le disque dans le BIOS
Appuyer sur F10 au démarrage.
Vérifier que le disque apparaît dans Storage > Device Configuration.

Si le disque n'apparaît pas :

vérifier les câbles SATA et l'alimentation ;
ou tester le disque sur un autre PC.
Étape 2 : Tester le disque

Au démarrage :

F2 → System Diagnostics → Hard Disk Test

Si le test échoue, le disque est probablement défectueux.

Étape 3 : Ouvrir l'environnement de récupération
Démarrer sur une clé USB Windows ou accéder à Réparation.
Choisir Dépannage → Options avancées → Invite de commandes.
Étape 4 : Vérifier les partitions
diskpart
list disk
select disk 0
list partition

Vérifier la présence de :

partition EFI (FAT32, environ 100 à 500 Mo) ;
partition Windows principale.
Étape 5 : Vérifier Windows
dir C:\Windows

Si le dossier existe, Windows est toujours installé.

Étape 6 : Monter la partition EFI
diskpart
select disk 0
select partition 1
assign letter=Z
exit
Étape 7 : Vérifier la partition EFI
dir Z:\EFI\Microsoft\Boot

Vérifier la présence de bootmgfw.efi et du dossier EFI.

Étape 8 : Recréer les fichiers de démarrage
bcdboot C:\Windows /s Z: /f UEFI

Le message attendu est :

Les fichiers de démarrage ont bien été créés.
Étape 9 : Vérifier l'ordre de démarrage

Au démarrage :

Appuyer sur F9.
Choisir le disque en UEFI.
Étape 10 : Redémarrer

Si tout est correct, Windows doit démarrer normalement.

Si cela ne fonctionne toujours pas

Les étapes suivantes consistent à :

réparer le magasin BCD (bootrec, bcdedit) ;
vérifier le système de fichiers avec chkdsk ;
réparer Windows avec sfc et DISM hors ligne ;
en dernier recours, effectuer une réparation de Windows sans supprimer les fichiers.
