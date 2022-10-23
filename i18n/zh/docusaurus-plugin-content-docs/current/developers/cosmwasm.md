---
sidebar_label: CosmWasm概述
---

# CosmWasm在Rollmint上

CosmWasm是一个为Comos生态搭建的智能合约平台，通过运用WebAssembly（Wasm）为Cosmos-SDK构建智能合约。 在本教程中，我们会探索如何用Rollmint整和CosmWasm和数据可用层。

> 注意：这个教程会使用仍处于Alpha阶段的Rollmint进行探索开发。 如果你遇到bugs，请用提交一个Github问题单，或者在我们的Discord中告知我们。 此外，Rollmint允许你在Celestia上构建sovereign rollups，它现在还不支持欺诈证明，所以仍旧在节点需要重复执行交易去检查链的有效性的“悲观”模式下运行（即.全节点模式）。 此外，Rollmint目前仅支持单个序列器。

你可以在这里学到更多关于CosmWasm的东西 [这里](https://docs.cosmwasm.com/docs/1.0/)。

在本教程中，我们会仔细学习下面的内容：

* [为你的CosmWasm智能合约设置依赖项](./cosmwasm-dependency.md)
* [在CosmWasm上设置Rollmint](./cosmwasm-dependency.md#wasmd-installation)
* [展示用CosmWasm链将本地网络连接到Celestia](./cosmwasm-environment.md)
* [部署一个Rust的智能合约到CosmWasm链](./cosmwasm-contract-deployment.md)
* [和智能合约交互](./cosmwasm-contract-interaction.md)

我们本次教程使用的智能合约，是由CosmWasm团队为Nameserive购买提供的。

你可以在这里查看合约 [这里](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice)。

如何为域名服务编写Rust的智能合约不在本次教程范围内。 今后，我们会增加更多为Celestia编写 CosmWasm智能合约的教程。
