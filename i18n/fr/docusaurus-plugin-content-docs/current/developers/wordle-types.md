---
sidebar_label: Types
---

# Types de Wordle

Durant les prochaines étapes, nous allons créer des types qui seront utilisés par les messages que nous avons créés.

## Types de Scaffoling Wordle

```sh
ignite scaffold map wordle word submitter --no-message
```

Ce type est une carte appelée `Wordle` avec deux valeurs appelées `mot` et `déposant`. `submitter` est l'adresse de la personne qui a soumis le mot.

Le second type est le type `Guess`. Il nous permet de stocker la dernière estimation pour chaque adresse qui a soumis une solution.

```sh
ignite scaffold map guess word submitter count --no-message
```

Ici, nous allons également stocker `count` pour compter combien de devinettes cette adresse a soumis.
