---
name: manage-agent
description: Comment lire, créer et fusionner des agents avec la CLI `debussy agent`, et le format des fichiers `agents/<mot>.toml`. À charger quand tu dois savoir qui l'instance déclare ou préparer un agent.
---

# Gérer les agents

La CLI est la source de vérité : pour une option, `debussy agent <commande> --help`. Dans une commande, `<a>` est l'id de l'agent (le nom de son fichier dans `agents/`).

## Lire

- `debussy agent --help` : toutes les commandes.
- `debussy agent list` : les agents déclarés (par le cœur et par `agents/`), modèle, effort, département. Un fichier illisible y figure **avec la raison** : c'est la vérification d'un fichier.
- `debussy agent show --id <a>` : qui il est, son modèle, ses permissions, ses postes, ses instructions, ses affectations.

## Créer

`debussy agent create --name <nom>` : l'id est émis et le fichier `agents/<id>.toml` écrit dans le dépôt de l'instance, **à commiter**. Complète ensuite le fichier, puis vérifie.

## Format de `agents/<mot>.toml`

Pour un modèle, regarde `agents/` de ce dépôt. Champs en tête de fichier :

- `department` : le département.
- `model`, `effort` : modèle et effort.
- `skills` : liste de noms de skills.
- `allowed_tools` : liste des outils permis. C'est une autorité d'exécution : n'en donne pas plus que le rôle n'en demande.
- `instructions` : texte (chaîne multiligne `"""`).
- Table `[persona]` : `name`, `personality`, `face` (chemin d'un avatar, `avatars/<mot>.png`).
- Aucune ligne `template`.

## Vérifier

1. `debussy agent list` : le fichier est listé sans erreur.
2. `debussy agent show --id <a>` : les champs sont ceux que tu as écrits.

Si une commande n'apparaît pas dans `debussy agent --help`, elle n'existe pas : ne l'invente pas.

## Fusionner

`debussy agent merge --id <a> --into <b>` sans `--yes` montre le plan et s'arrête : lis-le. Avec `--yes`, c'est définitif (le fichier de `<a>` est supprimé) et c'est l'acte du Directeur : une session est refusée.
