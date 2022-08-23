- - -
sidebar_label : 桥接节点
- - -

# Setting up a Celestia Bridge Node

本教程将介绍如何设置 Celestia 桥接节点。

桥接节点连接数据可用性层和共识层，同时还可以选择成为验证者。

Validators do not have to run bridge nodes, but are encouraged to in order to relay blocks to the data availability network.

## Overview of bridge nodes

Celestia桥接节点具有以下属性：

1. Import and process “raw” headers & blocks from a trusted Core process (meaning a trusted RPC connection to a celestia-core node) in the Consensus network. Bridge Nodes can run this Core process internally (embedded) or simply connect to a remote endpoint. Bridge Nodes also have the option of being an active validator in the Consensus network. 桥接节点可以在内部（嵌入式）运行此核心进程，也可以简单地连接到远程端点。 桥接节点还可以选择成为共识网络中的活跃验证者, 向 DA 网络中的轻节点提供带有数据可用性标头的块共享
2. 验证和擦除“原始”块的代码
3. 向 DA 网络中的轻节点提供带有数据可用性标头的块共享 ![桥接节点图](/img/nodes/BridgeNodes.png)

从实现的角度来看，桥接节点运行两个独立的进程：

1. 带有 Celestia Core 的 Celestia 应用程序（[参见 repo](https://github.com/celestiaorg/celestia-app)）

    * **Celestia App**是运行应用程序和权益证明逻辑的状态机。 **Celestia App** is the state machine where the application and the proof-of-stake logic is run. Celestia App is built on [Cosmos SDK](https://docs.cosmos.network/) and also encompasses **Celestia Core**.
    * **Celestia Core**是状态交互、共识和区块生产层。 **Celestia Core** is the state interaction, consensus and block production layer. Celestia Core is built on [Tendermint Core](https://docs.tendermint.com/), modified to store data roots of erasure coded blocks among other changes ([see ADRs](https://github.com/celestiaorg/celestia-core/tree/master/docs/celestia-architecture)).

2. Celestia 节点（[见 repo](https://github.com/celestiaorg/celestia-node)）

    * **Celestia Node** augments the above with a separate libp2p network that serves data availability sampling requests. The team sometimes refer to this as the “halo” network. 该团队有时将其称为“光环”网络。

## Hardware requirements

建议使用以下硬件最低要求来运行桥接节点：

* 内存: 8 GB RAM
* CPU：四核
* 磁盘：250 GB SSD 存储
* 带宽： 1 Gbps下载/100 Mbps上传

## Setting up your bridge node

以下教程是在 Ubuntu Linux 20.04 (LTS) x64 实例机器上完成的。

### Setup the dependencies

请按照[这里](../developers/environment.md)的步骤安装依赖项

## Deploy the Celestia bridge node

### Install Celestia node

安装 Celestia Node 二进制文件，它将用于运行 Bridge 节点。

请按照[这里](../developers/celestia-node.md)的教程来安装Celestia节点。

### Initialize the bridge node

运行以下命令：

```sh
celestia bridge init --core.remote tcp://<ip-address>:26657
```

如果您需要连接到的 RPC 端点列表，可以从[此处](./mamaki-testnet.md#rpc-endpoints)的列表中查看

### Run the bridge node

启动桥接节点并连接到验证器节点的 gRPC 端点 (通常在端口 9090上显示)：

> 注意：为了获得获取/提交状态相关信息的能力，例如提交 PayForData 交易或查询节点账户余额的能力，验证者（核心）节点的 gRPC 端点必须按指示传递如下

```sh
celestia bridge start --core.grpc http://<ip>:9090
```

如果您需要连接到的 RPC 端点列表，可以从[此处](./mamaki-testnet.md#rpc-endpoints)的列表中查看

您可以按照[这里](./keys.md)的`cel-key`指示步骤为您的节点创建密钥

启动桥接节点后，将为您生成一个钱包密钥。 Once you start the Bridge Node, a wallet key will be generated for you. You will need to fund that address with Mamaki Testnet tokens to pay for PayForData transactions. You can find the address by running the following command: 您可以通过运行以下命令找到地址：

```sh
./cel-key list --node.type bridge --keyring-backend test
```

可以在这里请求 [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)测试网代币

#### Optional: run the bridge node with a custom key

要使用自定义密钥运行桥接节点：

1. 自定义密钥必须存在于 celestia 桥接节点目录中的正确路径(默认: `~/.celestia-bridge/keys/keyring-test`)
2. 自定义密钥的名称必须在 `开始`时传递，就像这样：

```sh
celestia bridge start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Optional: start the bridge node with SystemD

请按照[这里](./systemd.md#celestia-bridge-node)的教程，通过SystemD，将桥接节点设置为后台进程。

您已成功地设置了与网络同步的桥接节点。
