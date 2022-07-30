- - -
sidebar_label : 桥接节点
- - -

# 设置Celestia桥接节点

本教程将介绍如何设置 Celestia 桥接节点。

桥接节点连接数据可用性层和共识层，同时还可以选择成为验证者。

## 桥接节点概述

Celestia桥接节点具有以下属性：

1. 从共识网络中的受信任核心进程（意味着与 celestia-core 节点的受信RPC 连接）导入和处理“原始”区块头和块。 桥接节点可以在内部（嵌入式）运行此核心进程，也可以简单地连接到远程端点。 桥接节点还可以选择成为共识网络中的活跃验证者, 向 DA 网络中的轻节点提供带有数据可用性标头的块共享
2. 验证和擦除“原始”块的代码
3. 向 DA 网络中的轻节点提供带有数据可用性标头的块共享 ![bridge-node-diagram](/img/nodes/BridgeNodes.png)

从实现的角度来看，桥接节点运行两个独立的进程：

1. 带有 Celestia Core 的 Celestia 应用程序（[参见 repo](https://github.com/celestiaorg/celestia-app)）

    * Celestia App是运行应用程序和权益证明逻辑的状态机。**Celestia App** 基于 Cosmos SDK构建，还包含 Celestia Core。 Celestia App is built on [Cosmos SDK](https://docs.cosmos.network/) and also encompasses **Celestia Core**.
    * **Celestia Core** is the state interaction, consensus and block production layer. Celestia Core is built on [Tendermint Core](https://docs.tendermint.com/), modified to store data roots of erasure coded blocks among other changes ([see ADRs](https://github.com/celestiaorg/celestia-core/tree/master/docs/celestia-architecture)).

2. Celestia Node ([see repo](https://github.com/celestiaorg/celestia-node))

    * **Celestia Node** augments the above with a separate libp2p network that serves data availability sampling requests. The team sometimes refer to this as the “halo” network.

## Hardware Requirements

The following hardware minimum requirements are recommended for running the bridge node:

* Memory: 8 GB RAM
* CPU: Quad-Core
* Disk: 250 GB SSD Storage
* Bandwidth: 1 Gbps for Download/100 Mbps for Upload

## Setting Up Your Bridge Node

The following tutorial is done on an Ubuntu Linux 20.04 (LTS) x64 instance machine.

### Setup The Dependencies

Follow the tutorial here installing the dependencies [here](../developers/environment.md).

## Deploy the Celestia Bridge Node

### Install Celestia Node

Install the Celestia Node binary, which will be used to run the Bridge Node.

Follow the tutorial for installing Celestia Node [here](../developers/celestia-node.md).

### Initialize the Bridge Node

Run the following:

```sh
celestia bridge init --core.remote tcp://<ip-address>:26657 
```

If you need a list of RPC endpoints to connect to, you can check from the list [here](./mamaki-testnet.md#rpc-endpoints)

### Run the Bridge Node

Start the Bridge Node with a connection to a validator node's gRPC endpoint (which is usually exposed on port 9090):

> NOTE: In order for access to the ability to get/submit state-related information, such as the ability to submit PayForData transactions, or query for the node's account balance, a gRPC endpoint of a validator (core) node must be passed as directed below._

```sh
celestia bridge start --core.grpc http://<ip>:9090
```

If you need a list of RPC endpoints to connect to, you can check from the list [here](./mamaki-testnet.md#rpc-endpoints)

You can create your key for your node by following the `cel-key` instructions [here](./keys.md)

Once you start the Bridge Node, a wallet key will be generated for you. You will need to fund that address with Mamaki Testnet tokens to pay for PayForData transactions. You can find the address by running the following command:

```sh
./cel-key list --node.type bridge --keyring-backend test
```

Mamaki Testnet tokens can be requested [here](./mamaki-testnet.md#mamaki-testnet-faucet).

#### Optional: Run the Bridge Node with a Custom Key

In order to run a bridge node using a custom key:

1. The custom key must exist inside the celestia bridge node directory at the correct path (default: `~/.celestia-bridge/keys/keyring-test`)
2. The name of the custom key must be passed upon `start`, like so:

```sh
celestia bridge start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Optional: Start the Bridge Node with SystemD

Follow the tutorial on setting up the bridge node as a background process with SystemD [here](./systemd.md#celestia-bridge-node).

You have successfully set up a bridge node that is syncing with the network.
