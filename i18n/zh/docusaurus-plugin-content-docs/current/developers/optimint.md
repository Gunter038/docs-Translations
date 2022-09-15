# 优化

![优化](/img/optimint.png)

[Optimint](https://github.com/celestiaorg/optimint)是一个乐观的汇总 ABCI（应用程序区块链接口）在 为了使用Celestia的Cosmos SDK构建主权链。

它是通过替换宇宙SDK Tendermint构建的共识层，替换为直接与Celestia的数据可用性层通信。

它启动一个乐观汇总，将事务收集到块中，将其发布到Celestia，以获得共识和数据可用性。

Optimint的目标是让任何人都能够设计和部署cosmos区域，在几分钟内就可以在Celestia。

## 教程

以下教程将帮助您开始构建 连接Celestia数据可用性的Cosmos SDK应用程序 层通过Optimint。 我们称这些链为主权汇总。

您可以开始学习以下教程：

- [文字游戏](./wordle.md)
- [CosmWasm教程](./cosmwasm.md)
