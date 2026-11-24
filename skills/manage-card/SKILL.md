---
name: manage-card
description: La mécanique de la CLI `debussy card` pour mener une carte : se réveiller et attendre, déposer un artefact, écrire au journal, déléguer, renommer un titre provisoire. À charger quand tu es le handler ou l'orchestrateur d'une carte.
---

# Mener une carte : la mécanique de la CLI

Ce skill dit comment on se sert de la CLI. Il ne dit ni les transitions (`debussy card show --id <n>` liste celles qui sont permises et pourquoi les autres ne le sont pas), ni la procédure de ton board, ni git. Dans une commande, `<n>` est le numéro de la carte.

## Au réveil

1. Lis la carte : `debussy card show --id <n>`.
2. Si son titre est provisoire (une phrase tronquée, finie par « … »), renomme-la tout de suite en une ligne de 120 caractères au plus, qui dit le sujet :
   `debussy card rename --id <n> --title "…"`
   Le journal garde l'ancien titre ; le numéro, la branche et le handler ne changent pas.
3. Dis où tu en es, en quelques mots : `debussy card progress --id <n> "…"`.

## Attendre

Pour attendre (une délégation, le Directeur, une réponse), **termine ton run**. Tu es réveillé par un message : fin d'un run délégué, réponse, message du Directeur. Ne surveille pas, ne boucle pas, ne dors pas.

Seule une ligne de journal de genre `question` fait « attendre le Directeur » : un `progress` qui dit « j'attends » ne le prévient pas. Pose une question seulement quand tu es bloqué sans voie sûre, puis termine ton run.

Ne termine jamais ton run tant qu'un job à toi tourne.

## Déposer un artefact

1. Écris chaque fichier dans un dossier de travail hors de tout dépôt : `${TMPDIR:-/tmp}/card-<n>/`. Jamais à la racine de l'instance ni dans un clone : Debussy garde sa propre copie. Nom parlant, sans point initial, qui dit la carte et la pièce : `cadrage-<n>.md`, `questions-<n>.toml`.
2. Dépose avec la commande exacte :
   `debussy card artifact --id <n> --type <type> --file <chemin>`
   Les types sont ceux que déclare le board : `debussy card show --id <n>` les montre. Pour un lien : `--url <https://…> --title "…"` au lieu de `--file`.
3. Un appel dépose **une version**. Plusieurs pièces d'une même version (un manifeste et ses images) se déposent en un seul appel : `--file` répété.
4. Vérifie la ligne `deposited …` que la commande rend. Sans elle, rien n'est déposé.
5. Relire un dépôt : `debussy card artifact --id <n> --type <type> --show`.

### Questionnaire

`--type questions` : voir `references/questionnaire.md` pour le format et la règle de lisibilité.

Un questionnaire déposé avec son cadrage prend `--interactive` : le verdict n'est alors pas demandé tant que les questions n'ont pas reçu réponse et que les pièces n'ont pas été redéposées.

Les réponses du Directeur périment ce que tu as déposé avant elles. Après la dernière, intègre-les, puis **redépose** les pièces concernées (le cadrage au moins) : c'est ce redépôt qui fait demander le verdict.

### Démo

`--type demo` : manifeste et images en un seul appel ; voir `references/demo.md`.

## Journal

`debussy card write --id <n> --kind <genre> "…"` (ou `--file <chemin>` pour un texte long).

- Les genres sont ceux que le type de la carte déclare : `debussy card show --id <n>` les liste, avec qui peut les écrire.
- `question` : au Directeur ; elle seule le fait attendre.
- `response` : ta réponse brève à un `message` libre du Directeur. Un message n'est ni une décision ni un verdict et ne fait avancer aucun état ; s'il te demande de décider à sa place, pose plutôt une `question`.
- `progress` : où tu en es (`debussy card progress --id <n> "…"` fait de même, sur une ligne).
- Une `answer` du Directeur nomme la ligne qu'elle répond (`--to`) ; tu n'en écris pas.

## Déléguer

`debussy agent delegate --to <agent> --brief "sur quoi porte la délégation" "ce qu'on lui demande"`

- Le brief dit le sujet, le texte dit la demande.
- Pour reparler à la même délégation, réutilise le même `--name`. Sans `--name`, une nouvelle délégation naît.
- `--cwd <chemin absolu>` : le dossier où elle travaille (par défaut, le tien).
- Ne surveille pas : termine ton run, tu es réveillé quand elle finit, avec son résultat.
- Un délégué ne voit ni ce skill ni tes consignes. Si tu lui confies un dépôt d'artefact ou un questionnaire, donne-lui les règles qui le concernent (dossier hors dépôt, format de `references/questionnaire.md`, `description` du type d'artefact du board).
