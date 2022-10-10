---
sidebar_label: Présentation de Wordle
---

# Application Wordle sur Optimint

![mamaki-testnet](/img/wordle.jpg)

This tutorial guide will go over building a cosmos-sdk app for Rollmint, the Sovereign-Rollup implementation of Tendermint, for the popular game [Wordle](https://www.nytimes.com/games/wordle/index.html).

This tutorial will go over how to setup Rollmint in the Ignite CLI and use it to build the game. Le tutoriel va passer en revue la simple conception, ainsi que conclure avec de futures implémentations et idées pour étendre le code.

> NOTE: This tutorial will explore developing with Rollmint, which is still in Alpha stage. Si vous rencontrez des bugs, veuillez écrire une Issue Github ou nous le faire savoir dans notre Discord. Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

## Pré-requis

Étant donné que ce tutoriel est destiné aux développeurs expérimentés vis-àvis du Cosmos-SDK, nous vous recommandons de passer en revue les tutoriels suivants dans Ignite pour comprendre tous les différents composants du Cosmos-SDK avant de continuer avec ce tutoriel.

* [Hello, World](https://docs.ignite.com/guide/hello)
* [Blog et modules de base](https://docs.ignite.com/guide/blog)
* [Tutoriel de Nameservice](https://docs.ignite.com/guide/nameservice)
* [Scavenger Hunt](https://docs.ignite.com/guide/scavenge)

Vous n'avez pas à faire ces guides pour suivre ce tutoriel Wordle, mais cela vous aide à mieux comprendre l'architecture du Cosmos-SDK.

## Implémentation de la conception

Les règles de Wordle sont simples : il faut deviner le mot du jour.

Points clés à considérer :

* Le mot est un mot à cinq lettres.
* Vous avez 6 suppositions.
* Toutes les 24 heures, il y a un nouveau mot.

L'interface pour Wordle vous montre quelques indicateurs : un vert surligné sur une lettre dans une certaine position signifie que c'est la lettre correcte dans la bonne position. Une surbrillance jaune signifie que c'est une lettre correcte pour le mot dans la mauvaise position. Un surlignage gris signifie que la lettre ne fait pas partie du mot.

Pour la simplicité de la conception, nous éviterons ces astuces, bien qu'il y ait des moyens d'étendre cette base de code pour implémenter cela, que nous montrerons à la fin.

Dans cette conception actuelle, nous implémentons les règles suivantes :

* 1 mot peut être soumis par jour
* Chaque adresse aura 6 tentatives pour deviner le mot
* Il doit s'agir d'un mot à cinq lettres.
* Quiconque devine correctement le mot avant leurs 6 essais est supérieur a une récompense de 100 jetons WORDLE.

Nous irons plus loin dans l'architecture pour atteindre cet objectif dans le guide. Mais pour l'instant, nous allons commencer à mettre en place notre environnement de développement.

## Table des matières pour ce tutoriel

Le tutoriel suivant est divisé en sections suivantes :

1. [Ignite et Scaffolding de la Chaine](./scaffold-wordle.md)
2. [Installation de Rollmint](./install-rollmint.md)
3. [Modules](./wordle-module.md)
4. [Messages](./wordle-messages.md)
5. [Types](./wordle-types.md)
6. [Keepers](./wordle-keeper.md)
7. [Exécution de Wordle](./run-wordle.md)
