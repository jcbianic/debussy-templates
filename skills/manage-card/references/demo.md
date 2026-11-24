# Manifeste de démo (artefact `demo`)

La démo se dépose **en un seul appel**, le manifeste et toutes les images ensemble :

`debussy card artifact --id <n> --type demo --file ${TMPDIR:-/tmp}/card-<n>/manifeste.toml --file ${TMPDIR:-/tmp}/card-<n>/<image>.png …`

Deux dépôts au même commit font une version. La PR est déjà ouverte en brouillon depuis `build` : dépose la démo quand la construction est finie, avant de passer en `validation`.

## Le manifeste

```toml
commit = "<commit où les captures sont prises>"

[[shots]]
file = "avant-liste.png"
key = "liste"
side = "before"
description = "La liste des cartes : titre tronqué"

[[shots]]
file = "apres-liste.png"
key = "liste"
side = "after"
description = "La liste des cartes : titre renommé"
```

- `commit` en tête : le commit où les captures sont prises.
- Un `[[shots]]` par image : `file`, `key`, `description`, `side` (`"before"` ou `"after"`), et `config` si ce n'est pas la configuration par défaut.
- Pour une même `key`, l'avant et l'après sont pris au même commit.
- Configuration par défaut : desktop, clair. Mobile ou sombre seulement si le changement les touche ; les axes que le board accepte sont dans sa déclaration du type `demo`.
