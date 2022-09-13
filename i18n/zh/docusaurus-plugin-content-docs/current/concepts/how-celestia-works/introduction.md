# 介绍

Celestia 是一个模块化区块链网络，其目标是构建可扩展的[数据可用性层](https://blog.celestia.org/celestia-a-scalable-general-purpose-data-availability-layer-for-decentralized-apps-and-trust-minimized-sidechains/)，支持下一代可扩展的区块链架构—— [模块化区块链](https://celestia.org/learn/)。 Celestia 通过将[执行与共识分离](https://arxiv.org/abs/1905.09274)并引入新的原始[数据可用性采样](https://arxiv.org/abs/1809.09044)来扩展。

前者意味着 Celestia 只负责订购交易并保证其数据可用性；这类似于[减少对原子广播的共识](https://en.wikipedia.org/wiki/Atomic_broadcast#Equivalent_to_Consensus)。

后者通过仅要求资源有限的轻节点从每个块中采样少量随机块来验证数据可用性，为[数据可用性问题](https://coinmarketcap.com/alexandria/article/what-is-data-availability)提供了一种有效的解决方案。

有趣的是，更多参与采样的轻节点增加了网络可以安全处理的数据量，从而使区块大小能够增加，而不会同样增加验证链的成本。
