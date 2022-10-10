---
sidebar_label: Огляд CosmWasm
---

# CosmWasm on Rollmint

CosmWasm — це смартконтрактна платформа, створена для екосистеми Cosmos шляхом використання WebAssembly (Wasm) для створення смартконтрактів для Cosmos-SDK. In this tutorial, we will be exploring how to integrate CosmWasm with Celestia's Data Availability Layer using Rollmint.

> NOTE: This tutorial will explore developing with Rollmint, which is still in Alpha stage. Якщо ви зіткнетеся з помилками, будь ласка, напишіть заявку на Github Issue або повідомте нам про це в нашому Discord. Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

Про CosmWasm можна дізнатися більше [тут](https://docs.cosmwasm.com/docs/1.0/).

У цьому посібнику ми переходимо до наступних кроків:

* [Встановлення залежностей для ваших смартконтрактів CosmWasm](./cosmwasm-dependency.md)
* [Setting up Rollmint on CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Створіть екземпляр локальної мережі для свого ланцюга CosmWasm, підключеного до Celestia](./cosmwasm-environment.md)
* [Розгортання смартконтракту Rust у мережі CosmWasm](./cosmwasm-contract-deployment.md)
* [Взаємодія зі смартконтрактом](./cosmwasm-contract-interaction.md)

Смартконтракт, який ми будемо використовувати для цього підручника, надано командою CosmWasm для придбання Nameservice.

Ви можете перевірити контракт [тут](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

Як написати смартконтракт Rust для Nameservice, виходить за рамки цього посібника. У майбутньому ми додамо більше навчальних посібників для написання смартконтрактів CosmWasm для Celestia.
