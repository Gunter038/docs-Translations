---
sidebar_label: CosmWasm 概述
---

# CosmWasm on Rollmint

CosmWasm 是一个为 Cosmos 生态系统构建的智能合约平台，利用 WebAssembly（Wasm）为 Cosmos-SDK 构建智能合约。 In this tutorial, we will be exploring how to integrate CosmWasm with Celestia's Data Availability Layer using Rollmint.

> NOTE: This tutorial will explore developing with Rollmint, which is still in Alpha stage. 如果您遇到错误，请请求一个 Github 问题或在我们的 Discord 中告诉我们。 Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

您可以在[这里](https://docs.cosmwasm.com/docs/1.0/)了解更多关于 CosmWasm 的信息。

在本教程中，我们将介绍以下内容：

* [为您的 CosmWasm 智能合约设置依赖项](./cosmwasm-dependency.md)
* [Setting up Rollmint on CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [为连接到Celestia 的 CosmWasm 链实例化本地网络](./cosmwasm-environment.md)
* [将 Rust 智能合约部署到 CosmWasm 链](./cosmwasm-contract-deployment.md)
* [与智能合约交互](./cosmwasm-contract-interaction.md)

我们将用于本教程的智能合约是 CosmWasm 团队为 Nameservice 购买提供的。

你可以在[这里](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice)查看合约。

如何为 Nameservice 编写 Rust 智能合约超出了本教程的范围。 未来，我们将添加更多为 Celestia 编写 CosmWasm 智能合约的教程。
