# Rollmint

![rollmint](/img/rollmint.png)

[Rollmint](https://github.com/celestiaorg/rollmint) is an ABCI (Application Blockchain Interface) implementation for sovereign rollups to deploy on top of Celestia.

它是通过插件式替换Cosmos-SDK中的共识层Tendermint来构建的，使得（执行环境）直接与Celestia的数据可用性层通信。

我们启动了一个主权独立的rollup，它把交易打包进区块，并且发布到Celestia，以实现共识和数据可用。

The goal of Rollmint is to enable anyone to design and deploy a sovereign rollup on Celestia in minutes.

Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

## 教程

The following tutorials will help you get started building Cosmos-SDK applications that connect to Celestia's Data Availability Layer via Rollmint. 我们称这些链为主权rollup。

您可以开始学习以下教程：

- [文字游戏](./wordle.md)
- [CosmWasm教程](./cosmwasm.md)
