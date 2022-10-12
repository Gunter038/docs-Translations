---
sidebar_label: Types
---

# Les types dans Wordle

Durant les prochaines étapes, nous allons créer des types qui seront utilisés par les messages que nous avons créés.

## Echafauder les types dans Wordle

```sh
ignite scaffold map wordle word submitter --no-message
```

Ce type est une carte appelée `Wordle` avec deux valeurs appelées `word` et `submitter`. `submitter` est l'adresse de la personne qui a soumis le Wordle.

Le second type est le type du `Guess`. Il nous permet de stocker la dernière proposition pour chaque adresse qui a soumis une proposition.

```sh
ignite scaffold map guess word submitter count --no-message
```

Ici, nous allons également stocker le `count` pour comptabiliser le nombre de propositions que l'adresse a soumis.
