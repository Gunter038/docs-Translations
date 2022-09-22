---
sidebar_label: Обзор CosmWasm
---

# CosmWasm на Optimint

CosmWasm - это платформа смарт контрактов, предназначенная для экосистемы Cosmos, использующая WebAssembly (Wasm) для создания смарт контрактов для Cosmos-SDK. В этом руководстве мы рассмотрим, как интегрировать CosmWasm с Celestia's Data Availability Layer с помощью Optimint.

> ПРИМЕЧАНИЕ: В этом руководстве рассматривается процесс создания с использованием Optimint, который пока находится на Альфа стадии. Если вы столкнулись с багами (ошибками), пожалуйста создайте тикет на Github или сообщите нам в Discord. Furthermore, while Optimint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Optimint currently only supports a single sequencer.

You can learn more about CosmWasm [here](https://docs.cosmwasm.com/docs/1.0/).

In this tutorial, we will going over the following:

* [Setting up your dependencies for your CosmWasm smart contracts](./cosmwasm-dependency.md)
* [Setting up Optimint on CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Instantiate a local network for your CosmWasm chain connected to Celestia](./cosmwasm-environment.md)
* [Deploying a Rust smart contract to CosmWasm chain](./cosmwasm-contract-deployment.md)
* [Interacting with the smart contract](./cosmwasm-contract-interaction.md)

The smart contract we will use for this tutorial is one provided by the CosmWasm team for Nameservice purchasing.

You can check out the contract [here](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

How to write the Rust smart contract for Nameservice is outside the scope of this tutorial. In the future we will add more tutorials for writing CosmWasm smart contracts for Celestia.
