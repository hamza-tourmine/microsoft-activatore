<div align="center">

# 🌐 Guide de dépannage Internet sous Windows

### Connexion réseau active, mais accès Web impossible

[![README](https://img.shields.io/badge/🏠_README-Accueil-2563EB?style=for-the-badge)](README.md)
[![Internet](https://img.shields.io/badge/🌐_Internet-Dépannage-16A34A?style=for-the-badge)](internet.md)

</div>

---

## 📋 Description du problème

L'ordinateur est **connecté au réseau Internet**, mais les sites Web ne s'ouvrent pas correctement dans le navigateur.

### Symptômes possibles

- ✅ Wi-Fi ou Ethernet connecté
- ✅ Icône Internet active
- ❌ Chrome n'accède pas aux sites Web
- ❌ Certaines pages restent en chargement
- ❌ Erreurs réseau ou DNS

---

## 🌐 Étape 1 — Désactiver le protocole QUIC dans Chrome

Ouvrez **Google Chrome**.

Dans la barre d'adresse, saisissez :

```text
chrome://flags/


<div align="center">

# 🌐 Dépannage de connexion Internet sous Windows

### Connexion Internet active, mais accès Web impossible

[![README](https://img.shields.io/badge/🏠_README-Accueil-2563EB?style=for-the-badge)](README.md)
[![Internet](https://img.shields.io/badge/🌐_Internet-Dépannage-16A34A?style=for-the-badge)](internet.md)

---

**Diagnostic • DNS • TCP/IP • Winsock • Proxy • Chrome**

</div>

---

## 📋 Description du problème

Ce guide concerne principalement le cas suivant :

> L'ordinateur indique qu'il est connecté à Internet, mais les sites Web ne s'ouvrent pas dans le navigateur.

### Symptômes possibles

- ✅ Wi-Fi ou Ethernet connecté
- ✅ Icône réseau active
- ❌ Google Chrome n'ouvre pas les sites
- ❌ Les pages restent en chargement
- ❌ Erreur DNS
- ❌ Erreur de connexion
- ❌ Internet fonctionne sur d'autres appareils uniquement

---

# 🔍 Étape 1 — Vérifier la configuration réseau

Ouvrez :

```text
Invite de commandes
```

ou :

```text
PowerShell
```

Puis exécutez :

```cmd
ipconfig /all
```

Cette commande affiche la configuration réseau complète de l'ordinateur.

Vérifiez notamment :

```text
IPv4 Address
Default Gateway
DHCP
DNS Servers
```

---

# 🌍 Étape 2 — Tester la connexion Internet

Exécutez :

```cmd
ping 8.8.8.8
```

### Exemple de résultat fonctionnel

```text
Reply from 8.8.8.8
```

Si vous recevez des réponses, la connexion réseau vers Internet fonctionne probablement.

Si vous obtenez :

```text
Request timed out
```

ou :

```text
Destination host unreachable
```

le problème peut être lié à :

- la carte réseau
- le routeur
- la passerelle
- le VPN
- le pare-feu
- la configuration IP

---

# 🔎 Étape 3 — Tester le DNS

Exécutez :

```cmd
nslookup google.com
```

Le DNS permet de convertir un nom de domaine comme :

```text
google.com
```

en adresse IP.

Si `ping 8.8.8.8` fonctionne mais que `nslookup google.com` échoue, le problème est probablement lié au DNS.

---

# 🧹 Étape 4 — Vider le cache DNS

Exécutez :

```cmd
ipconfig /flushdns
```

Cette commande vide le cache DNS local de Windows.

### Résultat attendu

```text
Successfully flushed the DNS Resolver Cache.
```

---

# 🔄 Étape 5 — Renouveler l'adresse IP

## Libérer l'adresse IP actuelle

```cmd
ipconfig /release
```

Puis demander une nouvelle adresse IP :

```cmd
ipconfig /renew
```

> ⚠️ Ces commandes sont principalement utiles lorsque l'adresse IP est fournie automatiquement par DHCP.

---

# 🔧 Étape 6 — Réinitialiser Winsock

Ouvrez le terminal en tant qu'administrateur.

Exécutez :

```cmd
netsh winsock reset
```

Cette commande réinitialise le catalogue Winsock de Windows.

Winsock intervient dans les communications réseau utilisées par les applications.

Après l'exécution, redémarrez l'ordinateur.

---

# 🛠️ Étape 7 — Réinitialiser TCP/IP

Exécutez en tant qu'administrateur :

```cmd
netsh int ip reset
```

Cette commande réinitialise certains paramètres TCP/IP de Windows.

> ⚠️ Vérifiez la configuration réseau avant cette opération si l'ordinateur utilise une adresse IP statique.

---

# 🌐 Étape 8 — Vérifier le proxy Windows

Affichez la configuration du proxy WinHTTP :

```cmd
netsh winhttp show proxy
```

### Configuration sans proxy

Vous pouvez obtenir un résultat similaire à :

```text
Direct access (no proxy server)
```

Si un proxy incorrect est configuré, cela peut bloquer l'accès Web.

---

# ♻️ Réinitialiser le proxy WinHTTP

Exécutez :

```cmd
netsh winhttp reset proxy
```

Cette commande remet la configuration WinHTTP en accès direct.

> ⚠️ Ne pas exécuter cette commande sur un réseau professionnel utilisant volontairement un proxy.

---

# 🗺️ Étape 9 — Vérifier le chemin réseau

Exécutez :

```cmd
tracert google.com
```

Cette commande affiche le chemin réseau entre votre ordinateur et le serveur distant.

Elle peut aider à identifier l'étape où la communication est interrompue.

---

# 🌐 Étape 10 — Vérifier Google Chrome

Ouvrez Google Chrome.

Dans la barre d'adresse, saisissez :

```text
chrome://flags/
```

Recherchez :

```text
QUIC
```

Si une option liée à QUIC est disponible, vous pouvez temporairement la définir sur :

```text
Disabled
```

Puis relancez Chrome.

> ⚠️ Les options de `chrome://flags` sont expérimentales et peuvent changer ou disparaître selon la version de Chrome.

---

# ⚡ Réparation réseau rapide

Ouvrez **CMD ou PowerShell en tant qu'administrateur**.

Exécutez les commandes suivantes dans l'ordre :

```cmd
ipconfig /flushdns
ipconfig /release
ipconfig /renew
netsh winsock reset
netsh int ip reset
```

Puis :

```text
Redémarrez l'ordinateur.
```

---

# 🧪 Diagnostic rapide

Utilisez cet ordre :

```text
Internet ne fonctionne pas
        │
        ▼
ping 8.8.8.8
        │
        ├── Échec
        │     │
        │     ▼
        │  Vérifier IP, routeur,
        │  passerelle, VPN et réseau
        │
        └── Fonctionne
              │
              ▼
       nslookup google.com
              │
              ├── Échec
              │     │
              │     ▼
              │  Problème DNS probable
              │
              └── Fonctionne
                    │
                    ▼
             Vérifier Chrome,
             proxy et applications
```

---

# 📌 Tableau des commandes

| Commande | Rôle |
|---|---|
| `ipconfig /all` | Afficher la configuration réseau |
| `ping 8.8.8.8` | Tester l'accès réseau vers Internet |
| `nslookup google.com` | Tester la résolution DNS |
| `ipconfig /flushdns` | Vider le cache DNS |
| `ipconfig /release` | Libérer l'adresse IP DHCP |
| `ipconfig /renew` | Demander une nouvelle adresse IP |
| `netsh winsock reset` | Réinitialiser Winsock |
| `netsh int ip reset` | Réinitialiser TCP/IP |
| `netsh winhttp show proxy` | Afficher le proxy WinHTTP |
| `netsh winhttp reset proxy` | Réinitialiser le proxy WinHTTP |
| `tracert google.com` | Afficher le chemin réseau |

---

# ⚠️ Précautions

Avant de réinitialiser la configuration réseau, vérifiez si l'ordinateur utilise :

- une adresse IP statique
- un DNS personnalisé
- un VPN
- un proxy d'entreprise
- une configuration réseau professionnelle
- un logiciel de sécurité réseau

Certaines commandes peuvent modifier la configuration réseau locale.

---

# ✅ Ordre recommandé

Pour un ordinateur connecté au réseau mais sans accès Web :

```cmd
ping 8.8.8.8
nslookup google.com
ipconfig /flushdns
netsh winhttp show proxy
ipconfig /release
ipconfig /renew
netsh winsock reset
netsh int ip reset
```

Ensuite :

```text
1. Redémarrer Windows
2. Ouvrir Chrome
3. Tester google.com
4. Tester un autre navigateur
```

---

<div align="center">

## 🛡️ Windows Network Troubleshooting

**Diagnostiquer avant de réinitialiser**

[![Retour au README](https://img.shields.io/badge/⬅️_Retour-README-2563EB?style=for-the-badge)](README.md)

</div>
