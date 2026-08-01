# Format de CONTEXT.md

## Structure

```md
# {Nom du contexte}

{Une ou deux phrases : ce qu'est ce contexte et pourquoi il existe.}

## Langue

**Order / Commande** : {Une ou deux phrases : ce que c'est}
_Avoid_ : Purchase, transaction, achat

**Invoice / Facture** : Demande de paiement envoyée au client après la livraison.
_Avoid_ : Bill, payment request

**Customer / Client** : Personne physique ou morale qui passe des commandes.
_Avoid_ : Client, buyer, account, acheteur
```

## Règles

- **Distingue synonymes complets et partiels.** Les synonymes complets (interchangeables dans tout scénario : review / revue) s'écrivent en co-canoniques via `/` avec des espaces. Les partiels (divergent dans un certain scénario : client / acheteur) — un dans le canon, les autres dans `_Avoid_`.
- **Pour les concepts liés au code — l'anglais d'abord.** Si un terme correspond à du code (classe, fonction, table, fichier), la première variante dans le canon est anglaise/latine : `Order / Commande`. C'est un signal, pas une contrainte ; il n'y a pas de champ explicite « forme pour le code ».
- **Sois catégorique.** Fige le canon — un terme ou un jeu de synonymes complets via `/` — et mets tout le superflu dans `_Avoid_`.
- **Garde les définitions courtes.** Une à deux phrases maximum. Définis ce que c'EST, pas ce que ça fait. La définition — dans la langue de travail de l'équipe.
- **N'inclus que les termes spécifiques au contexte de ce projet.** Les concepts généraux de programmation (timeouts, types d'erreur, motifs utilitaires) n'ont pas leur place ici, même si le projet les utilise abondamment. Avant d'ajouter un terme, demande-toi : cette notion est-elle unique à ce contexte ou est-ce un concept général de programmation ? Seul le premier cas appartient au glossaire.
- **Regroupe les termes sous des sous-titres**, quand des grappes naturelles se dessinent. Si tous les termes relèvent d'un même domaine cohérent, une liste plate est normale.

## Dépôts à contexte unique et à contextes multiples

**Un seul contexte (la plupart des dépôts) :** un unique `CONTEXT.md` à la racine du dépôt.

**Plusieurs contextes :** `CONTEXT-MAP.md` à la racine énumère les contextes, où ils vivent et comment ils sont liés entre eux :

```md
# Carte des contextes

## Contextes

- [Ordering](./src/ordering/CONTEXT.md) — reçoit et suit les commandes des clients
- [Billing](./src/billing/CONTEXT.md) — émet les factures et traite les paiements
- [Fulfillment](./src/fulfillment/CONTEXT.md) — gère la préparation et l'expédition depuis l'entrepôt

## Liens

- **Ordering → Fulfillment** : Ordering publie des événements `OrderPlaced` ; Fulfillment les consomme pour lancer la préparation
- **Fulfillment → Billing** : Fulfillment publie des événements `ShipmentDispatched` ; Billing les consomme pour émettre une facture
- **Ordering ↔ Billing** : Types partagés pour `CustomerId` et `Money`
```

Le skill détermine lui-même quelle structure s'applique :

- Si `CONTEXT-MAP.md` existe — il le lit pour trouver les contextes
- S'il n'existe qu'un `CONTEXT.md` à la racine — un seul contexte
- S'il n'y a ni l'un ni l'autre — il crée le `CONTEXT.md` racine paresseusement, quand le premier terme sera figé

Quand il y a plusieurs contextes, détermine auquel se rattache le sujet courant. Si ce n'est pas clair — demande.
