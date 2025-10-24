# Guide : Configurer un serveur pour CobbleNuutt Launcher

## 📋 Table des matières
1. [Vue d'ensemble](#vue-densemble)
2. [Méthode 1 : Utiliser Nebula (Recommandé)](#méthode-1--utiliser-nebula-recommandé)
3. [Méthode 2 : Configuration manuelle](#méthode-2--configuration-manuelle)
4. [Héberger vos fichiers](#héberger-vos-fichiers)
5. [Calculer les hash MD5](#calculer-les-hash-md5)
6. [Tester votre distribution](#tester-votre-distribution)

---

## Vue d'ensemble

Le launcher a besoin d'un fichier `distribution.json` qui contient :
- Les informations du serveur (nom, adresse, version Minecraft)
- La liste de tous les mods requis
- Les fichiers de configuration
- Les resource packs et shaders
- Les URLs pour télécharger tous ces fichiers

**Emplacement actuel de la distribution :**
```javascript
// Dans app/assets/js/distromanager.js
exports.REMOTE_DISTRO_URL = 'https://helios-files.geekcorner.eu.org/distribution.json'
```

Vous devez modifier cette URL pour pointer vers votre propre fichier `distribution.json` hébergé.

---

## Méthode 1 : Utiliser Nebula (Recommandé)

### Avantages
✅ Calcule automatiquement les MD5
✅ Génère automatiquement la structure JSON
✅ Scan tous vos fichiers
✅ Plus rapide et moins d'erreurs

### Étapes

1. **Télécharger Nebula**
   - Aller sur : https://github.com/dscalzi/Nebula/releases
   - Télécharger la version pour votre OS

2. **Préparer vos fichiers de serveur**
   ```
   MonServeur/
   ├── mods/
   │   ├── mod1.jar
   │   ├── mod2.jar
   │   └── ...
   ├── config/
   │   ├── config1.cfg
   │   └── ...
   ├── resourcepacks/
   │   └── pack.zip
   └── ...
   ```

3. **Lancer Nebula**
   - Ouvrir Nebula
   - Sélectionner votre dossier de serveur
   - Configurer les informations (nom, version MC, etc.)
   - Générer le `distribution.json`

4. **Héberger les fichiers** (voir section ci-dessous)

---

## Méthode 2 : Configuration manuelle

### 1. Utiliser le template fourni

Un fichier `distribution_template.json` a été créé dans ce dossier. Copiez-le et modifiez :

```json
{
    "version": "1.0.0",
    "servers": [
        {
            "id": "CobbleNuutt-1.20.1",
            "name": "CobbleNuutt Server",
            "address": "VOTRE_IP:25565",
            "minecraftVersion": "1.20.1",
            ...
        }
    ]
}
```

### 2. Ajouter vos mods

Pour chaque mod, ajoutez une entrée dans `modules` :

```json
{
    "id": "com.example:monmod:1.0.0",
    "name": "Mon Super Mod",
    "type": "ForgeMod",
    "required": {
        "value": true
    },
    "artifact": {
        "size": 1234567,
        "MD5": "abc123...",
        "url": "https://votre-site.com/mods/monmod-1.0.0.jar"
    }
}
```

### 3. Types de modules disponibles

| Type | Usage | Exemple |
|------|-------|---------|
| `ForgeHosted` | Forge lui-même | forge-1.20.1.jar |
| `ForgeMod` | Mods Forge | JEI, Optifine, etc. |
| `Library` | Bibliothèques Java | Dépendances |
| `File` | Fichiers configs, resource packs | configs, textures |
| `VersionManifest` | Manifest de version Forge | version.json |

---

## Héberger vos fichiers

Vous avez plusieurs options :

### Option 1 : GitHub Pages (Gratuit)
```bash
# 1. Créer un repo GitHub
# 2. Activer GitHub Pages dans Settings
# 3. Upload vos fichiers dans le repo
# 4. URL sera : https://votre-username.github.io/votre-repo/
```

### Option 2 : Serveur Web personnel
- Uploadez via FTP/SFTP
- Assurez-vous que les fichiers sont accessibles en HTTPS

### Option 3 : Services cloud
- Google Drive (avec lien direct)
- Dropbox (avec lien direct)
- OneDrive
- **Note :** Certains services peuvent avoir des limitations

### ⚠️ Important
- Tous les fichiers doivent être accessibles en **HTTPS**
- Les URLs doivent être **directes** (pas de redirections)
- Testez chaque URL dans un navigateur avant

---

## Calculer les hash MD5

### Sur Windows (PowerShell)
```powershell
Get-FileHash -Algorithm MD5 "chemin\vers\fichier.jar" | Select-Object Hash
```

### Sur Windows (Script Python)
Créez `calculate_md5.py` :
```python
import hashlib
import sys

def calculate_md5(filepath):
    md5 = hashlib.md5()
    with open(filepath, 'rb') as f:
        for chunk in iter(lambda: f.read(4096), b''):
            md5.update(chunk)
    return md5.hexdigest()

if __name__ == '__main__':
    print(calculate_md5(sys.argv[1]))
```

Utilisation :
```bash
python calculate_md5.py monmod.jar
```

### Sur Linux/Mac
```bash
md5sum fichier.jar
```

---

## Configurer l'URL de distribution

1. **Modifier le fichier `distromanager.js`**

```javascript
// Avant
exports.REMOTE_DISTRO_URL = 'https://helios-files.geekcorner.eu.org/distribution.json'

// Après
exports.REMOTE_DISTRO_URL = 'https://votre-site.com/distribution.json'
```

2. **Pour tester en local d'abord**

Vous pouvez aussi tester avec un serveur local :
```bash
# Dans le dossier contenant distribution.json
python -m http.server 8000
```

Puis dans `distromanager.js` :
```javascript
exports.REMOTE_DISTRO_URL = 'http://localhost:8000/distribution.json'
```

---

## Tester votre distribution

### 1. Vérifier le JSON
- Utilisez https://jsonlint.com/ pour valider la syntaxe
- Vérifiez que toutes les URLs sont correctes

### 2. Tester les téléchargements
- Ouvrez chaque URL dans un navigateur
- Vérifiez que le fichier se télécharge

### 3. Vérifier les MD5
```bash
# Télécharger le fichier
# Calculer le MD5
# Comparer avec celui dans distribution.json
```

### 4. Lancer le launcher
```bash
npm start
```

Regardez la console pour les erreurs :
- `CTRL + Shift + I` pour ouvrir la console DevTools
- Vérifiez les messages d'erreur de téléchargement

---

## Structure d'exemple complète

```
VotreServeur/
├── distribution.json          # Fichier principal
├── files/
│   ├── forge/
│   │   ├── forge-universal.jar
│   │   └── version.json
│   ├── mods/
│   │   ├── jei.jar
│   │   ├── optifine.jar
│   │   └── ...
│   ├── config/
│   │   └── monmod.cfg
│   └── resourcepacks/
│       └── pack.zip
└── server-icon.png
```

---

## Discord Rich Presence (Optionnel)

Pour activer Discord Rich Presence :

1. Créer une application Discord : https://discord.com/developers/applications
2. Copier le Client ID
3. Uploader vos images dans "Rich Presence" → "Art Assets"
4. Mettre à jour dans `distribution.json` :

```json
"discord": {
    "clientId": "VOTRE_CLIENT_ID",
    "smallImageText": "CobbleNuutt",
    "smallImageKey": "nom-de-votre-image"
}
```

---

## Checklist finale

Avant de distribuer votre launcher :

- [ ] Tous les fichiers sont uploadés et accessibles
- [ ] Tous les MD5 sont calculés et corrects
- [ ] L'URL de distribution est configurée
- [ ] Le fichier JSON est valide (jsonlint.com)
- [ ] Toutes les URLs fonctionnent en HTTPS
- [ ] Le launcher se lance sans erreur
- [ ] Les fichiers se téléchargent correctement
- [ ] Le jeu se lance avec tous les mods

---

## Ressources utiles

- **Documentation Helios Distribution** : https://github.com/dscalzi/helios-distribution-types
- **Nebula (générateur)** : https://github.com/dscalzi/Nebula
- **Exemple de distribution** : `docs/sample_distribution.json`
- **Documentation complète** : `docs/distro.md`

---

## Support

Pour toute question :
- Discord CobbleNuutt : https://discord.gg/npfsQfpYp8
- GitHub Issues : https://github.com/Deltaartsstudio/cobblenuuttLauncher/issues
