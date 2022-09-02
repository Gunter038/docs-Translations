- - -
sidebar_label : Celestia的数据可用层
- - -

# Celestia App交易的生命周期

用户通过Celestia App发送`PayForData`交易来实现数据可用性。 每个交易都由发送者的标识，需要实现可用性的数据（也被称为‘消息’），数据大小，命名空间标识，以及签名组成。 区块生产者把多个`PayForData`交易打包进区块。

在提议一个区块之前， 生产者通过ABCI++把区块发送给状态机，在那里，每个`PayForData`交易被拆分为带命名空间的消息（后文表示为`Msg`），它由数据和命名空间标识以及一个可执行交易（后文用`e-Tx`表示）组成。这个可执行的交易，不包含数据，而只是一个摘要，之后可以用来证明，数据确实是可用的。

因此，区块数据由划分到命名空间下面的数据和可执行交易组成。 请注意，区块被提交之后Celestia的状态机只会执行这种特定的交易。

![Lifecycle of a Celestia App Transaction](/img/concepts/tx-lifecycle.png)

接下来，区块生产者把区块数据的摘要添加到区块头中。 就像[这里](./data-availability-layer.md#fraud-proofs-of-incorrectly-extended-data)说的一样，这个摘要是4K个中间默克尔根（对应于扩展矩阵的一行或一列）的默克尔根。 为了计算这个摘要，区块生产者会执行以下操作：

- 可执行交易和带命名空间的数据被划分成多个份额。 每个份额由，以命名空间标识为前缀的字节数据构成。 最后，可执行交易会关联上相应的命名空间。
- 它把这些份额分配到方阵当中（按行）。 请注意，每个份额的长度会被补足到最接近的2的幂。 输出的大小为k x k的方阵被称为原始数据。
- 通过前文所述的二维Reed-Solomon编码，原始数据被扩展为2k x 2k的方阵。 扩展出来的份额（它包含纠删码）被关联到一个预留的命名空间。
- 利用上文所述的NMT（带命名空间的默克尔树），计算扩展以后的方阵的每个行和列的摘要。

因此，区块数据的摘要是默克尔树的根，而树叶是带命名空间的默克尔树的根，每个树叶代表扩展以后的方阵的一行或一列。

## 数据可用性的检查

![DA network](/img/concepts/consensus-da.png)

为增强连接性，Celestia的节点通过一个单独的libp2p网络和Celestia App连接，这被称为_DA网络_，它响应DAS（数据可用性采样）请求。

轻节点通过DA网络连接Celestia节点，监听被扩展的区块头（由区块头和DA元数据组成，比如4K个中间默克尔根），然后针对收到的区块头执行DAS（请求随机的数据块）

请注意，虽然被推荐，但执行DAS是可选的——轻节点可以简单地相信，区块头中的摘要对应的数据，确实在Celestia DA层中是可用的。 另外，轻节点也可以向Celestia App提交交易（就是`PayForData`交易）。

While performing DAS for a block header, every light node queries Celestia Nodes for a number of random data chunks from the extended matrix and the corresponding Merkle proofs. If all the queries are successful, then the light node accepts the block header as valid (from a DA perspective).

If at least one of the queries fails (i.e., either the data chunk is not received or the Merkle proof is invalid), then the light node rejects the block header and tries again later. The retrial is necessary to deal with false negatives, i.e., block headers being rejected although the block data is available. This may happen due to network congestion for example.

Alternatively, light nodes may accept a block header although the data is not available, i.e., a _false positive_. This is possible since the soundness property (i.e., if an honest light node accepts a block as available, then at least one honest full node will eventually have the entire block data) is probabilistically guaranteed (for more details, take a look at the [original paper](https://arxiv.org/abs/1809.09044)).

By fine tuning Celestia's parameters (e.g., the number of data chunks sampled by each light node) the likelihood of false positives can be sufficiently reduced such that block producers have no incentive to withhold the block data.
