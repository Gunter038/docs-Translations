---
sidebar_label: Présentation de Wordle
---

# Application Wordle sur Optimint

![mamaki-testnet](/img/wordle.jpg)

Ce tutoriel va aller plus loin dans la construction d'une application du cosmos-sdk pour Optimint, l'implémentation d'Optimistic Rollup de Tendermint, pour le jeu populaire [Wordle](https://www.nytimes.com/games/wordle/index.html).

Ce tutoriel va vous expliquer comment configurer Optimint dans le Ignite CLI et l'utiliser pour construire le jeu. Le tutoriel va passer en revue la simple conception, ainsi que conclure avec de futures implémentations et idées pour étendre le code.

> NOTE : Ce tutoriel explorera le développement avec Optimint, qui est toujours en phase Alpha. Si vous rencontrez des bugs, veuillez écrire une Issue Github ou nous le faire savoir dans notre Discord. De plus, alors qu'Optimint vous permet de construire des rollups souverains sur Celestia, il ne supporte pas encore les preuves de fraude et fonctionne donc en mode "pessimistique" où les nodes auraient besoin de réexécuter les transactions pour vérifier la validité de la chaîne (c.a.d - un  full node). De plus, Optimint ne prend actuellement en charge qu'un seul séquenceur.

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
2. [Installation d'Optimint](./install-optimint.md)
3. [Modules](./wordle-module.md)
4. [Messages](./wordle-messages.md)
5. [Types](./wordle-types.md)
6. [Gardiens](./wordle-keeper.md)
7. [Exécution de Wordle](./run-wordle.md)
