- - -
sidebar_label : Celestia 的数据可用性层
- - -

# Celestia 的数据可用性层

Celestia 是一个数据可用性 (DA) 层，它为[数据可用性问题](https://coinmarketcap.com/alexandria/article/what-is-data-availability)提供了一个可扩展的解决方案。 由于区块链网络的无许可性质，DA 层必须为执行层和结算层提供一种机制，以最小化信任的方式检查交易数据是否确实可用。

Celestia DA 层的两个关键特性是[数据可用性采样](https://blog.celestia.org/celestia-mvp-release-data-availability-sampling-light-clients/) (DAS) 和[命名空间默克尔树](https://github.com/celestiaorg/nmt)(NMT)。 这两个功能都是新颖的区块链扩展解决方案：DAS 使轻节点无需下载整个区块即可验证数据可用性；NMT 使 Celestia 上的执行和结算层能够下载仅与它们相关的交易。

## 数据可用性采样(DAS)

一般来说，轻节点只下载包含区块数据（即交易列表）的承诺（即Merkle根）的区块头。

为了使 DAS 成为可能，Celestia 使用二维 Reed-Solomon 编码方案对块数据进行编码：每个块数据被分成 k × k 块，排列在 k × k 矩阵中，并通过多次应用 Reed-Solomon 编码来增加校验数据，使它扩展为 2k × 2k。

然后，为扩展矩阵的行和列计算 4k 个单独的 Merkle 根；再通过这些 Merkle 根生成一个（总的）Merkle 根，用作区块头中的区块数据承诺。

![2D Reed-Soloman (RS) 编码](/img/concepts/reed-solomon-encoding.png)

为了验证数据是否可用，Celestia 轻节点正是对 2k × 2k 数据块进行采样。

每个轻节点在扩展后的矩阵中随机选择一组唯一坐标，并在这些坐标处，向存储全节点查询数据块和相应的 Merkle 证明。 如果轻节点对每个采样的查询都接收到有效响应，则[很有可能保证](https://github.com/celestiaorg/celestia-node/issues/805#issuecomment-1150081075)整个块的数据是可用的。

此外，每个接收到的具有正确 Merkle 证明的数据块都会被发送到网络。 结果，只要 Celestia 轻节点对足够多的数据块进行采样（即，至少 k × k 个不重复数据块），完整的区块可以被诚实的全节点恢复。

有关 DAS 的更多详细信息，请查看[原始论文](https://arxiv.org/abs/1809.09044)。

### 可扩展性

DAS 使 Celestia 能够扩展 DA 层。 DAS 可以由资源有限的轻节点执行，因为每个轻节点仅对块数据的一小部分进行采样。 网络中的轻节点越多，它们可以共同下载和存储的数据就越多。

这意味着增加执行 DAS 的轻节点的数量允许更大的块（即，具有更多事务），同时仍然保持 DAS 对资源有限的轻节点的可行性。 然而，为了验证区块头，Celestia 轻节点需要下载 4k 中间 Merkle 根。

对于 n 字节的块数据大小，这意味着每个轻节点必须下载 O(n) 字节。 因此，Celestia 轻节点带宽容量的任何改进都会对 Celestia DA 层的吞吐量产生二次影响。

### 错误扩展数据的欺诈证明

下载 4k 中间 Merkle 根的要求是 使用二维 Reed-Solomon 编码方案的结果。 或者，DAS 可以使用标准（即一维）Reed-Solomon 编码来设计，其中原始数据被分成 k 个块并用 k 个附加扩展奇偶校验数据块。 由于块数据提交是 2k 个结果数据块的 Merkle 根，轻节点不再需要下载 O(n) 字节来验证块头。

标准 Reed-Solomon 编码的缺点是处理不正确地生成扩展数据的恶意块生产者。

这是可能的，因为__Celestia 不需要大多数共识（即区块生产者）诚实以保证数据可用性__。 因此，如果扩展数据无效，则原始数据可能无法恢复，即使轻节点正在采样足够多的唯一块（即，对于标准编码，至少为 k，对于二维编码，至少为 k × k）。

作为一种解决方案，_错误生成扩展数据的欺诈证明_使轻节点能够拒绝具有无效扩展数据的块。 这样的证明需要重建编码并验证不匹配。 使用标准 Reed-Solomon 编码，这需要下载原始数据，即 O(n) 字节。 相比之下，使用二维 Reed-Solomon 编码，只需要 O(n) 个字节，因为只需验证扩展矩阵的一行或一列就足够了。

## 命名空间 Merkle 树(NMTs)

Celestia 将块数据划分为多个命名空间，每个使用 DA 层的应用程序（例如，rollup）一个命名空间。 因此，每个应用程序只需要下载自己的数据，而可以忽略其他应用程序的数据。

为此，DA 层必须能够证明提供的数据是完整的，即返回一定命名空间的所有数据。 为此，Celestia 正在使用命名空间默克尔树 (NMTs)。

NMT 是一棵 Merkle 树，其叶子按命名空间标识符排序，并修改了哈希函数，以便树中的每个节点都包含其所有后代的命名空间范围。 下图显示了一个高度3（即8个数据块）的 NMT 示例， 数据被划分为三个命名空间。

![Namespaced Merkle 树](/img/concepts/nmt.png)

当应用程序请求命名空间2的数据时，DA层必须 提供数据块 `D3`， `D4`, `D5`, 和 `D6` 和节点 `N2`, `N8` 和 `N7` 作为证据(注意应用程序已经有根 `N14` 来自 个块头)。

因此，应用程序能够检查所提供的数据是否为区块数据的部分。 此外，应用程序可以验证已提供命名空间 2 的所有数据 都已提供。 如果DA层只提供例如数据块`D4`和`D5`，它还必须提供节点`N11`和`N12`作为证明。 然而，应用程序可以通过检查两个节点的名称空间范围来识别数据的不完整性，即`N12`和`N11`都有属于名称空间2的后代。

关于NMT的更多细节，请看[原始论文](https://arxiv.org/abs/1905.09274)。

## 为数据可用性开发 PoS（权益证明）区块链

### 提供数据可用性

Celestia DA 层由 PoS 区块链组成。 Celestia 将此区块链称为 [Celestia App](https://github.com/celestiaorg/celestia-app)，它是一个由 [Cosmos SDK](https://docs.cosmos.network/v0.44/) 构建的提供交易以促进 DA 层的应用程序。 下面介绍了 Celestia App 的主要组成部分。

![Celestia App 的主要组成部分](/img/concepts/celestia-app.png)

Celestia App 是建立在 [Celestia Core](https://github.com/celestiaorg/celestia-core) 之上的 [Tendermint 共识算法](https://arxiv.org/abs/1807.04938) 的改进版。 Vanilla Tendermint 和 Celestia Core 的重要更新的部分内容如下：

- 启用区块数据的纠删码技术（使用二维Reed-Solomon 算法）。
- 通过Tendermint将常规Merkle树替换为用于存储区块数据的 [Namespaced Merkle](https://github.com/celestiaorg/nmt) 树，使得上述各层（即执行和结算）只下载需要的数据（更多细节，请见下面案例描述的部分）。

关于 Tendermint 的更多更新细节，请看 [ADRs](https://github.com/celestiaorg/celestia-core/tree/v0.34.x-celestia/docs/celestia-architecture)。 请注意，Celestia Core 节点仍在使用 Tendermint p2p 网络。

与Tendermint类似，Celestia Core通过 [ABCI++](https://github.com/tendermint/tendermint/tree/master/spec/abci%2B%2B) 连接到应用层（即状态机），也是 [ABCI](https://github.com/tendermint/tendermint/tree/master/spec/abci)（应用区块链接口）的主要发展之一。

Celestia App 状态机对于执行 PoS 逻辑和实现 DA 层的治理是必要的。

然而，Celestia App 是数据不可知的 -- 状态机既不验证也不存储 Celestia App 所提供的数据。
