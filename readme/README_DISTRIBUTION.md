# 📦 Guide de Distribution CobbleNuutt Launcher

Ce guide explique comment générer et déployer la distribution pour le CobbleNuutt Launcher.

---

## 📋 Table des matières

1. [Prérequis](#prérequis)
2. [Configuration de Nebula](#configuration-de-nebula)
3. [Génération de la distribution](#génération-de-la-distribution)
4. [Déploiement sur le serveur](#déploiement-sur-le-serveur)
5. [Test du launcher](#test-du-launcher)
6. [Mise à jour de la distribution](#mise-à-jour-de-la-distribution)
7. [Dépannage](#dépannage)

---

## 🔧 Prérequis

- **Node.js 20+** : https://nodejs.org/
- **Java 21+** : https://adoptium.net/
- **Git** : https://git-scm.com/
- **Accès FTP** au serveur web (nuutt-cobblemon.com)
- **Nebula** : https://github.com/dscalzi/Nebula

---

## ⚙️ Configuration de Nebula

### 1. Cloner Nebula

```bash
git clone https://github.com/dscalzi/Nebula.git
cd Nebula
npm install
```

### 2. Créer le fichier `.env`

Créez un fichier `.env` à la racine de Nebula :

```properties
# Root directory où seront générés les fichiers de distribution
ROOT=D:\dev fivem\CobbleNuutt-Distribution

# Dossier de données du launcher (pour test local)
HELIOS_DATA_FOLDER=C:\Users\VOTRE_NOM\AppData\Roaming\CobbleNuutt Launcher

# URL publique où les fichiers seront hébergés
BASE_URL=https://nuutt-cobblemon.com/launcher/

# Java 21 requis pour Minecraft 1.21.1
JAVA_EXECUTABLE=java
```

**⚠️ Remplacez `VOTRE_NOM` par votre nom d'utilisateur Windows**

---

## 🏗️ Génération de la distribution

### Étape 1 : Initialiser la structure

```bash
cd Nebula
npm run start -- init root
```

Cela crée la structure de base dans `D:\dev fivem\CobbleNuutt-Distribution\`

### Étape 2 : Générer le serveur Fabric

```bash
npm run start -- generate server CobbleNuutt 1.21.1 --fabric 0.17.2
```

**Paramètres :**
- `CobbleNuutt` : ID du serveur
- `1.21.1` : Version de Minecraft
- `--fabric 0.17.2` : Version de Fabric Loader

### Étape 3 : Copier les fichiers du serveur

#### A. Créer les dossiers fabricmods

```powershell
mkdir "D:\dev fivem\CobbleNuutt-Distribution\servers\CobbleNuutt-1.21.1\fabricmods\required"
mkdir "D:\dev fivem\CobbleNuutt-Distribution\servers\CobbleNuutt-1.21.1\fabricmods\optionalon"
mkdir "D:\dev fivem\CobbleNuutt-Distribution\servers\CobbleNuutt-1.21.1\fabricmods\optionaloff"
```

#### B. Copier les mods (REQUIS)

```powershell
Copy-Item "D:\dev fivem\CobbleNuutt-Server\mods\*" -Destination "D:\dev fivem\CobbleNuutt-Distribution\servers\CobbleNuutt-1.21.1\fabricmods\required\" -Recurse
```

#### C. Copier les configs

```powershell
Copy-Item "D:\dev fivem\CobbleNuutt-Server\config" -Destination "D:\dev fivem\CobbleNuutt-Distribution\servers\CobbleNuutt-1.21.1\files\" -Recurse
```

#### D. Copier les resourcepacks (optionnel)

```powershell
Copy-Item "D:\dev fivem\CobbleNuutt-Server\resourcepacks" -Destination "D:\dev fivem\CobbleNuutt-Distribution\servers\CobbleNuutt-1.21.1\files\" -Recurse
```

#### E. Copier les shaderpacks (optionnel)

```powershell
Copy-Item "D:\dev fivem\CobbleNuutt-Server\shaderpacks" -Destination "D:\dev fivem\CobbleNuutt-Distribution\servers\CobbleNuutt-1.21.1\files\" -Recurse
```

### Étape 4 : Configurer les métadonnées

#### A. Éditer `servermeta.json`

Fichier : `D:\dev fivem\CobbleNuutt-Distribution\servers\CobbleNuutt-1.21.1\servermeta.json`

```json
{
  "meta": {
    "version": "1.0.0",
    "name": "CobbleNuutt Server",
    "description": "Serveur Cobblemon CobbleNuutt - Attrapez-les tous !",
    "icon": "",
    "address": "play.nuutt-cobblemon.com:25565",
    "discord": {
      "shortId": "CobbleNuutt",
      "largeImageText": "CobbleNuutt Server",
      "largeImageKey": "server-cobbenuutt"
    },
    "mainServer": true,
    "autoconnect": true,
    "javaOptions": {
      "supported": ">=21",
      "suggestedMajor": 21,
      "ram": {
        "recommended": 6144,
        "minimum": 4096
      }
    }
  },
  "fabric": {
    "version": "0.17.2"
  }
}
```

#### B. Créer `distrometa.json`

Fichier : `D:\dev fivem\CobbleNuutt-Distribution\meta\distrometa.json`

```json
{
  "meta": {
    "rss": "https://nuutt-cobblemon.com/api/rss",
    "discord": {
      "clientId": "VOTRE_DISCORD_CLIENT_ID",
      "smallImageText": "CobbleNuutt",
      "smallImageKey": "cobbenuutt-logo"
    }
  }
}
```

#### C. Ajouter l'icône du serveur (optionnel)

Copiez une image PNG ou JPG :
```
D:\dev fivem\CobbleNuutt-Distribution\servers\CobbleNuutt-1.21.1\server-icon.png
```

### Étape 5 : Générer le distribution.json

```bash
cd Nebula
npm run start -- generate distro
```

**Cela va :**
- ✅ Scanner tous les mods (138+)
- ✅ Calculer automatiquement les MD5
- ✅ Générer le fichier `distribution.json`
- ✅ Sauvegarder dans `D:\dev fivem\CobbleNuutt-Distribution\distribution.json`

**Durée estimée :** 2-5 minutes

---

## 🌐 Déploiement sur le serveur

### Structure à uploader

Uploadez **TOUT le contenu** de `D:\dev fivem\CobbleNuutt-Distribution\` sur votre serveur web.

**Via FTP :**

```
Source (local) :
D:\dev fivem\CobbleNuutt-Distribution\
├── distribution.json
└── servers\
    └── CobbleNuutt-1.21.1\
        ├── files\
        ├── libraries\
        └── fabricmods\

Destination (serveur) :
nuutt-cobblemon.com/public_html/launcher/
├── distribution.json
└── servers\
    └── CobbleNuutt-1.21.1\
        ├── files\
        ├── libraries\
        └── fabricmods\
```

### Vérification du déploiement

Une fois uploadé, testez dans votre navigateur :

```
https://nuutt-cobblemon.com/launcher/distribution.json
```

Vous devriez voir le contenu JSON.

---

## 🧪 Test du launcher

### Test en local (sans upload)

Pour tester avant d'uploader :

```bash
cd Nebula
npm run start -- generate distro distribution_dev --installLocal
```

Cela installe la distribution directement dans le launcher pour test.

### Test avec le launcher

```powershell
cd D:\dev fivem\cobblenuuttLauncher
npm start
```

**Le launcher devrait :**
1. ✅ Télécharger `distribution.json`
2. ✅ Installer Fabric Loader 0.17.2
3. ✅ Télécharger tous les mods (138+)
4. ✅ Télécharger les configs, resourcepacks, shaderpacks
5. ✅ Lancer Minecraft 1.21.1 avec Cobblemon

### Vérification dans DevTools

Appuyez sur `Ctrl + Shift + I` pour ouvrir la console et vérifiez :
- Pas d'erreurs de téléchargement
- Tous les MD5 correspondent
- Les modules se chargent correctement

---

## 🔄 Mise à jour de la distribution

### Ajouter/Modifier un mod

1. **Ajoutez/Modifiez** le mod dans :
   ```
   D:\dev fivem\CobbleNuutt-Distribution\servers\CobbleNuutt-1.21.1\fabricmods\required\
   ```

2. **Régénérez** la distribution :
   ```bash
   cd Nebula
   npm run start -- generate distro
   ```

3. **Uploadez** le nouveau `distribution.json` ET le nouveau mod sur le serveur

### Modifier la configuration du serveur

1. **Éditez** `servermeta.json`
2. **Régénérez** la distribution
3. **Uploadez** le nouveau `distribution.json`

### Changer l'adresse du serveur

Éditez `servermeta.json` :
```json
"address": "nouvelle-adresse.com:25565"
```

Puis régénérez et uploadez.

---

## 🛠️ Dépannage

### Erreur : "Please provide a URL protocol"

**Problème :** Le fichier `.env` n'a pas `https://` dans `BASE_URL`

**Solution :** Vérifiez que `BASE_URL=https://nuutt-cobblemon.com/launcher/`

---

### Erreur : "Server already exists"

**Problème :** Le serveur a déjà été généré

**Solution :** Supprimez le dossier existant :
```powershell
Remove-Item -Recurse -Force "D:\dev fivem\CobbleNuutt-Distribution\servers\CobbleNuutt-1.21.1"
```

Puis régénérez.

---

### Les mods ne se téléchargent pas

**Problème :** Les fichiers ne sont pas uploadés ou les URLs sont incorrectes

**Solutions :**
1. Vérifiez que les fichiers sont bien sur le serveur
2. Testez les URLs dans un navigateur
3. Vérifiez que `BASE_URL` dans `.env` est correct
4. Vérifiez les permissions des fichiers sur le serveur

---

### Erreur : "Invalid fabric.mod.json"

**Problème :** Certains mods ont des métadonnées invalides

**Solution :** Ce n'est généralement pas bloquant. Nebula ignore ces erreurs et continue. Si le mod est critique, contactez le développeur du mod.

---

### Le launcher ne trouve pas Java 21

**Problème :** Java 21 n'est pas installé ou pas dans le PATH

**Solutions :**
1. Installez Java 21 : https://adoptium.net/
2. Ajoutez Java au PATH système
3. Ou spécifiez le chemin dans `servermeta.json` :
   ```json
   "javaOptions": {
     "executable": "C:\\Program Files\\Java\\jdk-21\\bin\\java.exe"
   }
   ```

---

## 📚 Ressources utiles

- **Documentation Nebula** : https://github.com/dscalzi/Nebula
- **Documentation Helios Launcher** : https://github.com/dscalzi/HeliosLauncher
- **Format distribution.json** : https://github.com/dscalzi/HeliosLauncher/blob/master/docs/distro.md
- **Discord CobbleNuutt** : https://discord.gg/npfsQfpYp8
- **Site web** : https://nuutt-cobblemon.com

---

## 📝 Notes importantes

- **Fabric Loader** : Version 0.17.2 pour Minecraft 1.21.1
- **Java requis** : Java 21 minimum
- **RAM recommandée** : 6GB (4GB minimum)
- **Nombre de mods** : 138+ mods Fabric
- **Taille totale** : ~500MB à télécharger pour le client

---

## ✅ Checklist de déploiement

Avant de distribuer le launcher :

- [ ] Tous les mods sont dans `fabricmods/required/`
- [ ] Les configs sont copiées
- [ ] `servermeta.json` est configuré (adresse serveur, RAM, etc.)
- [ ] `distrometa.json` est configuré (RSS, Discord)
- [ ] Icône serveur ajoutée (optionnel)
- [ ] `distribution.json` généré sans erreur
- [ ] Tous les fichiers uploadés sur le serveur
- [ ] URL `https://nuutt-cobblemon.com/launcher/distribution.json` accessible
- [ ] Test du launcher réussi
- [ ] Minecraft se lance avec tous les mods
- [ ] Connexion au serveur fonctionne

---

**Dernière mise à jour :** 25 octobre 2025

**Auteur :** CobbleNuutt Team
