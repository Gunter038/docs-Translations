# Optimint

![optimint](/img/optimint.png)

[Optimint](https://github.com/celestiaorg/optimint) 是ABCI (应用区块链接口) 的一个实现，以便主权独立的 rollups 布署在 Celestia 上。

它是通过插件式替换Cosmos-SDK中的共识层Tendermint来构建的，使得（执行环境）直接与Celestia的数据可用性层通信。

我们启动了一个主权独立的rollup，它把交易打包进区块，并且发布到Celestia，以实现共识和数据可用。

Optimint的目标是让任何人都可以在几分钟内，在Celestia上设计和布署主权独立的rollup。

此外，Optimint允许你在Celestia上构建主权独立的rollup的同时，它目前还不能支持欺诈证明，因此它只能工作在‘悲观’模式下，也就是说节点需要重新执行所有的交易，来验证链的有效性（相当于全节点）。 此外，Optimint当前只支持单一的排序器。

## 教程

以下教程将帮助您开始，通过Optimint构建，连接到Celestia数据可用性层的Cosmos SDK应用程序。 我们称这些链为主权rollup。

您可以开始学习以下教程：

- [文字游戏](./wordle.md)
- [CosmWasm教程](./cosmwasm.md)
