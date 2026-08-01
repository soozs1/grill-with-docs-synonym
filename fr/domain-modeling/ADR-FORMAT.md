# Format des ADR

Les ADR se trouvent dans `docs/adr/` et utilisent une numérotation continue : `0001-slug.md`, `0002-slug.md`, etc. Crée le répertoire `docs/adr/` paresseusement — seulement quand le premier ADR sera nécessaire.

## Modèle

```md
# {Titre court de la décision}

{1 à 3 phrases : le contexte, ce qui a été décidé et pourquoi.}
```

Et c'est tout. Un ADR peut tenir en un paragraphe. La valeur est de consigner *qu'une* décision a été prise et *pourquoi* — pas de remplir des sections.

## Sections facultatives

Inclus-les seulement quand elles ajoutent réellement de la valeur. La plupart des ADR n'en ont pas besoin.

- **Statut** dans le frontmatter (`proposed | accepted | deprecated | superseded by ADR-NNNN`) — utile quand on revient sur les décisions
- **Options envisagées** — seulement quand les alternatives écartées méritent d'être retenues
- **Conséquences** — seulement quand il faut souligner des effets en aval peu évidents

## Numérotation

Parcours `docs/adr/` pour trouver le plus grand numéro existant et augmente-le de un.

## Quand proposer un ADR

Les trois doivent être vrais :

1. **Difficile à inverser** — le coût de changer d'avis plus tard est sensible
2. **Pas évident sans contexte** — un futur lecteur regardera le code et demandera « pourquoi ont-ils fait ça comme ça ? »
3. **Résultat d'un vrai compromis** — il y avait de vraies alternatives, et vous en avez choisi une pour des raisons précises

Si la décision est facile à inverser — passe, tu la inverseras tout simplement. Si elle ne surprend pas — personne ne se demandera « pourquoi ». S'il n'y avait pas de vraie alternative — rien à consigner, sinon « nous avons fait l'évidence ».

### Ce qui convient

- **Forme architecturale.** « Nous utilisons un monorepo. » « Le modèle d'écriture est event-sourced, le modèle de lecture est projeté dans Postgres. »
- **Motifs d'intégration entre contextes.** « Ordering et Billing communiquent par événements de domaine, et non par HTTP synchrone. »
- **Choix technologiques avec verrouillage (lock-in).** Base de données, bus de messages, fournisseur d'authentification, cible de déploiement. Pas chaque bibliothèque — seulement celles dont le changement prend un trimestre entier.
- **Décisions de frontières et de périmètre de responsabilité.** « Les données Customer appartiennent au contexte Customer ; les autres contextes n'y font référence que par ID. » Les « non » explicites sont aussi précieux que les « oui ».
- **Écarts délibérés par rapport au chemin évident.** « Nous utilisons du SQL pur plutôt qu'un ORM, parce que X. » Tout cas où un lecteur raisonnable supposerait le contraire. Cela retiendra le prochain ingénieur de « corriger » ce qui a été fait intentionnellement.
- **Contraintes invisibles dans le code.** « Nous ne pouvons pas utiliser AWS à cause d'exigences de conformité. » « Le temps de réponse doit être inférieur à 200 ms à cause d'un contrat avec une API partenaire. »
- **Alternatives écartées, quand le rejet n'est pas évident.** Si vous avez envisagé GraphQL et choisi REST pour des raisons subtiles — consignez-le, sinon dans six mois quelqu'un proposera à nouveau GraphQL.
