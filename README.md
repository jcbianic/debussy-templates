# debussy-templates

Modèles d'agents et avatars à tirer dans une instance [Debussy](https://github.com/jcbianic/debussy). Le dépôt ne contient que des fichiers de données : rien à installer, rien à compiler.

## Brancher le dépôt et tirer

Depuis une instance Debussy :

```sh
# 1. brancher le dépôt comme source
debussy skills source add jcbianic/debussy-templates

# 2. tirer un agent
debussy instance agents pull --source jcbianic/debussy-templates

# 3. tirer les avatars proposés
debussy instance avatars pull
```

La page **Bibliothèque** de la console fait la même chose, sans ligne de commande.

Seule la branche par défaut compte : il n'y a pas de branche par version de Debussy.

## Format

- Un fichier par modèle, sans index ni métadonnées à côté.
- Agents : `agents/<mot>.toml`.
- Avatars : `avatars/<mot>.png`, image matricielle de 2 Mio au plus. Jamais de SVG.
- Aucune ligne `template` dans un agent.
- Pas de `README` ni de `CREDITS` dans `avatars/` : il s'afficherait comme un avatar. Les crédits sont à la racine, dans `CREDITS.md`.

## Compatibilité

La référence est le dernier tag publié de Debussy, aujourd'hui `v0.2.0-nightly.10`. Un modèle ne cite une commande `debussy …` qu'une fois celle-ci publiée dans cette référence.

## Skills et boards

- `skills/manage-card/` : un skill (`SKILL.md` et `references/`) sur la mécanique de la CLI `debussy card`.
- `boards/` : deux boards d'exemple, `tailles.toml` et `debussy-product.toml`. Ce dernier garde la forme ancienne du format : `debussy board check --file` l'accepte mais signale des champs à migrer.

Les autres skills que citent les agents s'affichent « manquants » au tirage tant que leur source n'est pas branchée. Elles sont publiques :

```sh
debussy skills source add mattpocock/skills
debussy skills source add pbakaus/impeccable
```

## Contributions

Fermées. La branche `main` est réservée à son propriétaire (`.github/CODEOWNERS`) : un agent porte une autorité d'exécution (`allowed_tools`), donc une modification venue de l'extérieur atteindrait toutes les instances qui le tirent.

## Licence et crédits

Les fichiers TOML et ce README sont sous licence MIT (voir `LICENSE`). Les images suivent leur propre licence ; `CREDITS.md` donne la provenance de chacune (œuvre, auteur, source, statut). Seul le portrait de Vivaldi a une provenance vérifiée : celle des autres avatars est signalée comme non vérifiée dans `CREDITS.md`.
