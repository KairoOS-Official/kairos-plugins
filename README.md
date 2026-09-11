# 🔌 KaïroOS Plugins Store

Dépôt officiel des plugins communautaires pour **KaïroOS** — le frontend d'arcade et de salon open source.

Ce dépôt alimente directement l'onglet **Plugins & Extensions** dans KaïroOS via l'API GitHub. Tout plugin soumis et mergé ici devient instantanément installable en un clic !

---

## 📦 Comment ça marche ?

### Types de Plugins

| Type | Description | Distribution |
|------|-------------|--------------|
| **Builtin** | Plugin système intégré à KaïroOS | Livré avec l'application |
| **Official** | Plugin validé par l'équipe KaïroOS | Store officiel |
| **Community** | Plugin développé par la communauté | Store communautaire |

### Plugins Officiels

Les plugins officiels sont dans le dépôt principal [`KairoOS-Official/KairoOS`](https://github.com/KairoOS-Official/KairoOS) dans le dossier `plugins/` :

| Plugin | Description | Type |
|--------|-------------|------|
| **kairo-remote** | Contrôle à distance & PWA mobile | Builtin |
| **kairo-scraper** | Scraping automatique des jaquettes | Official |
| **kairo-spotify-screensaver** | Spotify Connect + paroles karaoké | Official |

### Plugins Communautaires

Les plugins de la communauté sont hébergés ici et accessibles depuis l'onglet **Communauté** dans les paramètres.

---

## 🚀 Installer un Plugin

### Depuis l'application (recommandé)

1. Ouvrez **Paramètres** → **Plugins & Extensions**
2. Cliquez sur l'onglet **Communauté**
3. Parcourez les plugins disponibles et cliquez sur **Installer**
4. Activez le plugin !

### Manuellement

Copiez le dossier du plugin dans :
- **Mode portable** : `builds/portable/plugins/`
- **Mode installé** : `%APPDATA%\kairo-os\plugins/`

---

## 🔧 Créer un Plugin

### Structure

```
mon-plugin/
├── plugin.json      ← Contrat, métadonnées, permissions (obligatoire)
├── index.js         ← Point d'entrée (ou main.py, mon-plugin.exe)
├── preview.svg      ← Miniature du plugin (recommandé)
└── README.md        ← Documentation d'utilisation
```

### Spécification `plugin.json`

```json
{
  "id": "mon-plugin",
  "name": "Mon Super Plugin",
  "version": "1.0.0",
  "author": "VotrePseudo",
  "type": "community",
  "description": "Description claire du plugin.",
  "min_kairo_version": "0.1.0",
  "permissions": [
    "network",
    "read_games",
    "notifications"
  ],
  "entry": "index.js",
  "commands": ["start", "stop"],
  "settings_schema": {
    "api_key": {
      "type": "string",
      "secret": true,
      "label": "Clé API"
    }
  },
  "sandbox": true
}
```

> 📖 Consultez le guide complet dans [`PLUGIN_GUIDE.md`](PLUGIN_GUIDE.md) pour la spécification détaillée.

### Langages Supportés

- **JavaScript** (Node.js) — le plus simple pour commencer
- **Python** — pour le scripting rapide
- **Rust** — pour les performances maximales
- **Go, C++, etc.** — tout langage compilable en binaire

### Permissions

| Permission | Rôle |
|------------|------|
| `network` | Requêtes HTTP, serveur local |
| `read_games` | Lecture de la bibliothèque |
| `launch_games` | Lancement/arrêt de jeux |
| `read_settings` | Lecture de la config |
| `write_settings` | Modification de la config |
| `filesystem` | Accès au dossier du plugin |
| `notifications` | Affichage de toasts |

---

## 📤 Contribuer

### Via Pull Request (recommandé)

1. **Forkez** ce dépôt
2. Créez une branche : `git checkout -b plugin/mon-nouveau-plugin`
3. Ajoutez votre dossier dans `community/`
4. Testez localement dans KaïroOS
5. Ouvrez une **Pull Request**

### Soumettre sans GitHub

Envoyez votre plugin via :
- **Discord** : canal #plugin-submissions
- **Email** : plugins@kairo-os.com

L'équipe ajoutera votre plugin pour vous.

---

## ✅ Critères de Validation

Votre plugin sera accepté si :

- [ ] `plugin.json` est un JSON valide
- [ ] L'`id` est unique
- [ ] Les permissions demandées sont justifiées
- [ ] Le plugin fonctionne en mode sandbox
- [ ] Une documentation est fournie
- [ ] Pas de dépendances dangereuses (ex: accès total au filesystem)

---

## 🛡️ Sécurité

Tous les plugins communautaires s'exécutent en **sandbox** :
- Accès réseau autorisé uniquement si déclaré
- Aucun accès au filesystem hors du dossier plugin
- Aucune modification de la config sans permission
- Surveillance en temps réel par le PluginManager

En cas de problème, le plugin est automatiquement arrêté.

---

## 📁 Structure du Dépôt

```
kairos-plugins/
├── README.md              ← Ce fichier
├── PLUGIN_GUIDE.md        ← Guide complet de création
└── community/             ← Vos contributions ici
```

> **Note** : Les plugins officiels (`kairo-remote`, `kairo-scraper`, `kairo-spotify-screensaver`) sont dans le dépôt principal [`KairoOS-Official/KairoOS`](https://github.com/KairoOS-Official/KairoOS), pas ici.

---

## 🔗 Liens Utiles

- [Site officiel](https://kairo-os.com)
- [Dépôt principal](https://github.com/KairoOS-Official/KairoOS)
- [Discord](https://discord.gg/kairo-os)
- [Guide des plugins](PLUGIN_GUIDE.md)

---

Fait avec ❤️ par la communauté KaïroOS.
