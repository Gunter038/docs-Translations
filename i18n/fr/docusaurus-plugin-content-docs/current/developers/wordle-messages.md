---
sidebar_label: Messages
---

# Messages

Les messages nous permettent de traiter et soumettre de l'information à notre module spécifique.

Selon la définition fournie par le Cosmos-SDK, les [messages](https://docs.cosmos.network/master/building-modules/messages-and-queries.html#messages) sont :

> Dans le Cosmos-SDK, les messages sont des objects qui sont contenus dans des transactions pour déclencher des transitions d'état. Chaque module du Cosmos-SDK définit une liste de messages et comment les traiter.

Pour des messages pour Wordle, et étant donné notre concept initial, nous ferons 2 messages avec ignite.

* Le premier est : `SubmitWordle` et il ne génère que le Wordle du jour.
* Le second est : `SubmitGuess` et il tente de deviner le wordle soumis. It also passes a word as a guess.

Avec ces concepts initiaux, nous pouvons commencer à créer nos messages !

## Scaffolding A Message

To create the `SubmitWordle` message, we run the following command:

```sh
ignite scaffold message submit-wordle word
```

This creates the `submit-wordle` message that takes in `word` as a parameter.

We now create the final message, `SubmitGuess`:

```sh
ignite scaffold message submit-guess word
```

Here, we are passing a word as a guess with `submit-guess`.
