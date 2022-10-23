# Rollmint

![rollmint](/img/rollmint.png)

[Rollmint](https://github.com/celestiaorg/rollmint) 是一个为sovereign rollups部署在Celestia顶层的ABCI (Application Blockchain Interface) 应用。

它是通过一个可直接和Celestia数据可用层通信，随时可用的代理替换Tendermint，Cosmos-SDK共识层而建立的。

它启动一个将交易收集到区块中的sovereign rollup，然后发布在Celestia，以获得共识和数据可用性。

Rollmint 的目标是使任何人都可以在几分钟内在Celestia上设计和部署一个sovereign rollup。

此外，Rollmint允许你在Celestia上建立sovereign rollups，它目前不支持欺诈验证，所以是运行在一个节点需要重复执行交易去验证链的有效性(即.一个全节点)的“悲观”模式下。 此外，Rollmint目前只支持单个序列器。

## 教程

接下来的教程会帮你通过Rollmint构建一个连接到Celestia数据可用层的Cosmos-SDK应用。 我们将这些链称为Sovereign Rollups。

你可以按以下教程开始：

- [gm world](./gm-world.md)
- [Wordle Game](./wordle.md)
- [CosmWasm教程](./cosmwasm.md)
