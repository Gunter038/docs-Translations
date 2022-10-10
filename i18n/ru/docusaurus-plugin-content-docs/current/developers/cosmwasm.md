---
sidebar_label: Обзор CosmWasm
---

# CosmWasm on Rollmint

CosmWasm - это платформа смарт контрактов, предназначенная для экосистемы Cosmos, использующая WebAssembly (Wasm) для создания смарт контрактов для Cosmos-SDK. In this tutorial, we will be exploring how to integrate CosmWasm with Celestia's Data Availability Layer using Rollmint.

> NOTE: This tutorial will explore developing with Rollmint, which is still in Alpha stage. Если вы столкнулись с багами (ошибками), пожалуйста создайте тикет на Github или сообщите нам в Discord. Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

Вы можете узнать больше о CosmWasm [здесь](https://docs.cosmwasm.com/docs/1.0/).

В этом руководстве мы рассмотрим следующее:

* [Настройка необходимых зависимостей для смарт-контрактов CosmWasm](./cosmwasm-dependency.md)
* [Setting up Rollmint on CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Создайте локальную сеть для вашей сети CosmWasm, подключенной к Celestia](./cosmwasm-environment.md)
* [Создание смарт контракта на основе Rust в сети CosmWasm](./cosmwasm-contract-deployment.md)
* [Взаимодействие со смарт контрактом](./cosmwasm-contract-interaction.md)

Смарт контракт, который мы будем использовать в этом руководстве, предоставлен командой CosmWasm для покупки Nameservice.

Вы можете ознакомиться с контрактом [тут](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

Как написать смарт контракт на основе Rust для Nameservice выходит за рамки данного руководства. В будущем мы добавим больше руководств по написанию CosmWasm смарт контрактов для Celestia.
