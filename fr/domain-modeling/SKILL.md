---
name: domain-modeling
description: Construis et affine le modèle de domaine du projet. À utiliser quand l'utilisateur veut figer la terminologie du domaine ou un langage commun (ubiquitous language), consigner une décision d'architecture (ADR), ou quand un autre skill a besoin d'entretenir le modèle de domaine.
---

# Modélisation de domaine

Construis et affine activement le modèle de domaine du projet au fil de la conception. C'est une discipline *active* : contester les termes, imaginer des cas limites et consigner le glossaire et les décisions au moment précis où ils se cristallisent. (Se contenter de *lire* `CONTEXT.md` pour le vocabulaire — ce n'est pas ce skill ; c'est une habitude d'une ligne, accessible à n'importe quel skill. Ce skill — pour les cas où tu modifies le modèle, pas simplement où tu t'en sers.)

## Structure des fichiers

La plupart des dépôts ont un seul contexte :

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

Si `CONTEXT-MAP.md` existe à la racine, le dépôt contient plusieurs contextes. La carte indique où se trouve chacun :

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                       ← décisions à l'échelle du système
├── src/
│   ├── ordering/
│   │   ├── CONTEXT.md
│   │   └── docs/adr/              ← décisions propres à un contexte
│   └── billing/
│       ├── CONTEXT.md
│       └── docs/adr/
```

Crée les fichiers paresseusement — seulement quand il y a quelque chose à consigner. Si `CONTEXT.md` n'existe pas encore, crée-le quand le premier terme sera figé. Si `docs/adr/` n'existe pas, crée-le quand le premier ADR sera nécessaire.

## Langue

Mène la session dans la langue de travail de l'utilisateur (par défaut — français (français)).

Dans le glossaire, distingue le *terme canonique* et sa *définition*.

- **Le canon reflète le code.** Consigne le terme canonique tel qu'il vit dans le projet. Dans un projet vierge (greenfield) — demande une fois à l'équipe, en proposant d'emblée des variantes bilingues (« on l'appelle *Order / Commande* ? »), et fige le choix.
- **Synonymes complets — co-canoniques, via `/`.** Si des termes sont identiques en sens et interchangeables dans tout scénario (review / revue, parsing / analyse), écris-les dans le canon séparés par `/` avec des espaces : `Order / Commande`. Il peut y en avoir plus de deux.
- **Synonymes partiels — dans `_Avoid_`.** Si des termes sont proches mais divergent en sens dans un certain scénario (compte / utilisateur / client), choisis un canon et mets les autres dans `_Avoid_`.
- **L'ordre = signal doux pour le code.** Pour les concepts liés au code (qui correspondent à une classe, une fonction, une table, un module, un fichier), le terme anglais/latin vient en premier dans le canon : `Order / Commande`. Il n'y a pas de champ explicite « forme pour le code », et ce n'est pas une contrainte : l'agent s'appuie sur la forme anglaise pour les identifiants, mais le choix des moyens de nommage est libre. Les concepts purement conversationnels peuvent venir en français d'abord.
- **Définition** — dans la langue de travail, une ou deux phrases, « ce que c'est ». `_Avoid_` — liste séparée par des virgules (l'ordre n'importe pas), incluant les formes rejetées tant anglaises que françaises.

Le but — que les termes du glossaire correspondent à la façon dont ils sont nommés dans le code et dans le langage de l'équipe. Les termes approuvés sont réutilisés par l'agent ; la couche de persistance est `CONTEXT.md`. Si l'équipe préfère une autre convention (glossaire entièrement francophone, translittération dans les identifiants) — d'accord, sois cohérent et fige le choix dans un ADR.

## Au fil de la session

### Recoupe avec le glossaire

Quand l'utilisateur emploie un terme en contradiction avec le langage existant dans `CONTEXT.md`, relève-le immédiatement : « Dans le glossaire, "annulation" est définie comme X, mais tu sembles vouloir dire Y — alors, lequel des deux ? »

### Affine les formulations floues

Quand l'utilisateur utilise des termes vagues ou surchargés, propose un terme canonique précis : « Tu dis "compte" — tu veux dire Customer ou User ? Ce sont deux choses différentes. »

### Établis les synonymes complets

Quand un terme a des candidats synonymes, propose-les à l'utilisateur : approuver, rejeter ou reporter (« on l'appelle *grilling / griller*, ou on en discute plus tard ? »). « Reporter » est une échappatoire dans le dialogue ; seuls les synonymes approuvés entrent dans l'entrée du glossaire.

L'heuristique — **le test de substitution** : des termes sont des synonymes complets seulement si on peut les intervertir dans n'importe quelle phrase et n'importe quel scénario sans changer le sens. Si tu peux construire un scénario où ils divergent — **tu dois une fois l'exposer explicitement** (« voici un scénario où X et Y sont deux choses différentes ; on fusionne vraiment ? »). C'est un avertissement de risque, pas un veto : la décision revient à l'utilisateur. Il approuve, ayant vu le contre-exemple — on écrit le synonyme complet via `/`. Il rejette — un seul canon, le reste dans `_Avoid_`. L'avertissement est unique, sans rabâcher. Les termes approuvés, réutilise-les ensuite et fige-les dans `CONTEXT.md` sur place.

### Discute des scénarios concrets

Quand les liens entre notions du domaine sont discutés, éprouve-les sur des scénarios concrets. Imagine des scénarios qui sondent les cas limites et forcent l'utilisateur à tracer clairement les frontières entre concepts.

### Recoupe avec le code

Quand l'utilisateur affirme comment quelque chose fonctionne, vérifie que cela concorde avec le code. Si tu trouves une contradiction — mets-la à jour : « Le code annule les commandes entièrement, mais tu viens de dire qu'une annulation partielle est possible — lequel est vrai ? »

### Mets à jour CONTEXT.md sur place

Quand un terme est figé, mets immédiatement à jour `CONTEXT.md`. Ne les accumule pas par lots — fige-les au fur et à mesure. Utilise le format de [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md). `CONTEXT.md` doit être entièrement exempt de détails d'implémentation. Ne transforme `CONTEXT.md` ni en spécification, ni en brouillon, ni en dépôt de décisions d'implémentation. C'est un glossaire et rien de plus.

### Propose des ADR avec parcimonie

Propose de créer un ADR seulement quand les trois sont vrais :

1. **Difficile à inverser** — le coût de changer d'avis plus tard est sensible
2. **Pas évident sans contexte** — un futur lecteur demandera « pourquoi ont-ils fait ça comme ça ? »
3. **Résultat d'un vrai compromis** — il y avait de vraies alternatives, et vous en avez choisi une pour des raisons précises

S'il manque au moins un des trois — passe l'ADR. Utilise le format de [ADR-FORMAT.md](./ADR-FORMAT.md).
