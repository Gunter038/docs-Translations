# Rollmint

![rollmint](/img/rollmint.png)

[Rollmint](https://github.com/celestiaorg/rollmint)是ABCI 主权 (应用程序区块链接) 实施 要部署在Celestia顶部的汇总

它是通过插件式替换Cosmos-SDK中的共识层Tendermint来构建的，使得（执行环境）直接与Celestia的数据可用性层通信。

我们启动了一个主权独立的rollup，它把交易打包进区块，并且发布到Celestia，以实现共识和数据可用。

Rollmint的目标是让任何人都能够设计和 在几分钟内在Celestia上部署主权汇总。

此外，Rollmint允许您构建独占汇总 在Celestia上，它目前还不支持欺诈证明 因此，在“悲观”模式下运行，节点需要 重新执行事务以检查链的有效性 (即完整节点) 。 此外，Rollmint目前仅支持 单个序列器。

## 教程

以下教程将帮助您开始构建 通过Rollmint分层连接Celestia数据可用性的Cosmos SDK应用程序 我们称这些链为主权rollup。

您可以开始学习以下教程：

- [文字游戏](./wordle.md)
- [CosmWasm教程](./cosmwasm.md)
