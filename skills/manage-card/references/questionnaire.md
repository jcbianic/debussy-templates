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

## Le graphe (artefact `dag`)

Pour une demande complexe : des décisions qui s'enchaînent (le choix de l'une change ce qu'on demande ensuite) ou une structure du Product touchée. Une demande simple garde le questionnaire à plat. Tu es seul à rédiger le graphe ; un délégué fournit du contenu et ne dépose rien.

Un fichier TOML : des `[[nodes]]` (les boîtes), chacune avec ses `[[nodes.questions]]` au format ci-dessus. Dépose-le : `debussy card artifact --id <n> --type dag --file ${TMPDIR:-/tmp}/card-<n>/dag-<n>.toml`.

```toml
[[nodes]]
id = "portee"
label = "Où vit le skill"
context = "Avant tout le reste : l'endroit fixe l'ADR, le paquet et qui le met à jour."

[[nodes.questions]]
id = "lieu"
text = "Le skill vit-il dans l'instance ou dans le core ?"
context = "Dans l'instance, mise à jour à la main. Dans le core, il suit les commits de la CLI mais impose un paquet et un ADR."
recommend = "instance"
options = [
  { id = "instance", label = "Dans l'instance" },
  { id = "core", label = "Dans le core" },
]

[[nodes]]
id = "paquet"
label = "Le paquet du core"
needs = ["portee"]
context = "Ne se pose que si le skill va dans le core."

[[nodes.questions]]
id = "nom"
after = { question = "lieu", option = "core" }
text = "Quel nom pour le paquet ?"
context = "Le nom figure dans l'ADR et dans l'import ; il est coûteux à changer ensuite."
options = [
  { id = "skills", label = "skills" },
  { id = "catalogue", label = "catalogue" },
]
```

- **Boîte** : `id`, `label`, `needs` (les boîtes qu'elle attend), `context`. Une boîte est bloquée tant qu'un de ses besoins n'est pas traité ; sans question, elle est « à préciser » (en pointillé) : tu la remplis au fil des réponses.
- **Bornes** : 16 boîtes au plus, 6 questions au plus par boîte, 3 besoins au plus par boîte (demandé, non contrôlé). Les ids de question sont uniques dans tout le fichier. Pas de cycle.
- **`after`** : permis seulement entre les questions d'une même boîte ; la question n'est posée que si l'option nommée est choisie.
- **Le Directeur envoie une boîte entière à la fois** ; chaque envoi te réveille avec un message : la boîte, ses réponses, ce qu'elle débloque. Une note seule vaut réponse ; si tu y lis un refus, rouvre la boîte.
- **Redépôt** : tu redéposes le fichier entier. Pour changer une boîte déjà traitée, écris `reopen = true` sur la boîte, ou `dropped = ["id"]` pour retirer une question ; sans cela le dépôt est refusé. Une boîte traitée le reste quand un de ses besoins est rouvert : c'est à toi de rouvrir ce qui ne tient plus.
- **Verdict** : il n'est demandé que quand toutes les boîtes sont traitées et que le cadrage a été redéposé après le dernier envoi.
- **Relire** : `debussy card artifact --id <n> --type dag --show`.

La règle de lisibilité s'applique à chaque boîte et chaque question.
