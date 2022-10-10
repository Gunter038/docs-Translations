---
sidebar_label: Vista general de CosmWasm
---

# CosmWasm on Rollmint

CosmWasm es una plataforma de smart contracts construida para el ecosistema de Cosmos haciendo uso de WebAssembly (Wasm) para construir smart contracts para Cosmos-SDK. In this tutorial, we will be exploring how to integrate CosmWasm with Celestia's Data Availability Layer using Rollmint.

> NOTE: This tutorial will explore developing with Rollmint, which is still in Alpha stage. Si encuentras errores, por favor escribe un ticket de Github Issue o háznoslo saber en nuestro Discord. Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

Puedes aprender más sobre CosmWasm [aquí](https://docs.cosmwasm.com/docs/1.0/).

En este tutorial, repasaremos lo siguiente:

* [Configurando tus dependencias para tus smart contracts de CosmWasm](./cosmwasm-dependency.md)
* [Setting up Rollmint on CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Instanciar una red local para tu cadena CosmWasm conectada a Celestia](./cosmwasm-environment.md)
* [Implementando un smart contract de Rust a la cadena CosmWasm](./cosmwasm-contract-deployment.md)
* [Interactuando con el smart contract](./cosmwasm-contract-interaction.md)

El smart contract que usaremos para este tutorial es uno proporcionado por el equipo de CosmWasm para la compra de Nameservice.

Puedes revisar el contrato [aquí](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

Cómo escribir el smart contract de Rust para Nameservice está fuera del alcance de este tutorial. En el futuro añadiremos más tutoriales para escribir smart contracts de CosmWasm para Celestia.
