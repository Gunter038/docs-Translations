- - -
sidebar_label : Celestia 的数据可用性层
- - -

# Celestia 的数据可用性层

Celestia 是一个数据可用性 (DA) 层，它为[数据可用性问题](https://coinmarketcap.com/alexandria/article/what-is-data-availability)提供了一个可扩展的解决方案。 由于区块链网络的无许可性质，DA 层必须为执行层和结算层提供一种机制，以最小化信任的方式检查交易数据是否确实可用。

Celestia DA 层的两个关键特性是[数据可用性采样](https://blog.celestia.org/celestia-mvp-release-data-availability-sampling-light-clients/) (DAS) 和[命名空间默克尔树](https://github.com/celestiaorg/nmt)(NMT)。 这两个功能都是新颖的区块链扩展解决方案：DAS 使轻节点无需下载整个区块即可验证数据可用性；NMT 使 Celestia 上的执行和结算层能够下载仅与它们相关的交易。

## 数据可用性采样(DAS)

一般来说，轻节点只下载包含区块数据（即交易列表）的承诺（即Merkle根）。

为了使 DAS 成为可能，Celestia 使用二维 Reed-Solomon 编码方案对块数据进行编码：每个块数据被分成 k × k 块，排列在 ak × k 矩阵中，并用奇偶校验数据扩展为 2k × 2k通过多次应用 Reed-Solomon 编码来扩展矩阵。

然后，为扩展矩阵的行和列计算 4k 个单独的 Merkle 根；这些 Merkle 根的 Merkle 根被用作区块头中的区块数据承诺。

![2D Reed-Soloman (RS) Encoding](/img/concepts/reed-solomon-encoding.png)

为了验证数据是否可用，Celestia 轻节点正在对 2k × 2k 数据块进行采样。

每个轻节点在扩展矩阵中随机选择一组唯一坐标，并在这些坐标处查询数据块和相应的 Merkle 证明的完整节点。 如果轻节点对每个采样查询都接收到有效响应，则很有[可能保证](https://github.com/celestiaorg/celestia-node/issues/805#issuecomment-1150081075)整个块的数据是可用的。

此外，每个接收到的具有正确 Merkle 证明的数据块都会被发送到网络。 结果，只要 Celestia 轻节点对足够多的数据块进行采样（即，至少 k × k 个唯一块），完整的块可以通过诚实的全节点来恢复。

有关 DAS 的更多详细信息，请查看[原始论文](https://arxiv.org/abs/1809.09044)。

### 可扩展性

DAS 使 Celestia 能够扩展 DA 层。 DAS 可以由资源有限的轻节点执行，因为每个轻节点仅对块数据的一小部分进行采样。 网络中的轻节点越多，它们可以共同下载和存储的数据就越多。

这意味着增加执行 DAS 的轻节点的数量允许更大的块（即，具有更多事务），同时仍然保持 DAS 对资源有限的轻节点的可行性。 然而，为了验证区块头，Celestia 轻节点需要下载 4k 中间 Merkle 根。

对于 n 字节的块数据大小，这意味着每个轻节点必须下载 O(n) 字节。 因此，Celestia 轻节点带宽容量的任何改进都会对 Celestia DA 层的吞吐量产生二次影响。

### 错误扩展数据的欺诈证明

下载 4k 中间 Merkle 根的要求是 使用二维 Reed-Solomon 编码方案的结果。 或者，DAS 可以使用标准（即一维）Reed-Solomon 编码来设计，其中原始数据被分成 k 个块并用 k 个附加扩展奇偶校验数据块。 由于块数据提交是 2k 个结果数据块的 Merkle 根，轻节点不再需要下载 O(n) 字节来验证块头。

标准 Reed-Solomon 编码的缺点是处理不正确地生成扩展数据的恶意块生产者。

This is possible as __Celestia does not require a majority of the consensus (i.e., block producers) to be honest to guarantee data availability.__ Thus, if the extended data is invalid, the original data might not be recoverable, even if the light nodes are sampling sufficient unique chunks (i.e., at least k for a standard encoding and k × k for a 2-dimensional encoding).

As a solution, _Fraud Proofs of Incorrectly Generated Extended Data_ enable light nodes to reject blocks with invalid extended data. Such proofs require reconstructing the encoding and verifying the mismatch. With standard Reed-Solomon encoding, this entails downloading the original data, i.e., O(n) bytes. Contrastingly, with 2-dimensional Reed-Solomon encoding, only O(n ) bytes are required as it is sufficient to verify only one row or one column of the extended matrix.

## Namespaced Merkle Trees (NMTs)

Celestia partitions the block data into multiple namespaces, one for every application (e.g., rollup) using the DA layer. As a result, every application needs to download only its own data and can ignore the data of other applications.

For this to work, the DA layer must be able to prove that the provided data is complete, i.e., all the data for a given namespace is returned. To this end, Celestia is using Namespaced Merkle Trees (NMTs).

An NMT is a Merkle tree with the leafs ordered by the namespace identifiers and the hash function modified so that every node  in the tree includes the range of namespaces of all its descendants. The following figure shows an example of an NMT with height three (i.e., eight data chunks). The data is partitioned into three namespaces.

![Namespaced Merkle Tree](/img/concepts/nmt.png)

When an application requests the data for namespace 2, the DA layer must provide the data chunks `D3`, `D4`, `D5`, and `D6` and the nodes `N2`, `N8` and `N7` as proof (note that the application already has the root `N14` from the block header).

As a result, the application is able to check that the provided data is part of the block data. Furthermore, the application can verify that all the data for namespace 2 was provided. If the DA layer provides for example only the data chunks `D4` and `D5`, it must also provide nodes `N12` and `N11` as proofs. However, the application can identify that the data is incomplete by checking the namespace range of the two nodes, i.e., both `N12` and `N11` have descendants part of namespace 2.

For more details on NMTs, take a look at the [original paper](https://arxiv.org/abs/1905.09274).

## Building a PoS Blockchain for DA

### Providing Data Availability

The Celestia DA layer consists of a PoS blockchain. Celestia is dubbing this blockchain as the [Celestia App](https://github.com/celestiaorg/celestia-app), an application that provides transactions to facilitate the DA layer and is built using [Cosmos SDK](https://docs.cosmos.network/v0.44/). The following figure shows the main components of Celestia App.

![Main components of Celestia App](/img/concepts/celestia-app.png)

Celestia App is built on top of [Celestia Core](https://github.com/celestiaorg/celestia-core), a modified version of the [Tendermint consensus algorithm](https://arxiv.org/abs/1807.04938). Among the more important changes to vanilla Tendermint, Celestia Core:

- Enables the erasure coding of block data (using the 2-dimensional Reed-Solomon encoding scheme).
- Replaces the regular Merkle tree used by Tendermint to store block data with a [Namespaced Merkle tree](https://github.com/celestiaorg/nmt) that enables the above layers (i.e., execution and settlement) to only download the needed data (for more details, see the section below describing use cases).

For more details on the changes to Tendermint, take a look at the [ADRs](https://github.com/celestiaorg/celestia-core/tree/v0.34.x-celestia/docs/celestia-architecture). Notice that Celestia Core nodes are still using the Tendermint p2p network.

Similarly to Tendermint, Celestia Core is connected to the application layer (i.e., the state machine) by [ABCI++](https://github.com/tendermint/tendermint/tree/master/spec/abci%2B%2B), a major evolution of [ABCI](https://github.com/tendermint/tendermint/tree/master/spec/abci) (Application Blockchain Interface).

The Celestia App state machine is necessary to execute the PoS logic and to enable the governance of the DA layer.

However, the Celestia App is data-agnostic -- the state machine neither validates nor stores the data that is made available by the Celestia App.
