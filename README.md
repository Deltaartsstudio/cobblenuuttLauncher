<p align="center"><img src="./assets/images/logo.png" width="150px" height="150px" alt="CobbleNuutt Launcher"></p>

<h1 align="center">CobbleNuutt Launcher</h1>

<em><h5 align="center">Basé sur HeliosLauncher de dscalzi — Modifié par Delta Arts</h5></em>

<p align="center">
<a href="https://github.com/Deltaartsstudio/cobblenuuttLauncher/actions">
<img src="https://img.shields.io/github/actions/workflow/status/Deltaartsstudio/cobblenuuttLauncher/build.yml?branch=main&style=for-the-badge" alt="build">
</a>
<a href="https://github.com/Deltaartsstudio/cobblenuuttLauncher/releases">
<img src="https://img.shields.io/github/downloads/Deltaartsstudio/cobblenuuttLauncher/total.svg?style=for-the-badge" alt="downloads">
</a>
<img src="https://forthebadge.com/images/badges/made-with-love.svg" height="28px" alt="made-with-love">
</p>

<p align="center">
Lanceur Minecraft moddé simple et automatisé — plus besoin d’installer Java, Forge ou les mods à la main.  
<b>CobbleNuutt Launcher</b> s’occupe de tout pour vous.
</p>

---

## ✨ Fonctionnalités principales

- 🔒 **Gestion complète des comptes**
  - Support Microsoft (OAuth 2.0) et Mojang.
  - Connexion rapide et sécurisée.
  - Gestion multi-comptes facile.

- 📂 **Gestion efficace des fichiers**
  - Téléchargement automatique des ressources et mises à jour.
  - Validation des fichiers avant lancement (réparation automatique si nécessaire).

- ☕ **Java automatique**
  - Installe la bonne version de Java si celle du système est incompatible.
  - Fonctionne sans Java préinstallé.

- 📰 **Actualités intégrées**
  - Affiche les dernières annonces du serveur ou de Delta Arts directement dans le launcher.

- ⚙️ **Paramètres intuitifs**
  - Contrôle du Java, de la mémoire, des thèmes, etc.

- 🧭 **Support multi-serveurs**
  - Passez d’une configuration serveur à une autre en un clic.
  - Visualisez le nombre de joueurs connectés.

- 🔁 **Mises à jour automatiques**
  - Le launcher se met à jour tout seul sans intervention.

- 🌐 **Statut des services Mojang**
  - Consultez rapidement si les services Minecraft sont disponibles.

---

## 📥 Téléchargement

Téléchargez la dernière version depuis les [releases GitHub](https://github.com/Deltaartsstudio/cobblenuuttLauncher/releases).

| Plateforme | Fichier |
| ----------- | -------- |
| Windows x64 | `CobbleNuutt-Launcher-setup-VERSION.exe` |
| macOS x64 | `CobbleNuutt-Launcher-setup-VERSION-x64.dmg` |
| macOS arm64 | `CobbleNuutt-Launcher-setup-VERSION-arm64.dmg` |
| Linux x64 | `CobbleNuutt-Launcher-setup-VERSION.AppImage` |

---

## 🧑‍💻 Développement

### Pré-requis

- [Node.js](https://nodejs.org/en/) **v20 ou supérieur**

### Installation du projet

```bash
git clone https://github.com/Deltaartsstudio/cobblenuuttLauncher.git
cd cobblenuuttLauncher
npm install
```

### Lancer le launcher en mode développement

```bash
npm start
```

### Créer les installateurs

Pour votre plateforme actuelle :
```bash
npm run dist
```

Ou pour une plateforme spécifique :

| Plateforme | Commande |
| ----------- | -------- |
| Windows x64 | `npm run dist:win` |
| macOS | `npm run dist:mac` |
| Linux x64 | `npm run dist:linux` |

---

## ⚙️ Console de débogage

Ouvrez la console avec :

```text
ctrl + shift + i
```

⚠️ **Attention :** ne collez aucune commande que vous ne comprenez pas — certaines peuvent exposer des informations sensibles.

Pour enregistrer la sortie de la console :
> Clic droit → **Save as...**

---

## 💡 Développement sous Visual Studio Code

Ajoutez ce fichier à `.vscode/launch.json` pour activer le débogage :

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug Main Process",
      "type": "node",
      "request": "launch",
      "cwd": "${workspaceFolder}",
      "program": "${workspaceFolder}/node_modules/electron/cli.js",
      "args": ["."],
      "outputCapture": "std"
    },
    {
      "name": "Debug Renderer Process",
      "type": "chrome",
      "request": "launch",
      "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/electron",
      "windows": {
        "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/electron.cmd"
      },
      "runtimeArgs": [
        "${workspaceFolder}/.",
        "--remote-debugging-port=9222"
      ],
      "webRoot": "${workspaceFolder}"
    }
  ]
}
```

---

## 🤝 Crédits & Remerciements

- 🎨 **Modifié par :** [Delta Arts](https://github.com/Deltaartsstudio)  
- 🧩 **Projet original :** [HeliosLauncher](https://github.com/dscalzi/HeliosLauncher) par **dscalzi**

Merci de conserver les crédits du projet d’origine ❤️  
Ce projet reste open-source — n’hésitez pas à contribuer !

---

## 📚 Ressources utiles

- [Wiki du projet original](https://github.com/dscalzi/HeliosLauncher/wiki)
- [Discord original](https://discord.gg/zNWUXdt)
- [Documentation Microsoft Auth](https://github.com/dscalzi/HeliosLauncher/blob/master/docs/MicrosoftAuth.md)

---

<p align="center"><b>Bon jeu et à bientôt sur CobbleNuutt !</b></p>
