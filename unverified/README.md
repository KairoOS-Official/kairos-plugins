# 🔴 Plugins Non Vérifiés (Unverified)

> ⚠️ **Attention** : Ces plugins n'ont PAS été validés par l'équipe KaïroOS. Installez-les à vos propres risques.

---

## 📦 Comment ça marche ?

Les plugins non vérifiés sont des créations communautaires qui n'ont pas passé le processus de validation officiel. Ils sont accessibles directement depuis KaïroOS via **l'installation par URL GitHub**.

### Sécurité

Tous les plugins non vérifiés s'exécutent en **sandbox** :
- Permissions vérifiées avant installation
- Processus isolé
- Pas d'accès au filesystem hors du dossier plugin
- Surveillance en temps réel

---

## 🔍 Trouver des Plugins Non Vérifiés

### Sources recommandées

1. **GitHub** : Cherchez des repos avec le topic [`kairo-plugins`](https://github.com/topics/kairo-plugins)
2. **Discord** : Canal #community-plugins
3. **GitHub Discussions** : [kairos-community](https://github.com/KairoOS-Official/kairos-community/discussions)

### Vérifier un plugin avant installation

1. Consultez le `plugin.json` : vérifiez les permissions demandées
2. Lisez le code source : `index.js` ou autre point d'entrée
3. Vérifiez la réputation de l'auteur

---

## 🚀 Installer un Plugin Non Vérifié

### Depuis KaïroOS

1. Ouvrez **Paramètres** → **Plugins & Extensions**
2. Cliquez sur l'onglet **Non Vérifiés**
3. Collez l'URL du dépôt GitHub : `https://github.com/utilisateur/nom-plugin`
4. Cliquez sur **Analyser**
5. Vérifiez les permissions affichées
6. Cliquez sur **Installer**

### Structure du dépôt requis

```
mon-plugin/
├── plugin.json      ← obligatoire
├── index.js         ← point d'entrée
├── preview.png      ← recommandé
└── README.md        ← recommandé
```

### Exemple de `plugin.json`

```json
{
  "id": "mon-plugin",
  "name": "Mon Plugin",
  "version": "1.0.0",
  "author": "MonPseudo",
  "type": "unverified",
  "description": "Description claire",
  "permissions": ["network", "notifications"],
  "entry": "index.js"
}
```

---

## 📤 Publier votre Plugin

### Prérequis

1. Un dépôt GitHub public
2. Un fichier `plugin.json` valide
3. Un point d'entrée (`index.js` ou autre)

### Étapes

1. Créez votre dépôt GitHub
2. Ajoutez le topic `kairo-plugins` dans les settings du repo
3. Partagez l'URL :
   - **Discord** : canal #community-plugins
   - **GitHub** : Discussion dans [kairos-community](https://github.com/KairoOS-Official/kairos-community/discussions)

### Faire valider votre plugin

Si vous souhaitez que votre plugin soit promu en **Community Vérifié** :

1. Fork `kairos-plugins`
2. Ajoutez votre plugin dans `community/{nom-plugin}/`
3. Ouvrez une Pull Request
4. L'équipe valide et merge → votre plugin devient officiellement communautaire

---

## ⚠️ Avertissements

- Les plugins non vérifiés ne sont **pas supportés** par l'équipe KaïroOS
- En cas de problème, contactez directement l'auteur
- Vérifiez toujours les permissions avant installation
- Utilisez les plugins non vérifiés uniquement à des fins d'évaluation

---

## 🔗 Liens

- [Guide officiel des plugins](../PLUGIN_GUIDE.md)
- [Plugins officiels](../official/)
- [Plugins communautaires vérifiés](../community/)
- [Discord](https://discord.gg/kairo-os)
