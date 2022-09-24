---
sidebar_label: Module
---

# Créer le module Wordle

For the Wordle module, we can add dependencies offered by Cosmos-SDK.

From the Cosmos-SDK docs, a [module](https://docs.ignite.com/guide/nameservice#cosmos-sdk-modules) is defined as the following:

> In a Cosmos SDK blockchain, application-specific logic is implemented in separate modules. Modules keep code easy to understand and reuse. Each module contains its own message and transaction processor, while the Cosmos SDK is responsible for routing each message to its respective module.

De multiples modules existent pour le slashing, la validation, l'authentification.

## Scaffolding A Module

Nous utiliserons la dépendance du module `bank` pour les transactions.

From the Cosmos-SDK docs, the [`bank`](https://docs.cosmos.network/master/modules/bank/) module is defined as the following:

> The bank module is responsible for handling multi-asset coin transfers between accounts and tracking special-case pseudo-transfers which must work differently with particular kinds of accounts (notably delegating/undelegating for vesting accounts). It exposes several interfaces with varying capabilities for secure interaction with other modules which must alter user balances.

We build the module with the `bank` dependency with the following command:

```sh
ignite scaffold module wordle --dep bank
```

This will scaffold the Wordle module to our Wordle Chain project.
