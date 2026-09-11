# 🔌 Guide de Développement & Création de Plugins — KaïroOS

Bienvenue dans le guide officiel de création de plugins pour **KaïroOS**, le frontend d'arcade et de salon open source.

Ce document détaille l'architecture d'un plugin, le système de bac à sable (sandbox), le protocole de communication IPC, et explique comment concevoir, tester et publier vos extensions.

---

## 🏛️ 1. Architecture Générale

Un plugin KaïroOS est un dossier autonome vivant dans `plugins/<id>/` :
- Il s'exécute dans son **propre processus enfant supervisé** par le moteur Rust `PluginManager`.
- Il peut être écrit dans **n'importe quel langage** : JavaScript (Node.js), Python, Rust (binaire compilé `.exe`), Go, C++, etc.
- Il communique avec KaïroOS en temps réel via un **canal IPC standardisé sur stdin / stdout** en format JSON Lines.
- Il respecte un modèle de permissions strict : toute action non déclarée dans son contrat `plugin.json` est rejetée.

---

## 📁 2. Structure d'un Plugin

```
plugins/mon-plugin/
├── plugin.json          # Contrat, métadonnées, permissions et réglages
├── index.js             # Point d'entrée (ou main.py, mon-plugin.exe, etc.)
├── preview.svg          # Miniature affichée dans le Store (ou preview.png)
└── README.md            # Documentation d'utilisation du plugin
```

---

## 📜 3. Spécification de `plugin.json`

Le fichier `plugin.json` définit l'identité, les autorisations et le comportement de votre extension :

```json
{
  "id": "mon-plugin",
  "name": "Mon Super Plugin",
  "version": "1.0.0",
  "author": "Votre Nom / Studio",
  "type": "community",
  "description": "Description claire et concise de l'extension",
  "min_kairo_version": "0.1.0",
  "permissions": [
    "network",
    "read_games",
    "launch_games",
    "notifications"
  ],
  "entry": "index.js",
  "ui": null,
  "commands": [
    "start",
    "stop",
    "status",
    "custom_action"
  ],
  "settings_schema": {
    "api_token": {
      "type": "string",
      "secret": true,
      "label": "Clé d'API Secrète",
      "default": ""
    },
    "auto_sync": {
      "type": "boolean",
      "label": "Synchronisation automatique",
      "default": true
    },
    "interval_minutes": {
      "type": "number",
      "label": "Intervalle de vérification (minutes)",
      "default": 15
    }
  },
  "sandbox": true
}
```

### Types de Plugins (`type`) :
- `builtin` : Plugin système préinstallé au cœur de KaïroOS (non supprimable, ex: `kairo-remote`).
- `official` : Distribué et validé par l'équipe KaïroOS dans le catalogue officiel.
- `community` : Soumis par la communauté et vérifié par sandbox lors de l'installation.

---

## 🛡️ 4. Matrice des Permissions Système

Pour garantir la sécurité et la stabilité de la borne d'arcade de l'utilisateur, chaque accès sensible requiert une permission explicite :

| Permission | Rôle & Description | Risque Associé |
| :--- | :--- | :--- |
| `network` | Permet d'écouter sur un port local ou d'effectuer des requêtes distantes sur Internet | Écoute réseau / API distantes |
| `read_games` | Autorise la lecture de la liste des jeux, temps de jeu, favoris et jaquettes | Confidentialité de la bibliothèque |
| `launch_games` | Autorise le démarrage ou l'arrêt forcé de jeux et d'émulateurs | Contrôle d'exécution de processus |
| `read_settings` | Permet de lire la configuration de KaïroOS (`settings.json`) | Lecture de chemins et options |
| `write_settings`| Permet de modifier les fichiers de configuration système de KaïroOS | Modification des paramètres |
| `filesystem` | Autorise la lecture et l'écriture restreintes au sous-dossier propre du plugin | Stockage local du plugin |
| `notifications` | Autorise l'envoi de messages alertes et toasts visuels dans l'UI | Affichage à l'écran |

> [!WARNING]
> Tout appel à une action non autorisée (ex: émettre `launch_game` sans déclarer `launch_games`) est automatiquement rejeté par KaïroOS avec un log d'avertissement.

---

## 🔄 5. Protocole de Communication IPC (stdin / stdout)

KaïroOS et votre plugin s'échangent des lignes de texte au format JSON terminées par un saut de ligne `\n`.

### Événements émis par KaïroOS vers le Plugin (sur `stdin`) :
```json
// Détection du lancement d'un jeu par l'utilisateur
{ "event": "game_started", "data": { "game_id": "smw-snes", "title": "Super Mario World", "system": "snes" } }

// Notification de fermeture du jeu
{ "event": "game_stopped", "data": { "game_id": "smw-snes", "duration_seconds": 1420 } }

// Ordre d'exécution d'une commande déclarée
{ "command": "custom_action", "args": {} }
```

### Actions émises par le Plugin vers KaïroOS (sur `stdout`) :
```json
// Lancer un jeu (requiert la permission 'launch_games')
{ "action": "launch_game", "game_id": "smw-snes" }

// Afficher un toast dans l'interface KaïroOS (requiert la permission 'notifications')
{ "action": "notify", "message": "Sauvegarde synchronisée avec succès !", "type": "success" }
```

---

## 💻 6. Exemples d'Implémentation

### Exemple en JavaScript (Node.js) — `index.js`
```javascript
const readline = require('readline');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
  terminal: false
});

// Écoute des événements émis par KaïroOS
rl.on('line', (line) => {
  try {
    const msg = JSON.parse(line.trim());
    
    if (msg.event === 'game_started') {
      // Émettre une notification à l'écran
      const payload = {
        action: 'notify',
        message: `Bonne partie sur ${msg.data.title} !`,
        type: 'info'
      };
      console.log(JSON.stringify(payload));
    }
  } catch (err) {
    // Ignorer les lignes non JSON
  }
});
```

### Exemple en Rust — `src/main.rs`
```rust
use std::io::{self, BufRead, Write};
use serde_json::json;

fn main() {
    let stdin = io::stdin();
    for line in stdin.lock().lines().flatten() {
        if let Ok(msg) = serde_json::from_str::<serde_json::Value>(&line) {
            if msg.get("event").and_then(|e| e.as_str()) == Some("game_started") {
                let notify = json!({
                    "action": "notify",
                    "message": "Partie détectée par le plugin Rust !",
                    "type": "success"
                });
                println!("{}", notify);
                let _ = io::stdout().flush();
            }
        }
    }
}
```

---

## 🧪 7. Tester un Plugin en Local

1. Ouvrez KaïroOS en mode développement :
   ```powershell
   npm run tauri dev
   ```
2. Rendez-vous dans **Paramètres** → **Plugins & Extensions**.
3. Cliquez sur **Dossier Plugins** : l'Explorateur Windows s'ouvre dans le dossier actif.
4. Créez votre dossier (ex: `plugins/mon-test/`) et déposez votre `plugin.json` et votre code.
5. De retour dans l'interface, votre plugin apparaît immédiatement dans l'onglet **Installés** !
6. Vous pouvez basculer le bouton d'activation pour tester le démarrage et le redémarrage.

---

## 🚀 8. Comment Soumettre votre Plugin sur le Store

1. **Forkez** le dépôt officiel : [`KairoOS-Official/kairos-plugins`](https://github.com/KairoOS-Official/kairos-plugins).
2. Créez un dossier dans `community/<votre-plugin-id>/` contenant :
   - `plugin.json`
   - Votre code source ou binaire
   - `preview.png` ou `preview.svg` (ratio recommandé 400x240)
   - `README.md` expliquant l'usage et la configuration
3. Ouvrez une **Pull Request** sur la branche `main`.
4. Après validation par l'équipe, votre plugin sera automatiquement disponible en 1 clic dans l'onglet **Communauté** pour tous les utilisateurs de KaïroOS !

> 💡 **Pas de compte GitHub ?** Vous pouvez aussi soumettre votre plugin par email à plugins@kairo-os.com ou sur Discord dans le canal #plugin-submissions.

---

## 🔗 Liens Utiles

- [Site officiel](https://kairo-os.com)
- [Dépôt principal](https://github.com/KairoOS-Official/KairoOS)
- [Discord](https://discord.gg/kairo-os)
