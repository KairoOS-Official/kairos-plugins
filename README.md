# 🔌 KaïroOS Plugins Store

Dépôt officiel des plugins pour **KaïroOS** — le frontend d'arcade et de salon open source.

---

## 📦 Système de Plugins

### Vue d'ensemble

| Catégorie | Badge | Source | Installation | Validation |
|-----------|-------|--------|--------------|------------|
| **Builtin** | 🟣 SYSTÈME | Intégré à l'app | Préinstallé | Équipe KaïroOS |
| **Official** | 🟢 OFFICIEL | `official/` | 1 clic | Équipe KaïroOS |
| **Community** | 🟡 COMMUNAUTÉ | `community/` | 1 clic + sandbox | PR validée |
| **Unverified** | 🔴 NON VÉRIFIÉ | URL GitHub | Manuel + sandbox | Aucune |

### Plugins Builtin (Système)

Les plugins système sont intégrés à KaïroOS et ne peuvent pas être supprimés. Ils sont désactivables.

| Plugin | Description |
|--------|-------------|
| **kairo-remote** | Contrôle à distance & PWA mobile |

### Plugins Officiels

Les plugins officiels sont développés et maintenus par l'équipe KaïroOS.

| Plugin | Description |
|--------|-------------|
| **kairo-scraper** | Scraping automatique des jaquettes |
| **kairo-spotify-screensaver** | Spotify Connect + paroles karaoké |

### Plugins Communauté Vérifiés

Les plugins communautaires ont été validés par l'équipe via une Pull Request.

→ Consultez le dossier [`community/`](community/)

### Plugins Non Vérifiés

Les plugins non vérifiés sont des créations communautaires non validées. Ils sont installables via une URL GitHub.

→ Consultez le dossier [`unverified/`](unverified/)

---

## 🚀 Installer un Plugin

### Depuis l'application (recommandé)

1. Ouvrez **Paramètres** → **Plugins & Extensions**
2. Choisissez l'onglet correspondant :
   - **Installés** : plugins déjà installés
   - **Officiels** : plugins certifiés KaïroOS
   - **Communauté** : plugins vérifiés
   - **Non Vérifiés** : plugins par URL GitHub
3. Cliquez sur **Installer**

### Installer un plugin non vérifié

1. Ouvrez l'onglet **Non Vérifiés**
2. Collez l'URL du dépôt GitHub
3. Vérifiez les permissions affichées
4. Cliquez sur **Installer**

---

## 🔧 Créer un Plugin

### Structure

```
mon-plugin/
├── plugin.json      ← obligatoire
├── index.js         ← point d'entrée
├── preview.png      ← recommandé
└── README.md        ← recommandé
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
  "permissions": ["network", "read_games"],
  "entry": "index.js",
  "commands": ["start", "stop"],
  "settings_schema": {},
  "sandbox": true
}
```

> 📖 Consultez le guide complet dans [`PLUGIN_GUIDE.md`](PLUGIN_GUIDE.md)

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

### Pour les plugins vérifiés (recommandé)

1. **Forkez** ce dépôt
2. Ajoutez votre plugin dans `community/{nom-plugin}/`
3. Ouvrez une **Pull Request**
4. L'équipe valide et merge

### Pour les plugins non vérifiés (rapide)

1. Créez un dépôt GitHub avec `plugin.json` + code
2. Ajoutez le topic `kairoos-plugin` dans les settings
3. Partagez l'URL sur Discord ou GitHub Discussions
4. Les utilisateurs installent depuis l'app via l'URL

---

## ✅ Critères de Validation

Votre plugin sera accepté si :

- [ ] `plugin.json` est un JSON valide
- [ ] L'`id` est unique
- [ ] Les permissions sont justifiées
- [ ] Le plugin fonctionne en sandbox
- [ ] Une documentation est fournie

---

## 🛡️ Sécurité

Tous les plugins non-builtin s'exécutent en **sandbox** :
- Accès réseau déclaré uniquement
- Pas d'accès filesystem hors du dossier plugin
- Modification de config avec permission
- Surveillance temps réel

---

## 📁 Structure du Dépôt

```
kairos-plugins/
├── README.md              ← Ce fichier
├── PLUGIN_GUIDE.md        ← Guide complet
├── official/              ← Plugins officiels KaïroOS
├── community/             ← Plugins vérifiés (PR)
└── unverified/            ← Guide plugins non vérifiés
```

---

## 🔗 Liens

- [Dépôt principal](https://github.com/KairoOS-Official/KairoOS)
- [Discord](https://discord.gg/kairo-os)
- [Guide des plugins](PLUGIN_GUIDE.md)
- [Plugins non vérifiés](unverified/)

---

Fait avec ❤️ par la communauté KaïroOS.
