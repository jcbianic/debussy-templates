---
name: manage-board
description: Comment configurer un board avec la CLI `debussy board` : états, équipe, apparence, types de carte, steward. Chemin sûr (lire, valider, puis éditer). À charger quand tu dois lire ou modifier la définition d'un board.
---

# Configurer un board

La CLI est la source de vérité : ce skill donne l'ordre des gestes, pas la doc des commandes. Pour une option, `debussy board <commande> --help`. Ne devine ni un nom d'état, ni un type de carte, ni une icône, ni une couleur : lis-les.

Dans une commande, `<b>` est l'id du board (le nom de son fichier).

## 1. Lire d'abord

- `debussy board --help` : toutes les commandes.
- `debussy board list` : les boards de l'instance.
- `debussy board show --id <b>` : états, colonnes, clone, steward, journal.
- `debussy board show --id <b> --workflow` : états et types de carte, en mots.
- `debussy board show --id <b> --card-type <type>` : un type, ce que portent ses cartes et ce que son journal accepte.
- `debussy board show --id <b> --definition` : le texte actif de la définition.
- `debussy board file --id <b>` : le fichier du board face à la définition active (identique, modifié, invalide et pourquoi, absent).

## 2. Choisir le geste le plus étroit

| Je veux changer | Geste |
|---|---|
| nom, description, icône, couleur | `debussy board set --id <b> …` (ce qui n'est pas donné ne bouge pas) |
| l'équipe | `debussy board recruit` (sans `--agent` : liste les recrutables), puis `--agent <a>` (et `--post` pour un poste) ; `debussy board dismiss --agent <a>` |
| un type de carte | `debussy board type --id <b> --type <t> --brief "…"` : le steward l'écrit dans le fichier |
| le steward, ce qui veille | `debussy board show` ; `debussy board stewards --id <b>` ; `debussy board tell` pour lui parler |
| états, transitions, le reste | édition de la définition entière (ci-dessous) |

Les valeurs d'icône et de couleur sont celles que Debussy connaît : si `board set` en refuse une, lis son message, ne la contourne pas.

## 3. Éditer la définition entière

1. Pars du texte actif : `debussy board show --id <b> --definition`, copié dans un fichier de travail **hors de tout dépôt** (`${TMPDIR:-/tmp}/board-<b>/<b>.toml`).
2. Modifie ce fichier.
3. Valide : `debussy board check --file <chemin>`. Sortie 2 : la première erreur est dite, corrige-la et recommence. Un signalement de champ à migrer n'est pas une erreur, mais lis-le.
4. Applique : `debussy board edit --id <b> --file <chemin>`. Le fichier est vérifié en entier ; en cas d'erreur, rien ne change.
5. Relis : `debussy board file --id <b>` (« changed » : adopté au prochain passage), puis `board show --workflow`.

Des exemples de définitions sont dans `boards/` de ce dépôt.

## Règles

- Un geste étroit (`set`, `recruit`, `type`) avant une édition entière.
- Jamais d'`edit` sans `check` juste avant, sur le même fichier.
- Ne supprime pas un board (`board delete`) : c'est l'acte du Directeur.
- Un état ou un type que tu retires peut porter des cartes : regarde `board show` avant.
