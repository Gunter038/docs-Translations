---
sidebar_label: 集成Celestia
---

# 集成Celestia

> 此文档适用于如托管机构和浏览器的第三方服务提供商整合Celestia网络。

## Celestia服务提供者标记

Celestia是一个相当标准的以Cosmos-SDK为基础的区块链。 我们使用稍作修改的最新版本Tendermint 和 Cosmos-SDK。 这意味着我们是：

- 使用默认的 Cosmos-SDK 模块：授权、银行、发行、质押、 滑点、铸造、crisis、ibc主机、genutil、证据 ibc转账, params, 治理 (在某些待定容量里有限制), 升级, 授予, 收费, 能力, 支付
- 使用 Cosmos-SDK 和 Tendermint 提供的标准数字密钥方案，secp256k1 用于用户交易，tm-ed25519 用于签署和验证共识消息。

虽然所使用的模块会被更改，但Celestia的目标是尽可能减少此种情况。

### 托管和密钥管理

Celestia支持许多已经存在的密钥管理系统，因为我们依靠Cosmos-SDK 和 Tendermint 数据库来签署和核实交易。 [COSMOS-SDK 文档](https://docs.cosmos.network/master/basics/accounts.html#keys-accounts-addresses-and-signatures)

### RPC 和查询

在 celestia-app，只暴露了 Tendermint 和Cosmos-SDK 的标准RPC端点。 我们目前没有添加或减少任何核心函数，但这可能会在未来改动。 查询区块链中的数据也是如此。

在 celestia-node（数据可用性节点客户端）中，有一个 JSON-RPC API，允许您直接与 Celestia 的数据可用性层交互。 指南在[这里](https://docs. celestia. org/developers/node-tutorial)

### 兼容性

Linux，尤其是Ubuntu 20.04 LTS，是最佳的测试。 可能与其他操作系统兼容，但它们目前尚未测试。 一些用于擦除数据的加密数据库不能保证在其他平台上工作。

### 同步

由于我们使用 Tendermint 和 Cosmos-SDK，可以使用这些数据库支持的任何方法区块链同步区块链。 这包括fast同步、状态同步和快速同步。

### 相对于其他区块链的显著不同

相对于其他基于 Tendermint 的区块链，Celestia将有明显较长的区块时间，大约30* 秒。 这个区块时间的逻辑是优化采样区块链中轻客户端使用的带宽。 并且并不是因为我们以任何有意义的方式修改了Tendermint的共识。 验证者可能会下载/上传相对较大的区块。 应该注意到虽然这些区块很大，但实际上很少有典型的区块链状态执行在 Celestia 上进行。 这意味着带宽要求可能会大于典型的基于 Cosmos-SDK 的区块链全节点的带宽需求，计算需求的量级应该相似。

*可能会变动
