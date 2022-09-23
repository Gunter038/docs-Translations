---
sidebar_label: Types de Wordle
---

# Types de Wordle

Dans les prochaines étapes, nous allons créer des types qui seront utilisés par les messages que nous avons créés.

## Scaffoling Wordle Types

```sh
ignite scaffold map wordle word submitter --no-message
```

Ce type est une carte appelée `Wordle` avec deux valeurs appelées `mot` et `déposant`. `submitter` is the address of the person that submitted the Wordle.

The second type is the `Guess` type. It allows us to store the latest guess for each address that submitted a solution.

```sh
ignite scaffold map guess word submitter count --no-message
```

Here, we are also storing `count` to count how many guesses this address submitted.
