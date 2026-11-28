---
name: fix-instance
description: Comment remettre une instance Debussy d'aplomb avec la CLI : lire, diagnostiquer (agents illisibles, board invalide, skills manquants, changement non repris), réparer par le geste le plus étroit, vérifier. Le ledger est hors périmètre. À charger quand quelque chose dans l'instance ne se comporte pas comme ses fichiers le disent.
---

# Réparer une instance

La CLI est la source de vérité : ce skill donne l'ordre des gestes, pas la doc des commandes. Pour une option, `debussy <commande> --help`. Si une commande n'apparaît pas dans `--help`, elle n'existe pas : ne l'invente pas.

## Hors périmètre : le ledger

Si `debussy` refuse le ledger (message « ledger schema vN… ») ou si la garde du ledger alerte : **arrête-toi et préviens le Directeur**. Ne cherche pas un autre binaire qui le lise, ne tente aucune réparation. Aucune suite de ce skill ne s'y applique.

## 1. Lire d'abord

1. `debussy instance show` : quel Debussy tourne, pour quelle instance, où est son état.
2. `debussy agent list` : les agents déclarés. Un fichier illisible y figure **avec la raison** : c'est le premier diagnostic.
3. `debussy board list`, puis `debussy board file --id <b>` : le fichier du board face à sa définition active (identique, modifié, invalide et pourquoi, absent).
4. `debussy library source list` : les sources branchées, et ce que les agents citent sans l'avoir (skills « manquants »).

Note ce que tu as lu avant de toucher quoi que ce soit.

## 2. Diagnostiquer

| Constat | Cause probable | Réparation |
|---|---|---|
| agent listé avec une erreur | fichier `agents/<a>.toml` mal formé (champ, TOML) | corrige le fichier, voir le format dans le skill `manage-agent` |
| board « invalide » | définition refusée | `debussy board check --file <chemin>` dit la première erreur ; skill `manage-board` |
| board ou agent « modifié », non repris | le changement n'est pas encore chargé | `debussy instance reload` |
| skill cité par un agent mais manquant | source non branchée, ou skill non tiré | `debussy library source add <dépôt>`, puis `debussy skills pull` |
| le reload dit qu'il faut redémarrer | un changement n'est pris qu'au redémarrage | `debussy instance restart` |

## 3. Réparer : le geste le plus étroit

- Un fichier à la fois, le plus petit changement qui lève l'erreur.
- Board : pars du texte actif (`debussy board show --id <b> --definition`) copié **hors de tout dépôt**, modifie, `debussy board check --file <chemin>`, puis `debussy board edit --id <b> --file <chemin>`. Jamais d'`edit` sans `check` juste avant, sur le même fichier.
- Agent : édite `agents/<a>.toml` ; n'élargis jamais `allowed_tools` pour faire taire une erreur.
- `debussy instance reload` avant `debussy instance restart` ; le redémarrage seulement si le reload le demande.
- Un fichier de l'instance se commite dans son dépôt, comme tout changement : travaille dans un worktree, pas à la racine.

## 4. Vérifier

Refais la lecture de l'étape 1 : l'erreur a disparu, rien d'autre n'a bougé. Dis en quelques lignes ce qui était cassé, ce que tu as changé, ce que tu as vérifié.

## Ce que tu ne fais jamais

- Toucher au ledger, ou contourner son refus.
- `debussy board delete` et `debussy agent merge --yes` : actes du Directeur.
- Supprimer un fichier ou un état pour « faire passer » une vérification : lis d'abord ce qui en dépend.
- Réparer au-delà de ce que le diagnostic montre.
