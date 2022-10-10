---
sidebar_label: Aperçu de CosmWasm
---

# CosmWasm on Rollmint

CosmWasm est une plateforme de contrats intelligents construite pour l'écosystème Cosmos en utilisant la WebAssembly (Wasm) pour construire des contrats intelligents sur le Cosmos-SDK. In this tutorial, we will be exploring how to integrate CosmWasm with Celestia's Data Availability Layer using Rollmint.

> NOTE: This tutorial will explore developing with Rollmint, which is still in Alpha stage. Si vous rencontrez des bugs, nous vous prions d'écrire un ticket sur la section appropriée de Github ou de nous le faire savoir sur notre Discord. Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

Vous pouvez en apprendre davantage sur CosmWasm [ici](https://docs.cosmwasm.com/docs/1.0/).

Dans ce tutoriel, nous allons apprendre à :

* [Configurer vos dépendances pour vos contrats intelligents de CosmWasm](./cosmwasm-dependency.md)
* [Setting up Rollmint on CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Instancier un réseau local pour votre chaine CosmWasm connectée à Celestia](./cosmwasm-environment.md)
* [Déployer un contrat intelligent en Rust sur la chaine CosmWasm](./cosmwasm-contract-deployment.md)
* [Intéragir avec le contrat intelligent](./cosmwasm-contract-interaction.md)

Le contrat intelligent que nous utiliserons dans ce tutoriel est un fourni par l'équipe CosmWasm pour l'achat d'espace de nom.

Vous pouvez trouver le contrat [ici](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

Comment écrire le contrat intelligent en Rust pour le Nameservice ne fait pas l'objet de ce tutoriel. Dans le futur, nous ajouterons davantage de tutoriels pour écrire des contrats intelligents de CosmWasm sur Celestia.
