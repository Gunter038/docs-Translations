---
sidebar_label: Module
---

# Créer le module Wordle

Pour le module Wordle, nous pouvons ajouter des dépendances offertes par le Cosmos-SDK.

À partir de la documentation du Cosmos-SDK, un [module](https://docs.ignite.com/guide/nameservice#cosmos-sdk-modules) est défini comme suit :

> Dans une blockchain du Cosmos SDK, la logique spécifique à l'application est implémentée dans des modules séparés. Les modules gardent le code facile à comprendre et à réutiliser. Chaque module contient son propre message et son processeur de transaction, alors que le Cosmos SDK  est responsable du routage de chaque message vers son module respectif.

De multiples modules existent pour le slashing, la validation, l'authentification.

## Scaffolder un Module

Nous utiliserons la dépendance du module `bank` pour les transactions.

À partir de la documentation du Cosmos-SDK, un [`bank`](https://docs.cosmos.network/master/modules/bank/) est défini comme suit :

> Le module bancaire est responsable de la gestion des transferts de "coins" multi-actifs entre comptes et du suivi des pseudo-transferts de cas spéciaux qui doivent fonctionner différemment avec certains types de comptes (notamment déléguant/délèguant pour l'acquisition de comptes). Il expose plusieurs interfaces avec des capacités différentes pour une interaction sécurisée avec d'autres modules qui doivent modifier les soldes des utilisateurs.

Nous construisons le module avec la dépendance `bank` avec la commande suivante :

```sh
ignite scaffold module wordle --dep bank
```

Ceci scaffoldera le module Wordle à notre projet de Chaine World.
