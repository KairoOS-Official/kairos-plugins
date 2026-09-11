# Plugin Guide — KairoOS

## Structure obligatoire

```
yourname-pluginname/
  plugin.json   ← obligatoire
  preview.png   ← obligatoire (min 800x450px)
  README.md     ← obligatoire
  index.js      ← ou tout autre point d'entrée
```

## plugin.json minimum

```json
{
  "id": "yourname-pluginname",
  "name": "My Plugin",
  "version": "1.0.0",
  "author": "yourname",
  "type": "community",
  "description": "What it does",
  "permissions": [],
  "entry": "index.js"
}
```

## Permissions disponibles

| Permission | Description |
|--|--|
| `read_games` | Lire la bibliothèque de jeux |
| `launch_games` | Lancer et arrêter des jeux |
| `read_settings` | Lire la configuration KairoOS |
| `write_settings` | Modifier la configuration |
| `network` | Ouvrir un port réseau |
| `filesystem` | Lire/écrire dans le dossier du plugin |
| `notifications` | Envoyer des notifications dans l'UI |

## Règles communautaires

- Préfixe obligatoire : `pseudo-nom` (ex: `flo-achievements`)
- Zéro fichier `.exe` ou script qui télécharge des binaires externes
- Les permissions déclarées doivent correspondre à l'usage réel
- Un plugin qui usurpe un nom officiel sera supprimé immédiatement

## Publier

Dépose ton dossier dans `community/` et push directement — pas de PR nécessaire.
La validation automatique vérifie le `plugin.json` et le `preview.png`.
Si la validation échoue, un commentaire automatique t'explique ce qui manque.

## Signaler un plugin

Ouvre une issue avec le template "Report a plugin".
