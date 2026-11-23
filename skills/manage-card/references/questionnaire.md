# Format du questionnaire (artefact `questions`)

Un fichier TOML, une table `[[questions]]` par question. Dépose-le : `debussy card artifact --id <n> --type questions --file ${TMPDIR:-/tmp}/card-<n>/questions-<n>.toml` (avec `--interactive` s'il part avec son cadrage).

```toml
[[questions]]
id = "portee"
text = "Le skill vit-il dans l'instance ou dans le core ?"
context = "Dans l'instance, il se met à jour à la main quand la CLI change. Dans le core, il suit les commits de la CLI mais impose un paquet et un ADR."
recommend = "instance"
why = "Avis de l'architecte : le core ne livre pas de skills aujourd'hui."
options = [
  { id = "instance", label = "Dans l'instance" },
  { id = "core", label = "Dans le core" },
]
```

Champs : `id` (court, sans espace), `text`, `context`, `options` (chacune `id` et `label`) ; `recommend` (l'`id` d'une option) et `why` quand tu as un avis.

## Règle de lisibilité

Le Directeur lit chaque question plus tard, sans ta session ni le cadrage sous les yeux. Avant de déposer, relis-la comme lui.

- `text` est le titre : **une ligne**, **sans numéro** en tête (« Q1. », « 2) »).
- `context` est **toujours rempli** : 2 à 3 lignes, environ 250 caractères. Il dit ce qui est demandé et ce qu'engage chaque option.
- **Aucun renvoi non défini** : « Q1 », « S1 », « l'option B », « comme avant » ne sont écrits qu'avec ce qu'ils désignent.
- Le titre et le `context` doivent suffire seuls. Sinon, corrige avant de déposer.

Le board peut préciser cette règle dans la `description` du type `questions` (`debussy card show --id <n>`).

## Après les réponses

Les réponses te sont dites par un message. Après la dernière, intègre-les au cadrage et redépose-le. Pour répondre au Directeur dans le journal : voir « Journal » dans `SKILL.md`.
