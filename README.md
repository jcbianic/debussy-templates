# debussy-templates

Modèles d'agents, avatars, palettes et styles à tirer dans une instance [Debussy](https://github.com/jcbianic/debussy). Le dépôt ne contient que des fichiers de données : rien à installer, rien à compiler.

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
- Palettes : `palettes/<mot>.toml`, un `name` et deux tables `[light]` et `[dark]` qui donnent chaque jeton couleur de la console (hexadécimal, `rgb()` ou `hsl()` seulement ; `docked` opaque). Ni style, ni CSS : les jetons composés sont ceux de la console (ADR 0093, D6). Une erreur se lit `BAD_PALETTE`, avec la clé et le mode.
- Styles : `styles/<mot>.toml`, un `name`, une `base` (l'un des styles de la console) et des clés à valeurs typées et bornées (polices, rayons, titres, densité, élévation, verre, texture). Aucun CSS : une clé inconnue ou une valeur hors bornes se lit `BAD_STYLE`, avec la clé (ADR 0093, D10).
- Aucune ligne `template` dans un agent.
- Pas de `README` ni de `CREDITS` dans `avatars/` : il s'afficherait comme un avatar. Les crédits sont à la racine, dans `CREDITS.md`.

## Compatibilité

La référence est le dernier tag publié de Debussy, aujourd'hui `v0.2.0-nightly.12`. Les styles ne se tirent qu'avec la première nightly qui contient la carte 166 (#977) : aucun tag ne la contient encore, `nightly.12` non plus. Un modèle ne cite une commande `debussy …` qu'une fois celle-ci publiée dans cette référence.

## Palettes

- `palettes/lanterne-8-bits.toml` : la nuit de la lanterne, indigo et or ; papier crème en clair.
- `palettes/la-mer.toml` : estampe bleu de Prusse, vermillon et écume, nettement distincte de la palette Estampe livrée avec Debussy.

Dans chacune, « humain » est éloigné de « échec », et le texte et les états gardent un contraste d'au moins 4,5:1 sur le fond et la surface, en clair comme en sombre. Les palettes se tirent depuis la Bibliothèque d'une instance en `nightly.12` ou plus récente, puis se choisissent dans Apparence.

## Styles

- `styles/lanterne-8-bits.toml` : le style pixel de Lanterne, sur la base `estampe` : trait d'un pixel, panneaux carrés, titres en capitales monospace, ombre dure de 4 px, trame de 8 px. Il va avec la palette du même nom, sans en dépendre.

## Skills et boards

- `skills/manage-card/` : un skill (`SKILL.md` et `references/`) sur la mécanique de la CLI `debussy card`.
- `skills/manage-board/` : un skill sur la configuration d'un board (états, équipe, apparence, types de carte, steward) avec `debussy board` : lire, valider (`board check`), puis éditer.
- `skills/manage-agent/` : un skill sur les agents avec `debussy agent` (`list`, `show`, `create`, `merge`) et le format de `agents/<mot>.toml`.
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
