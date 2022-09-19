# 优化

![优化](/img/optimint.png)

[Optimint](https://github.com/celestiaorg/optimint) is an ABCI (Application Blockchain Interface) implementation for sovereign rollups to deploy on top of Celestia.

它是通过替换宇宙SDK Tendermint构建的共识层，替换为直接与Celestia的数据可用性层通信。

It spins up a sovereign rollup, which collects transactions into blocks and posts them onto Celestia for consensus and data availability.

The goal of Optimint is to enable anyone to design and deploy a sovereign rollup on Celestia in minutes.

Furthermore, while Optimint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Optimint currently only supports a single sequencer.

## 教程

以下教程将帮助您开始构建 连接Celestia数据可用性的Cosmos SDK应用程序 层通过Optimint。 我们称这些链为主权汇总。

您可以开始学习以下教程：

- [文字游戏](./wordle.md)
- [CosmWasm教程](./cosmwasm.md)
