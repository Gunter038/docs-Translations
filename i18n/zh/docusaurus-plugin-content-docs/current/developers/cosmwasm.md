---
sidebar_label: CosmWasm 概述
---

# CosmWasm在Rollmint上

CosmWasm 是一个为 Cosmos 生态系统构建的智能合约平台，利用 WebAssembly（Wasm）为 Cosmos-SDK 构建智能合约。 在本教程中，我们将探索如何集成 CosmWasm与Celestia的数据可用性层使用Rollmint。

> 注意：本教程将探索使用Rollmint进行开发， 仍处于Alpha阶段。 如果您遇到错误，请请求一个 Github 问题或在我们的 Discord 中告诉我们。 此外，Rollmint允许您构建独占汇总 在Celestia上，它目前还不支持欺诈证明 因此，在“悲观”模式下运行，节点需要 重新执行事务以检查链的有效性 （即完整节点） 此外，Rollmint目前仅支持 单个序列器。

您可以在[这里](https://docs.cosmwasm.com/docs/1.0/)了解更多关于 CosmWasm 的信息。

在本教程中，我们将介绍以下内容：

* [为您的 CosmWasm 智能合约设置依赖项](./cosmwasm-dependency.md)
* [设置Rollmint在CosmWasm上](./cosmwasm-dependency.md#wasmd-installation)
* [为连接到Celestia 的 CosmWasm 链实例化本地网络](./cosmwasm-environment.md)
* [将 Rust 智能合约部署到 CosmWasm 链](./cosmwasm-contract-deployment.md)
* [与智能合约交互](./cosmwasm-contract-interaction.md)

我们将用于本教程的智能合约是 CosmWasm 团队为 Nameservice 购买提供的。

你可以在[这里](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice)查看合约。

如何为 Nameservice 编写 Rust 智能合约超出了本教程的范围。 未来，我们将添加更多为 Celestia 编写 CosmWasm 智能合约的教程。
