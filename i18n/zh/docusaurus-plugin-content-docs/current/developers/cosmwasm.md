---
sidebar_label: CosmWasm 概述
---

# Optimint 上的 CosmWasm

CosmWasm 是一个为 Cosmos 生态系统构建的智能合约平台，利用 WebAssembly（Wasm）为 Cosmos-SDK 构建智能合约。 在本教程中，我们将探索如何使用 Optimint 将 CosmWasm 与 Celestia 的数据可用性层集成。

> 注意：本教程将探索使用仍处于 Alpha 阶段的 Optimint 进行开发。 如果您遇到错误，请请求一个 Github 问题或在我们的 Discord 中告诉我们。 此外，虽然 Optimint 允许您在 Celestia 上构建主权rollups ，但它目前还不支持欺诈证明，因此运行在“悲观”模式下，节点需要重新执行交易以检查链的有效性（即一个全节点）。 此外，Optimint 目前仅支持单个定序器。

您可以在[这里](https://docs.cosmwasm.com/docs/1.0/)了解更多关于 CosmWasm 的信息。

在本教程中，我们将介绍以下内容：

* [为您的 CosmWasm 智能合约设置依赖项](./cosmwasm-dependency.md)
* [在 CosmWasm 上设置 Optimint](./cosmwasm-dependency.md#wasmd-installation)
* [为连接到Celestia 的 CosmWasm 链实例化本地网络](./cosmwasm-environment.md)
* [将 Rust 智能合约部署到 CosmWasm 链](./cosmwasm-contract-deployment.md)
* [与智能合约交互](./cosmwasm-contract-interaction.md)

我们将用于本教程的智能合约是 CosmWasm 团队为 Nameservice 购买提供的。

你可以在[这里](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice)查看合约。

如何为 Nameservice 编写 Rust 智能合约超出了本教程的范围。 未来，我们将添加更多为 Celestia 编写 CosmWasm 智能合约的教程。
