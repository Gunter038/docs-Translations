---
sidebar_label: Aperçu de CosmWasm
---

# CosmWasm sur Optimint

CosmWasm est une plateforme de contrats intelligents construite pour l'écosystème Cosmos en utilisant la WebAssembly (Wasm) pour construire des contrats intelligents sur le Cosmos-SDK. Dans ce tutoriel, nous allons apprendre la manière d'intégrer CosmWasm avec la couche de disponibilité des données de Celestia, en utilisant Optimint.

> NOTE : ce tutoriel explore le développement sur Optimint, qui est toujours en phase alpha. Si vous rencontrez des bugs, nous vous prions d'écrire un ticket sur la section appropriée de Github ou de nous le faire savoir sur notre Discord. Également, alors qu'Optimint permet de construire des rollups souverains sur Celestia, il ne traite actuellement pas les preuves de fraude et fonctionne donc en mode "pessimiste", dans lequel les nodes doivent exécuter à nouveau les transactions pour vérifier la validité de la chaine (c-à-d un full node). Enfin, Optimint ne possède actuellement qu'un unique séquenceur.

Vous pouvez en apprendre davantage sur CosmWasm [ici](https://docs.cosmwasm.com/docs/1.0/).

Dans ce tutoriel, nous allons apprendre à :

* [Configurer vos dépendances pour vos contrats intelligents de CosmWasm](./cosmwasm-dependency.md)
* [Configurer Optimint sur CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Instancier un réseau local pour votre chaine CosmWasm connectée à Celestia](./cosmwasm-environment.md)
* [Déployer un contrat intelligent en Rust sur la chaine CosmWasm](./cosmwasm-contract-deployment.md)
* [Intéragir avec le contrat intelligent](./cosmwasm-contract-interaction.md)

Le contrat intelligent que nous utiliserons dans ce tutoriel est un fourni par l'équipe CosmWasm pour l'achat d'espace de nom.

Vous pouvez trouver le contrat [ici](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

Comment écrire le contrat intelligent en Rust pour le Nameservice ne fait pas l'objet de ce tutoriel. Dans le futur, nous ajouterons davantage de tutoriels pour écrire des contrats intelligents de CosmWasm sur Celestia.
