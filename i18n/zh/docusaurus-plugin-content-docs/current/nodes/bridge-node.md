- - -
sidebar_label : 桥接节点
- - -

# 设置Celestia桥接节点

本教程将介绍如何设置 Celestia 桥接节点。

桥接节点连接数据可用性层和共识层，同时还可以选择成为验证者。

验证者也可以不用运行桥接结点，但是鼓励这样做，以便将区块发送给数据可用网络。

## 桥接节点概述

Celestia桥接节点具有以下属性：

1. 从共识网络中的受信任核心进程（意味着与 celestia-core 节点的受信RPC 连接）导入和处理“原始”区块头和块。 桥接节点可以在内部（嵌入式）运行此核心进程，也可以简单地连接到远程端点。 桥接节点还可以选择成为共识网络中的活跃验证者, 向 DA 网络中的轻节点提供带有数据可用性标头的块共享
2. 验证和擦除“原始”块的代码
3. 向 DA 网络中的轻节点提供带有数据可用性标头的块共享 ![桥接节点图](/img/nodes/BridgeNodes.png)

从实现的角度来看，桥接节点运行两个独立的进程：

1. 带有 Celestia Core 的 Celestia 应用程序（[参见 repo](https://github.com/celestiaorg/celestia-app)）

    * **Celestia App**是运行应用程序和权益证明逻辑的状态机。 Celestia App 基于 [Cosmos SDK](https://docs.cosmos.network/)构建，还包含**Celestia Core**。
    * **Celestia Core**是状态交互、共识和区块生产层。 Celestia Core 基于[Tendermint Core](https://docs.tendermint.com/)构建，经过修改以存储纠删码块的数据根以及其他更改（[请参阅 ADR](https://github.com/celestiaorg/celestia-core/tree/master/docs/celestia-architecture)）。

2. Celestia 节点（[见 repo](https://github.com/celestiaorg/celestia-node)）

    * **Celestia Node**通过一个单独的 libp2p 网络对上述内容进行了扩充，该网络服务于数据可用性采样请求， 该团队有时将其称为“光环”网络。

## 硬件要求

建议使用以下硬件最低要求来运行桥接节点：

* 内存: 8 GB RAM
* CPU：四核
* 磁盘：250 GB SSD 存储
* 带宽： 1 Gbps下载/100 Mbps上传

## 设置您的桥接节点

以下教程是在 Ubuntu Linux 20.04 (LTS) x64 实例机器上完成的。

### 设置依赖项

请按照[这里](../developers/environment.md)的步骤安装依赖项

## 部署Celestia桥接节点

### 安装Celestia节点

安装 Celestia Node 二进制文件，它将用于运行 Bridge 节点。

请按照[这里](../developers/celestia-node.md)的教程来安装Celestia节点。

### 初始化桥接节点

运行以下命令：

```sh
celestia bridge init --core.ip <ip-address> --core.rpc.port <port>
```

> NOTE: The `--core.rpc.port` defaults to 26657, so if you do not specify it in the command line, it will default to that port. 如果你喜欢，你可以使用flag来指定另一个端口。

如果您需要连接到的 RPC 端点列表，可以从[此处](./mamaki-testnet.md#rpc-endpoints)的列表中查看

### 运行桥接节点

启动桥接节点并连接到验证器节点的 gRPC 端点 (通常在端口 9090上显示)：

```sh
celestia bridge start --core.ip <ip-address> --core.grpc.port <port>
```

> NOTE: The `--core.grpc.port` defaults to 9090, so if you do not specify it in the command line, it will default to that port. 如果你喜欢，你可以使用flag来指定另一个端口。

如果您需要连接到的 RPC 端点列表，可以从[此处](./mamaki-testnet.md#rpc-endpoints)的列表中查看

您可以按照[这里](./keys.md)的`cel-key`指示步骤为您的节点创建密钥

启动桥接节点后，将为您生成一个钱包密钥。 你需要使用测试网代币为该地址注资，以支付 PayForData 交易。 您可以通过运行以下命令找到地址：

```sh
./cel-key list --node.type bridge --keyring-backend test
```

你有两个网络获取测试网代币：

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> NOTE: If you are running a bridge node for your validator it is highly recommended to request Mamaki testnet tokens as this is the testnet used to test out validator operations.

#### 可选：使用自定义密钥运行桥接节点

要使用自定义密钥运行桥接节点：

1. 自定义密钥必须存在于 celestia 桥接节点目录中的正确路径(默认: `~/.celestia-bridge/keys/keyring-test`)
2. 自定义密钥的名称必须在 `开始`时传递，就像这样：

```sh
celestia bridge start --core.ip <ip> --core.grpc.port 9090 --keyring.accname <name_of_custom_key>
```

### 可选：通过SystemD启动桥接节点

请按照[这里](./systemd.md#celestia-bridge-node)的教程，通过SystemD，将桥接节点设置为后台进程。

您已成功地设置了与网络同步的桥接节点。
