---
sidebar_label: Aperçu de CosmWasm
---

# CosmWasm sur Optimint

CosmWasm est une plateforme de contrats intelligents construite pour l'écosystème Cosmos en utilisant la WebAssembly (Wasm) pour construire des contrats intelligents sur le Cosmos-SDK. Dans ce tutoriel, nous allons apprendre la manière d'intégrer le CosmWasm avec la couche de disponibilité des données de Celestia, en utilisant Optimint.

> NOTE : ce tutoriel explore le développement sur Optimint, qui est toujours en phase alpha. Si vous rencontrez des bugs, nous vous prions d'écrire un ticket sur la section appropriée de Github ou de nous le faire savoir sur notre Discord. Également, alors qu'Optimint permet de construire des rollups souverains sur Celestia, il ne traite actuellement pas les preuves de fraude et fonctionne donc en mode "pessimiste", dans lequel les nœuds doivent exécuter à nouveau les transactions pour vérifier la validité de la chaine (c-à-d un full node). Enfin, Optimint ne possède actuellement qu'un unique séquenceur.

Vous pouvez en apprendre davantage sur CosmWasm [ici](https://docs.cosmwasm.com/docs/1.0/).

Dans ce tutoriel, nous allons apprendre à :

* [Configurer vos dépendances pour vos contrats intelligents de CosmWasm](./cosmwasm-dependency.md)
* [Configurer Optimint sur CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Instancier un réseau local pour votre chaine CosmWasm connectée à Celestia](./cosmwasm-environment.md)
* [Déployer un contrat intelligent en Rust sur la chaine CosmWasm](./cosmwasm-contract-deployment.md)
* [Interacting with the smart contract](./cosmwasm-contract-interaction.md)

The smart contract we will use for this tutorial is one provided by the CosmWasm team for Nameservice purchasing.

You can check out the contract [here](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

How to write the Rust smart contract for Nameservice is outside the scope of this tutorial. In the future we will add more tutorials for writing CosmWasm smart contracts for Celestia.
