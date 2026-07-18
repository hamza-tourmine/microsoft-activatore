# 🖨️ Partager une imprimante avec des PC autorisés sous Windows

![Windows](https://img.shields.io/badge/Windows-10%20%7C%2011-0078D4?logo=windows&logoColor=white)
![Réseau](https://img.shields.io/badge/Réseau-Local-22C55E)
![Sécurité](https://img.shields.io/badge/Accès-Protégé-orange)
![Niveau](https://img.shields.io/badge/Niveau-Débutant-blue)

> Guide complet pour partager une imprimante connectée à un ordinateur principal, tout en limitant son utilisation aux comptes autorisés.

---

## 📑 Table des matières

- [Objectif](#-objectif)
- [Prérequis](#-prérequis)
- [Architecture du partage](#-architecture-du-partage)
- [Partie 1 — Configuration du PC principal](#️-partie-1--configuration-du-pc-principal)
  - [1. Configurer le réseau comme privé](#1️⃣-configurer-le-réseau-comme-privé)
  - [2. Créer les comptes autorisés](#2️⃣-créer-les-comptes-autorisés)
  - [3. Activer le partage réseau](#3️⃣-activer-le-partage-réseau)
  - [4. Vérifier l'imprimante](#4️⃣-vérifier-le-fonctionnement-de-limprimante)
  - [5. Partager l'imprimante](#5️⃣-partager-limprimante)
  - [6. Configurer les autorisations](#6️⃣-configurer-les-autorisations-dimpression)
  - [7. Récupérer le nom et l'adresse IP](#7️⃣-récupérer-le-nom-et-ladresse-ip-du-pc-principal)
- [Partie 2 — Connexion du PC distant](#️-partie-2--connexion-depuis-un-pc-autorisé)
- [Tests de validation](#-tests-de-validation)
- [Dépannage](#-dépannage)
- [Bonnes pratiques](#-bonnes-pratiques-de-sécurité)
- [Checklist finale](#-checklist-finale)

---

## 🎯 Objectif

L'imprimante est installée sur un **PC principal**, généralement par câble USB. Les autres ordinateurs du réseau local pourront l'utiliser uniquement après avoir fourni un **nom d'utilisateur** et un **mot de passe autorisés**.

> [!IMPORTANT]
> Le PC principal doit rester **allumé**, **connecté au réseau** et accessible pour que les PC distants puissent imprimer.

---

## ✅ Prérequis

Avant de commencer, vérifiez les points suivants :

- Windows 10 ou Windows 11 est installé sur les ordinateurs ;
- l'imprimante fonctionne correctement sur le PC principal ;
- tous les PC sont connectés au même réseau local ;
- vous disposez d'un compte administrateur sur le PC principal ;
- chaque compte autorisé possède un mot de passe ;
- le pilote de l'imprimante est installé et à jour.

---

## 🌐 Architecture du partage

```text
                  Réseau local / Routeur
                           │
              ┌────────────┴────────────┐
              │                         │
      PC principal                PC distant autorisé
      192.168.1.25                Compte : PC_Bureau
              │                         │
              └──── Imprimante partagée┘
                    \\PC-PRINCIPAL\HP_Bureau
```

| Élément | Exemple | Rôle |
|---|---|---|
| Nom du PC principal | `PC-PRINCIPAL` | Héberge l'imprimante |
| Adresse IPv4 | `192.168.1.25` | Permet de localiser le PC sur le réseau |
| Nom du partage | `HP_Bureau` | Identifie l'imprimante partagée |
| Compte autorisé | `PC_Bureau` | Autorise l'accès à l'impression |
| Chemin réseau | `\\PC-PRINCIPAL\HP_Bureau` | Permet d'installer l'imprimante distante |

---

# 🖥️ Partie 1 — Configuration du PC principal

## 1️⃣ Configurer le réseau comme privé

Le profil **Privé** autorise la découverte des appareils sur un réseau de confiance.

1. Ouvrez **Paramètres**.
2. Allez dans **Réseau et Internet**.
3. Sélectionnez votre connexion **Wi-Fi** ou **Ethernet**.
4. Ouvrez les propriétés de la connexion.
5. Sélectionnez **Réseau privé**.

> [!CAUTION]
> N'activez pas le profil privé lorsque vous êtes connecté à un réseau public, comme celui d'un café, d'un hôtel ou d'un aéroport.

---

## 2️⃣ Créer les comptes autorisés

Windows protège l'accès à l'imprimante en utilisant les comptes présents sur le PC principal.

Il est recommandé de créer un compte différent pour chaque ordinateur ou utilisateur :

| Ordinateur autorisé | Compte proposé |
|---|---|
| Bureau | `PC_Bureau` |
| Comptabilité | `PC_Comptabilite` |
| Direction | `PC_Direction` |

### Procédure

1. Ouvrez **Paramètres**.
2. Allez dans **Comptes**.
3. Ouvrez **Autres utilisateurs** ou **Famille et autres utilisateurs**.
4. Cliquez sur **Ajouter un compte**.
5. Sélectionnez **Je ne dispose pas des informations de connexion de cette personne**.
6. Cliquez sur **Ajouter un utilisateur sans compte Microsoft**.
7. Saisissez un nom, par exemple :

   ```text
   PC_Bureau
   ```

8. Définissez un mot de passe sécurisé.
9. Complétez les questions de sécurité.
10. Cliquez sur **Suivant**.

> [!NOTE]
> Le compte doit rester de type **Utilisateur standard**. Il n'a pas besoin d'être administrateur.

> [!WARNING]
> Ne créez pas un compte sans mot de passe. Windows bloque généralement l'authentification réseau des comptes locaux sans mot de passe.

Répétez cette opération pour chaque PC autorisé.

---

## 3️⃣ Activer le partage réseau

### Windows 11

1. Ouvrez **Paramètres**.
2. Accédez à **Réseau et Internet**.
3. Cliquez sur **Paramètres réseau avancés**.
4. Ouvrez **Paramètres de partage avancés**.
5. Dans **Réseaux privés**, activez :

   - **Découverte du réseau** ;
   - **Partage de fichiers et d'imprimantes**.

6. Dans **Tous les réseaux**, activez :

   - **Partage protégé par mot de passe**.

### Windows 10

1. Ouvrez le **Panneau de configuration**.
2. Accédez à **Réseau et Internet**.
3. Ouvrez **Centre Réseau et partage**.
4. Cliquez sur **Modifier les paramètres de partage avancés**.
5. Dans **Privé**, activez :

   - **Activer la découverte de réseau** ;
   - **Activer le partage de fichiers et d'imprimantes**.

6. Dans **Tous les réseaux**, activez :

   - **Activer le partage protégé par mot de passe**.

7. Cliquez sur **Enregistrer les modifications**.

📚 Référence : [Partager une imprimante réseau — Microsoft Support](https://support.microsoft.com/en-us/windows/hardware/printer/share-a-printer-as-a-network-printer)

---

## 4️⃣ Vérifier le fonctionnement de l'imprimante

Avant d'activer le partage :

1. Ouvrez **Paramètres**.
2. Accédez à **Bluetooth et appareils > Imprimantes et scanners**.
3. Sélectionnez l'imprimante.
4. Ouvrez **Propriétés de l'imprimante**.
5. Cliquez sur **Imprimer une page de test**.

> [!TIP]
> Ne continuez que si la page de test est correctement imprimée depuis le PC principal.

---

## 5️⃣ Partager l'imprimante

1. Appuyez sur <kbd>Windows</kbd> + <kbd>R</kbd>.
2. Saisissez la commande suivante :

   ```text
   control printers
   ```

3. Appuyez sur <kbd>Entrée</kbd>.
4. Faites un clic droit sur l'imprimante.
5. Sélectionnez **Propriétés de l'imprimante**.
6. Ouvrez l'onglet **Partage**.
7. Cochez **Partager cette imprimante**.
8. Saisissez un nom simple, par exemple :

   ```text
   HP_Bureau
   ```

9. Vous pouvez cocher **Rendu des travaux d'impression sur les ordinateurs clients**.
10. Cliquez sur **Appliquer**.

> [!TIP]
> Utilisez un nom court, sans accent, espace ou caractère spécial.

---

## 6️⃣ Configurer les autorisations d'impression

Toujours dans **Propriétés de l'imprimante** :

1. Ouvrez l'onglet **Sécurité**.
2. Sélectionnez le groupe **Tout le monde** ou **Everyone**.
3. Cliquez sur **Supprimer**.

Si sa suppression n'est pas possible, retirez au minimum son autorisation **Imprimer**.

> [!CAUTION]
> Ne supprimez pas les entrées `SYSTEM`, `Administrateurs` ou `CREATOR OWNER`. Elles sont nécessaires au fonctionnement et à l'administration de l'imprimante.

### Ajouter un utilisateur autorisé

1. Cliquez sur **Ajouter**.
2. Entrez le compte créé précédemment :

   ```text
   PC_Bureau
   ```

3. Cliquez sur **Vérifier les noms**.
4. Le compte devrait être reconnu sous une forme similaire à :

   ```text
   PC-PRINCIPAL\PC_Bureau
   ```

5. Cliquez sur **OK**.
6. Sélectionnez le nouvel utilisateur.
7. Cochez uniquement **Autoriser > Imprimer**.
8. Cliquez sur **Appliquer**, puis sur **OK**.

| Permission | Recommandation | Description |
|---|---:|---|
| Imprimer | ✅ Autoriser | Imprimer et gérer ses propres documents |
| Gérer les documents | ❌ Ne pas autoriser | Gérer les travaux des autres utilisateurs |
| Gérer cette imprimante | ❌ Ne pas autoriser | Modifier la configuration de l'imprimante |

Répétez cette procédure pour chaque compte autorisé.

---

## 7️⃣ Récupérer le nom et l'adresse IP du PC principal

### Trouver le nom du PC

1. Appuyez sur <kbd>Windows</kbd> + <kbd>R</kbd>.
2. Saisissez `cmd`.
3. Exécutez :

   ```cmd
   hostname
   ```

Exemple :

```text
PC-PRINCIPAL
```

### Trouver l'adresse IPv4

Dans la même fenêtre, exécutez :

```cmd
ipconfig
```

Repérez la ligne **Adresse IPv4** de la connexion utilisée.

Exemple :

```text
Adresse IPv4 : 192.168.1.25
```

### Chemins de connexion

Vous pouvez utiliser le nom du PC :

```text
\\PC-PRINCIPAL\HP_Bureau
```

Ou son adresse IP :

```text
\\192.168.1.25\HP_Bureau
```

> [!TIP]
> Configurez une **réservation DHCP** dans votre routeur afin que le PC principal conserve toujours la même adresse IP.

---

# 💻 Partie 2 — Connexion depuis un PC autorisé

## 1️⃣ Vérifier la connexion réseau

Le PC distant doit être connecté au même réseau local que le PC principal.

Vous pouvez tester la communication avec :

```cmd
ping 192.168.1.25
```

Remplacez `192.168.1.25` par l'adresse réelle du PC principal.

> [!NOTE]
> L'absence de réponse au `ping` ne prouve pas toujours que le PC est inaccessible, car le pare-feu peut bloquer cette commande.

---

## 2️⃣ Ouvrir le PC principal

1. Appuyez sur <kbd>Windows</kbd> + <kbd>R</kbd>.
2. Saisissez l'une des adresses suivantes :

   ```text
   \\PC-PRINCIPAL
   ```

   ou :

   ```text
   \\192.168.1.25
   ```

3. Appuyez sur <kbd>Entrée</kbd>.

---

## 3️⃣ Saisir les identifiants autorisés

Lorsque Windows demande les informations de connexion :

1. Cliquez si nécessaire sur **Plus de choix**.
2. Sélectionnez **Utiliser un autre compte**.
3. Saisissez le nom d'utilisateur sous cette forme :

   ```text
   PC-PRINCIPAL\PC_Bureau
   ```

4. Entrez le mot de passe défini sur le PC principal.
5. Cochez **Mémoriser mes informations d'identification**.
6. Validez.

> [!IMPORTANT]
> Le nom placé avant `\` est celui du **PC principal**, et non celui du PC distant.

---

## 4️⃣ Installer l'imprimante

Dans la fenêtre réseau :

1. Repérez l'imprimante `HP_Bureau`.
2. Faites un clic droit dessus.
3. Cliquez sur **Connexion**.
4. Attendez l'installation du pilote.

Windows peut demander l'autorisation d'un administrateur lors de la première installation.

### Méthode alternative

1. Ouvrez **Paramètres**.
2. Accédez à **Bluetooth et appareils > Imprimantes et scanners**.
3. Cliquez sur **Ajouter un appareil**.
4. Cliquez sur **Ajouter manuellement** si l'imprimante n'apparaît pas.
5. Sélectionnez **Sélectionner une imprimante partagée par nom**.
6. Saisissez :

   ```text
   \\PC-PRINCIPAL\HP_Bureau
   ```

7. Cliquez sur **Suivant**.

---

## 5️⃣ Imprimer une page de test

1. Ouvrez **Paramètres > Imprimantes et scanners**.
2. Sélectionnez l'imprimante partagée.
3. Ouvrez **Propriétés de l'imprimante**.
4. Cliquez sur **Imprimer une page de test**.

---

## 🧪 Tests de validation

Après la configuration, effectuez les tests suivants :

| Test | Résultat attendu |
|---|---|
| Impression locale depuis le PC principal | ✅ Réussite |
| Connexion avec `PC_Bureau` | ✅ Autorisée |
| Impression depuis le PC autorisé | ✅ Réussite |
| Connexion sans identifiant | ❌ Refusée |
| Connexion avec un mauvais mot de passe | ❌ Refusée |
| Accès avec un compte non autorisé | ❌ Refusé |

> [!WARNING]
> Cette configuration protège l'imprimante avec des **identifiants**, mais elle ne reconnaît pas physiquement l'ordinateur. Une personne connaissant les identifiants pourrait les utiliser depuis un autre PC du même réseau.

Pour un contrôle strict par ordinateur, utilisez également des adresses IP réservées et des règles de pare-feu adaptées, ou un véritable serveur d'impression.

---

## 🛠️ Dépannage

### L'imprimante n'apparaît pas

Vérifiez que :

- le PC principal est allumé ;
- l'imprimante fonctionne localement ;
- les deux PC sont sur le même réseau ;
- le profil réseau est défini sur **Privé** ;
- la découverte du réseau est activée ;
- le partage de fichiers et d'imprimantes est activé ;
- le pare-feu ou l'antivirus ne bloque pas le partage.

📚 Référence : [Résoudre les problèmes de connexion aux imprimantes partagées — Microsoft Support](https://support.microsoft.com/en-us/windows/hardware/printer/fix-shared-printer-connection-problems-in-windows)

### Erreur « Accès refusé »

Vérifiez :

- que le compte possède l'autorisation **Imprimer** ;
- que le compte n'est pas désactivé ;
- que le mot de passe est correct ;
- que le nom est saisi sous la forme `PC-PRINCIPAL\PC_Bureau` ;
- que le partage protégé par mot de passe est activé.

### Windows réutilise un ancien mot de passe

1. Ouvrez le **Panneau de configuration**.
2. Accédez à **Comptes d'utilisateurs**.
3. Ouvrez le **Gestionnaire d'informations d'identification**.
4. Sélectionnez **Informations d'identification Windows**.
5. Supprimez l'entrée correspondant au PC principal.
6. Reconnectez-vous avec les bons identifiants.

### Le nom du PC ne fonctionne pas

Essayez directement l'adresse IP :

```text
\\192.168.1.25\HP_Bureau
```

### Le pilote ne s'installe pas

- exécutez l'installation depuis un compte administrateur sur le PC distant ;
- installez le pilote officiel du fabricant ;
- vérifiez que les deux PC utilisent une architecture compatible ;
- redémarrez le service **Spouleur d'impression** si nécessaire.

> [!CAUTION]
> N'appliquez pas de modification du Registre trouvée sur Internet pour désactiver la sécurité RPC. Cela peut exposer le PC principal à des risques de sécurité.

---

## 🔐 Bonnes pratiques de sécurité

- Utilisez un compte différent pour chaque PC ou utilisateur.
- Choisissez des mots de passe uniques et difficiles à deviner.
- Ne communiquez pas les identifiants à des personnes non autorisées.
- Supprimez immédiatement les comptes qui ne sont plus utilisés.
- Accordez uniquement l'autorisation **Imprimer**.
- Maintenez Windows et les pilotes d'impression à jour.
- Ne partagez jamais l'imprimante sur un réseau public.
- Conservez les comptes `SYSTEM` et `Administrateurs` dans les permissions.
- Réservez l'adresse IP du PC principal dans le routeur.

---

## ☑️ Checklist finale

### PC principal

- [ ] L'imprimante fonctionne localement.
- [ ] Le réseau est configuré comme privé.
- [ ] La découverte du réseau est activée.
- [ ] Le partage de fichiers et d'imprimantes est activé.
- [ ] Le partage protégé par mot de passe est activé.
- [ ] Un compte avec mot de passe existe pour chaque PC autorisé.
- [ ] L'imprimante possède un nom de partage simple.
- [ ] Le groupe `Tout le monde` ne possède pas l'autorisation d'imprimer.
- [ ] Les comptes autorisés possèdent uniquement l'autorisation **Imprimer**.
- [ ] Le nom et l'adresse IP du PC principal ont été notés.

### PC distant

- [ ] Le PC est connecté au même réseau local.
- [ ] Le chemin réseau du PC principal est accessible.
- [ ] Les bons identifiants ont été enregistrés.
- [ ] L'imprimante partagée est installée.
- [ ] La page de test s'imprime correctement.

---

## 📚 Ressources officielles

- [Partager une imprimante en tant qu'imprimante réseau](https://support.microsoft.com/en-us/windows/hardware/printer/share-a-printer-as-a-network-printer)
- [Résoudre les problèmes de connexion aux imprimantes partagées](https://support.microsoft.com/en-us/windows/hardware/printer/fix-shared-printer-connection-problems-in-windows)
- [Autorisations d'impression Windows](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj190062%28v%3Dws.11%29)

---

## 📄 Licence

Ce guide peut être utilisé et adapté pour la documentation interne de votre organisation.

---

<div align="center">

**Guide de configuration — Partage sécurisé d'une imprimante sous Windows 10/11**

</div>
