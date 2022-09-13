- - -
sidebar_label : 单片与模块化区块链
- - -

# 单片与模块化区块链

区块链实例化[复制状态机](https://dl.acm.org/doi/abs/10.1145/98163.98167)：无许可分布式网络中的节点将确定性事务的有序序列应用于初始状态，从而产生共同的最终状态。 这意味着区块链需要以下四个功能：

- __执行__ 需要执行正确更新状态的交易。 因此，执行必须确保仅执行有效交易，即导致有效状态机转换的交易。
- __解决__ 需要一个环境来验证执行层， 解决欺诈纠纷，以及其他执行层之间的桥梁。
- __共识__ 意味着就交易的顺序达成一致。
- __数据可用性__ (DA) 需要使交易数据可用。 请注意，执行、结算和共识需要 DA。

Traditional blockchains, i.e. _monolithic blockchains_, implement all four functions together in a single base consensus layer. The problem with monolithic blockchains is that the consensus layer must perform a lot of different tasks and it cannot be optimized for only one of these functions. As a result, the monolithic paradigm limits the throughput of the system.

![Modular VS Monolithic](/img/concepts/monolithic-modular.png)

As a solution, modular blockchains decouple these functions among multiple specialized layers as part of a modular stack. Due to the flexibility that specialization provides, there are many possibilities in which that stack can be arranged. For example, one such arrangement is the separation of the four functions into three specialized layers.

The base layer consists of DA and consensus and thus, is referred to as the Consensus and DA layer (or for brevity, the DA layer), while both settlement and execution are moved on top in their own layers. As a result, every layer can be specialized to optimally perform only its function and thus, increase the throughput of the system. Furthermore, this modular paradigm enables multiple execution layers, i.e., [rollups](https://vitalik.ca/general/2021/01/05/rollup.html), to use the same settlement and DA layers.
